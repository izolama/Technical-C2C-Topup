# Technical-C2C-Topup
[![views](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fizolama%2FTechnical-C2C-Topup%2Fedit%2Fmain%2FREADME.md&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=views&edge_flat=false)](https://hits.seeyoufarm.com)

### Table of Contents
**[Function SingleTonAar](#singleton)**<br>
**[Function Get Deposit Balance](#deposit-balance)**<br>
**[Function Get Balance E-money](#get-balance)**<br>
**[Function Topup](#topup)**<br>
**[Function On Success](#function-on-success)**<br>
**[Function On Failed](#function-on-failed)**<br>

## Singleton

```java
 // open library
 private void initializeReader() {
        AarDeviceId aarDeviceId = new AarDeviceId(appContext);
        String deviceId = null;
        try {
            deviceId = aarDeviceId.getDeviceId();
        } catch (DeviceNotRegisteredException e) {
            String accessToken = "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJhdXRoLXNlcnZpY2U6MS4wLjAiLCJzdWIiOnsibmFtZSI6IlRyYW5zSmFrYXJ0YSIsInVzZXJuYW1lIjoidGoiLCJhdXRoX2xldmVsIjoiTUVSQ0hBTlRfR1JPVVAiLCJtaWQiOiJUSiJ9LCJpYXQiOjE1ODkxMjc5NTksImV4cCI6MTc0NjkxMjcxOSwibmJmIjoxNTg5MTI3OTYxfQ.YmCfzXR3j2B9LDogPUKgF32OD4d3hNucrQeadM7tywu3JHB31xvlVaqfzmzBdBgm8Xn8s4u20_6tGQCQ9rTq62UGWIoMr2Tq_sUmcRoX61N_QfgCJVbamtmDh_lGcpwQIzm1782kJsMYQRLRCk7bYfTt6RCpbNJzwLJIrFDHNeAEhnNs7L3-v4PFmIf4hbjpmTqPlWZIMvJrBG8SUdTdsmlzehus--XGzU2l4y5CIj6m09qfxZluRP45KFlHLbPln4DIac-8wpKhDYx3gSiIuacCI4dWIc837GuC8yDyr3TmIz4VV7PsjEl_NmQ6tEof2CyBy-L3NJRqLluGRGtA0w";
            aarDeviceId.init(accessToken, DeviceEnvironment.PROD);
        }

        InitLibraryTask initLibraryTask = new InitLibraryTask();
        initLibraryTask.execute(this.appContext);
    }
```
>Metode Java yang disebut initializeReader()
Membuat instance dari kelas AarDeviceId dengan menggunakan appContext sebagai argumen konstruktor.
AarDeviceId adalah sebuah kelas yang digunakan untuk menginisialisasi perangkat yang akan digunakan untuk membaca sesuatu (mungkin merupakan perangkat keras atau perangkat lunak).
Dalam kode ini, appContext digunakan sebagai konteks aplikasi untuk inisialisasi AarDeviceId.

##

```java
  @Override
        protected Void doInBackground(Context... contexts) {
            final Context mContext = contexts[0];
            SharedPreferenceAuthTokenRepository authTokenRepository = new SharedPreferenceAuthTokenRepository(mContext);

            nativeLib nativeLibrary;
            reader = new InitReader();
            AarDeviceId deviceId = new AarDeviceId(mContext);
            try {
                String ss = deviceId.getDeviceId();
            } catch (DeviceNotRegisteredException e) {
                deviceId.init(authTokenRepository.getToken(), DeviceEnvironment.PROD);
            }

            InitDebugCertificate initDebugCertificate = new InitDebugCertificate();
            // close init library

            try {
                nativeLibrary = new nativeLib(mContext, engineType);
                reader = initDebugCertificate.getDebug(nativeLibrary, mContext, engineType, new InitConfig() {
                    @Override
                    public ApiH2HConfig getH2HConfig() {
                        return new ProductionApiH2HConfig(mContext);
                    }

                    @Override
                    public MandiriTopupCardToCardConfig getConfigMandiriTopupC2C() {
                        return new MandiriCardToCardProduction();

                    }

                    @Override
                    public BniTopupCardToCardConfig getConfigBniTopupC2C() {
                        return new BniCardToCardProduction();
                    }

                    @Override
                    public DeductConfigBca getDeductConfigBca() {
                        return new DevDeductConfigBca();
                    }

                    @Override
                    public DeductConfigBri getDeductConfigBri() {
                        return new DevDeductConfigBri();
                    }

                    @Override
                    public DeductConfigMandiri getDeductConfigMandiri() {
                        return new DevDeductConfigMandiri();
                    }

                    @Override
                    public DeductConfigBni getDeductConfigBni() {
                        return new DevDeductConfigBni();
                    }

                    @Override
                    public DeductConfigDki getDeductConfigDki() {
                        return new DummyBankDkiConfig();
                    }

                    @Override
                    public String getAccessToken() {
                        return authTokenRepository.getToken();
                    }

                }, false);
            } catch (Exception e) {
                err = e.getMessage();
            }
            return null;
        }
```
>Kode yang diberikan adalah implementasi dari metode doInBackground() dalam kelas InitLibraryTask. Metode ini merupakan bagian dari AsyncTask, yang biasanya digunakan untuk menjalankan tugas latar belakang dalam aplikasi Android.

##

contoh penggunaan singleTonAar
```java
SingletonAar instance = SingletonAar.getInstance(context);
```
##

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>

## Deposit Balance

sebuah proses untuk mendapatkan saldo deposit menggunakan fitur kartu ke kartu (card-to-card) pada suatu aplikasi Android. Berikut adalah penjelasan singkat tentang apa yang dilakukan oleh metode `getDepositBalance();`

```java
 @Override
    public void getDepositBalance() {
        Log.i(TAG, "Get Deposit Balance action=getDepositBalance status=initiate");
        try {
            closeReader();
        } catch (Exception e) {
            e.printStackTrace();
        }
        final Handler handler = new Handler(Looper.getMainLooper());
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                reinitReader(new InitListener() {
                    @Override
                    public void onInitReaderComplete() {
                        final Handler handler = new Handler(Looper.getMainLooper());
                        cardToCardProcessor.getBalanceCardDeposit(cardToCardCallback);
                    }

                    @Override
                    public void onInitReaderFailed(String reason) {
                        view.dismissProgress();
                        view.showErrorMessage(reason);

                    }
                });
            }
        }, 500);

    }
```

##

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>

## Get Balance 
function get balance e-money . Metode ini digunakan untuk mengambil informasi saldo dari suatu kartu menggunakan perangkat pembaca kartu (card reader) berdasarkan logika bisnis yang ada dalam kode tersebut

```java
    public void getBalance() {
        int Tag = 8;
        String message = "Failed get Balance";
        byte[] uid = new byte[16];
        byte[] uidLen = new byte[1];
        int[] cardType = new int[1];
        if (reader.myReader.findCard(5000, uid, uidLen, cardType)) {
            final long start = System.currentTimeMillis();
            final int[] bankType = new int[1];
            final int[] balance = new int[1];
            final String[] cardNumber = new String[1];
            final int[] errorCode = new int[1];
            Date date = new Date();
            String StrDate = new SimpleDateFormat("yyyyMMddHHmmss", Locale.getDefault()).format(date);
            try {
                if (reader.myReader.readerGetBalance(cardType[0], StrDate, bankType, balance, cardNumber, errorCode)) {
                    final long stop = System.currentTimeMillis();
                    String cardBalanceResp = String.valueOf(balance[0]);
                    String cardNumberResp = cardNumber[0];
                    makeToast("Balance " + cardBalanceResp);
                    Log.d("TopupC2CTransactionPresenter", "initialBalance = " + currentAppTransaction.CardInitialBalance);
                    Log.d("TopupC2CTransactionPresenter", "getBalanceAfterError = " + cardBalanceResp.replace("Rp. ", "").replace(".", ""));
                    if (currentAppTransaction.CardNumber.equalsIgnoreCase(cardNumberResp)) {
                        if (!currentAppTransaction.CardInitialBalance.equalsIgnoreCase(cardBalanceResp.replace("Rp. ", "").replace(".", ""))) {
                            currentAppTransaction.CardLastBalance = cardBalanceResp;
                            forceSettlementTopup(false, false, true);
                        } else {
                            forceSettlementTopup(true, false, true);
                        }
                    } else {
                        forceSettlementTopup(true, true, true);
                    }

                } else {

                    showTopupFailDialogWhenBalanceReduced(message, Tag);
                    showTopupFailDialogWhenCorrectionOrForceNeeded(message, Tag);
                    Log.d(TAG, "TAG: " + CardToCardConst.ConstToString(Tag) + " message: " + message);
                    IS_TRX_SUCCESS = false;
                    makeToast(message);
                    ProgressDialogHelper.dismissProgressDialog();
                    view.displayRetryButton();
                }
            } catch (Exception e) {
                ProgressDialogHelper.dismissProgressDialog();
                showTopupFailDialogWhenBalanceReduced(message, Tag);
                showTopupFailDialogWhenCorrectionOrForceNeeded(message, Tag);
                Log.d(TAG, "TAG: " + CardToCardConst.ConstToString(Tag) + " message: " + message);
                IS_TRX_SUCCESS = false;
                makeToast(message);
                view.displayRetryButton();
            }
        } else {
            ProgressDialogHelper.dismissProgressDialog();
            showTopupFailDialogWhenBalanceReduced(message, Tag);
            showTopupFailDialogWhenCorrectionOrForceNeeded(message, Tag);
            Log.d(TAG, "TAG: " + CardToCardConst.ConstToString(Tag) + " message: " + message);
            IS_TRX_SUCCESS = false;
            makeToast(message);
            view.displayRetryButton();

        }
    }
```

##

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>

## Topup

ini adalah bagian dari suatu metode dalam sebuah aplikasi yang bertanggung jawab untuk melakukan proses top-up (pengisian saldo) ke suatu kartu menggunakan metode card-to-card.

```java
 public void topup() {
        view.hideRetryButton();
        tryCorrection = 0;
        CardBlacklistDao cardBlacklistDao = LocalDatabase.getInstance(context).cardBlacklistDao();
        AsyncTask.execute(new Runnable() {
            @Override
            public void run() {
                List<CardBlacklist> cardBlacklists = cardBlacklistDao.findCardBlacklist();
                BniTopupCardToCardConfig bniTopupCardToCardConfig = new BniCardToCardProduction();
                cardToCardProcessor.topup(currentAppTransaction, currentDenom, cardToCardCallback, cardBlacklists, bniTopupCardToCardConfig, BuildConfig.DEVICE_DRIVER, cardToCardProcessor);
            }
        });
    }
```
##

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>

## Function On Success 

Example Function success , sebuah metode yang dijalankan ketika proses top-up (pengisian saldo) sukses dalam suatu aplikasi. Metode ini menghandle beberapa tag (Tag) yang mungkin diberikan sebagai parameter untuk menentukan aksi apa yang akan diambil berdasarkan tag yang diterima.

```java
    public void onSuccess(String result, int Tag) {
        Log.d("TopupC2CTransactionPresenter", "Success card to card presenter result=" + result + "tag=" + Tag);
        if (Tag == MANDIRI_BALANCE) {
            String cardBalanceResp = result.split(",")[0];
            String cardNumberResp = result.split(",")[1];
            makeToast("Balance " + cardBalanceResp);
            AsyncTask.execute(new Runnable() {
                @Override
                public void run() {
                    Log.d("TopupC2CTransactionPresenter", "initialBalance = " + currentAppTransaction.CardInitialBalance);
                    Log.d("TopupC2CTransactionPresenter", "getBalanceAfterError = " + cardBalanceResp.replace("Rp. ", "").replace(".", ""));
                    if (!currentAppTransaction.CardInitialBalance.equalsIgnoreCase(cardBalanceResp.replace("Rp. ", "").replace(".", "")) && currentAppTransaction.CardNumber.equalsIgnoreCase(cardNumberResp)) {
                        currentAppTransaction.CardLastBalance = cardBalanceResp;
                        forceSettlementTopup(false, false, true);
                    } else {
                        forceSettlementTopup(true, false, true);
                    }
                }
            });
        }

        if (Tag == MANDIRI_TOPUP || Tag == MANDIRI_CORRECTION) {
            makeToast("success topup c2c");
            Log.d(TAG, "trx " + new Gson().toJson(currentAppTransaction));
            if (Tag == MANDIRI_CORRECTION)
                PreferenceManagers.setDataWithSameKey("finishCorrection", "true", context);
            Log.e("successc2c", new Gson().toJson(currentAppTransaction));


            if (SingletonAar.getInstance(context).getReader().myReader != null)
                SingletonAar.getInstance(context).getReader().myReader.closeReader();

            view.transactionSuccess(currentAppTransaction);
        }
        
        if (Tag == MANDIRI_DEPOSIT_BALANCE) {
            int depositBalance = Integer.parseInt(result);
            Log.d(TAG, "SALDO DEPOSIT: " + new CurrencyHelper().format(depositBalance));
            ProgressDialogHelper.dismissProgressDialog();

        } else if (Tag == MANDIRI_GET_FORCE_SETTLEMENT_OLD || Tag == MANDIRI_GET_FORCE_SETTLEMENT_NEW) {
            IS_TRX_SUCCESS = true;
            makeToast("Force settlement berhasil");
            PreferenceManagers.setDataWithSameKey("finishCorrection", "true", context);
            Log.e("ForceSettlement", new Gson().toJson(currentAppTransaction));

            if (SingletonAar.getInstance(context).getReader().myReader != null)
                SingletonAar.getInstance(context).getReader().myReader.closeReader();
            ProgressDialogHelper.dismissProgressDialog();
            view.transactionSuccess(currentAppTransaction);
        } else if (Tag == ON_CARD_DETECTED) {
            view.cardFound();
        }
    }
```
##

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>

## Function On Failed

Example Function failed

Ini adalah metode callback onFailed() yang mungkin merupakan bagian dari kelas presenter dalam suatu proyek Android. Metode ini menerima dua parameter: message, yang merupakan pesan error atau pesan yang menunjukkan kegagalan dalam operasi yang sedang dilakukan, dan Tag, yang digunakan untuk mengidentifikasi jenis operasi yang gagal.

```java
@Override
    public void onFailed(String message, int Tag) {
        ProgressDialogHelper.dismissProgressDialog();
        showTopupFailDialogWhenBalanceReduced(message, Tag);
        showTopupFailDialogWhenCorrectionOrForceNeeded(message, Tag);
        Log.d(TAG, "TAG: " + CardToCardConst.ConstToString(Tag) + " message: " + message);
        if (Tag == MANDIRI_CORRECTION || Tag == ARG_FAILED_DIFFERENT_CARD) {
            boolean isDepositBalanceReduced = topupFailDialog != null;
            if (isDepositBalanceReduced) {
                if (Tag != ARG_FAILED_DIFFERENT_CARD)
                    topupFailDialog.reduceLimit();
                topupFailDialog.setMessage(message);
                topupFailDialog.show();
            }

        }

        if (Tag == MANDIRI_GET_FORCE_SETTLEMENT_NEW) {
            IS_TRX_SUCCESS = true;
            PreferenceManagers.setDataWithSameKey("finishCorrection", "true", context);
            Log.e("ForceSettlement", new Gson().toJson(currentAppTransaction));

            if (SingletonAar.getInstance(context).getReader().myReader != null)
                SingletonAar.getInstance(context).getReader().myReader.closeReader();
            ProgressDialogHelper.dismissProgressDialog();
            view.transactionSuccess(currentAppTransaction);
        } else {
            IS_TRX_SUCCESS = false;
            makeToast(message);
            view.displayRetryButton();
        }
    }
```

**[Up To Table Of Contents 🔝 ](#table-of-contents)**<br>
