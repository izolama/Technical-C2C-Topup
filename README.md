# Technical-C2C-Mandiri

Topup C2C Home Fragment 

```java
 @Override
    public void displayHomeTopupC2C() {
        Log.d("MainActivity", "display home topupC2C");
        try {
            SingletonAar.getInstance(this).getReader().myReader.readerMandiriSetSamSlot(OrganicDriver.ORGANIC_SAM2);
            SingletonAar.getInstance(this).getReader().myReader.readerMandiriSetSamC2CSlot(OrganicDriver.ORGANIC_SAM2);
            SingletonAar.getInstance(this).getReader().myReader.readerMandiriSetKaC2CSlot(OrganicDriver.ORGANIC_SAM1);
        } catch (Exception e) {
            e.printStackTrace();
        }
        topBar.setVisibility(View.VISIBLE);
        stateDisplay = DISPLAY_HOME;
        getSupportFragmentManager()
                .beginTransaction()
                .replace(
                        R.id.activity_main_fl_fragmentContainer,
                        new TopupC2CHomeFragment(), "HOME"
                )
                .commit();
    }
```
##
<sub> 
 Kode Java di atas adalah implementasi dari metode displayHomeTopupC2C() yang digunakan untuk menampilkan tampilan beranda (home) dari fitur top-up C2C (Card-to-Card) dalam sebuah aplikasi Android. Berikut adalah penjelasan kode tersebut dalam bahasa Indonesia:
Kode ini mencatat pesan debug menggunakan Log.d() dengan tag "MainActivity" dan pesan "display home topupC2C".
Kode ini mencoba untuk melakukan beberapa operasi terkait objek Singleton bernama SingletonAar. Metode getInstance() dipanggil dengan this sebagai argumen, yang mengacu pada objek saat ini. Kemudian, beberapa metode dipanggil pada objek myReader yang diperoleh dari SingletonAar.getInstance(this).getReader().myReader.
Dalam blok try, metode readerMandiriSetSamSlot() dipanggil dengan nilai konstan OrganicDriver.ORGANIC_SAM2 sebagai argumen. Kemudian, readerMandiriSetSamC2CSlot() dipanggil dengan nilai konstan yang sama. Terakhir, readerMandiriSetKaC2CSlot() dipanggil dengan nilai konstan OrganicDriver.ORGANIC_SAM1 sebagai argumen.
Jika terjadi exception saat menjalankan operasi di atas, akan ditangkap di blok catch dan stack trace akan dicetak menggunakan e.printStackTrace().
topBar diatur untuk menjadi terlihat menggunakan topBar.setVisibility(View.VISIBLE). Visibilitas topBar diubah menjadi View.VISIBLE, yang artinya akan ditampilkan.
stateDisplay diperbarui dengan nilai DISPLAY_HOME.
Dilakukan transaksi fragment menggunakan metode getSupportFragmentManager().beginTransaction(). Instance baru dari TopupC2CHomeFragment dibuat dan digantikan dalam wadah fragment dengan ID R.id.activity_main_fl_fragmentContainer. Transaksi tersebut dikonfirmasi dengan commit() method.
Secara keseluruhan, kode ini mengatur tampilan untuk fitur top-up C2C (Card-to-Card) pada halaman beranda (home) dalam aplikasi Android dengan melakukan operasi terkait objek Singleton, memperbarui visibilitas suatu tampilan, dan menggantikan sebuah fragment dalam wadah fragment.
</sub>

##

Topup C2C Transaction Fragment 

```java
 @Override
    public void displayTopupC2CTransaction(int paidAmount) {

        topBar.setVisibility(View.VISIBLE);

        stateDisplay = DISPLAY_PAYMENT;
        getSupportFragmentManager()
                .beginTransaction()
                .replace(
                        R.id.activity_main_fl_fragmentContainer,
                        TopupC2CTransactionFragment.newInstance(paidAmount)
                )
                .commit();
    }
```
<sub> 
Metode "displayTopupC2CTransaction" ini merupakan sebuah override method yang diimplementasikan dalam sebuah kelas, mungkin dalam konteks sebuah aplikasi berbasis Android atau Java. Fungsinya adalah untuk menampilkan transaksi top-up peer-to-peer (C2C) dengan jumlah yang telah dibayar ke dalam suatu fragmen.
Mengatur tampilan top bar (atau bilah atas) menjadi terlihat (VISIBLE).
Mengatur variabel stateDisplay menjadi DISPLAY_PAYMENT, yang mungkin digunakan untuk mengatur status atau tampilan lainnya dalam aplikasi.
Memulai sebuah transaksi fragment menggunakan getSupportFragmentManager() untuk mengelola tampilan fragment dalam aktivitas.
Mengganti konten yang ada di dalam container fragment (R.id.activity_main_fl_fragmentContainer) dengan sebuah instance dari kelas TopupC2CTransactionFragment yang baru, yang akan dibuat dengan menggunakan metode "newInstance" dan akan menerima jumlah pembayaran (paidAmount) sebagai argumen.
Menyelesaikan transaksi fragment dengan melakukan commit() untuk mengaplikasikan perubahan ke tampilan.
Dengan demikian, metode "displayTopupC2CTransaction" ini bertujuan untuk menampilkan transaksi top-up peer-to-peer (C2C) ke dalam sebuah fragmen dengan menggunakan nilai jumlah pembayaran (paidAmount) yang diterima sebagai argumen.
</sub>
