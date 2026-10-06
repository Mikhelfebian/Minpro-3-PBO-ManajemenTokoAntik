# Dokumentasi Program Mini Project 2 PBO
## Sistem Manajemen Inventaris Toko Barang Antik (Monarch Antiqu'e)

---

### Informasi Mahasiswa
* **Nama** : Mikhel Febian
* **NIM** : 2509116056
* **Kelas** : B
* **Angkatan** : 2025
* **Mata Kuliah** : Pemrograman Berbasis Objek
* **Tema Program** : Sistem Penjualan dan Inventaris Barang Antik
* **Nama Sistem** : Monarch Antiqu'e System
* **Repository** : [Minpro-2-PBO-ManajemenTokoAntik](https://github.com/Mikhelfebian/Minpro-2-PBO-ManajemenTokoAntik/tree/main)

---

### 1. Deskripsi Singkat Program
Monarch Antiqu'e System adalah aplikasi pengelolaan inventaris barang antik berbasis *Command Line Interface* (CLI) yang dikembangkan menggunakan bahasa pemrograman Java. Program ini dirancang untuk memudahkan admin etalase toko dalam mengelola data koleksi barang antik dan perhiasan melalui operasi *Create, Read, Update, dan Delete* (CRUD).

Pada versi Mini Project 2 ini, arsitektur sistem telah disempurnakan dengan menerapkan prinsip-prinsip Pemrograman Berbasis Objek (*Object-Oriented Programming*) meliputi **Encapsulation**, **Inheritance**, **Polymorphism**, serta pemisahan struktur package berdasarkan pola arsitektur **Model-View-Controller (MVC)**.

---

### 2. Spesifikasi Lingkungan Pengembangan
* **Bahasa Pemrograman** : Java (JDK 17+)
* **Integrated Development Environment (IDE)** : Apache NetBeans
* **Struktur Data Memory** : `java.util.ArrayList`
* **Arsitektur Program** : Model-View-Controller (MVC)

---

### 3. Struktur Entitas dan Penerapan Inheritance

Sistem mengimplementasikan prinsip *Inheritance* (Pewarisan) dengan `Barang` sebagai *Superclass* serta `BarangAntik` dan `BarangPerhiasan` sebagai *Subclass*.

```mermaid
classDiagram
    direction BT

    class Barang {
        <<Superclass>>
        #int id
        #String namaBarang
        #double harga
        #int stok
    }

    class BarangAntik {
        <<Subclass 1>>
        -String asalNegara
        -int tahunPembuatan
    }

    class BarangPerhiasan {
        <<Subclass 2>>
        -String material
        -double beratGram
    }

    BarangAntik --|> Barang : extends
    BarangPerhiasan --|> Barang : extends
```

#### 3.1 Superclass: `Barang`
Menampung atribut dan perilaku umum yang dimiliki oleh seluruh entitas barang di toko.

| Modifikator Akses | Tipe Data | Nama Atribut | Deskripsi |
| :--- | :--- | :--- | :--- |
| `protected` | `int` | `id` | Identifikasi unik barang (*auto-increment*). |
| `protected` | `String` | `namaBarang` | Nama barang antik atau perhiasan. |
| `protected` | `double` | `harga` | Nominal harga barang dalam satuan Rupiah. |
| `protected` | `int` | `stok` | Ketersediaan jumlah unit barang. |

#### 3.2 Subclass 1: `BarangAntik` (Extends `Barang`)
Menampung data barang antik umum dengan atribut spesifik histori.

| Modifikator Akses | Tipe Data | Nama Atribut | Deskripsi |
| :--- | :--- | :--- | :--- |
| `private` | `String` | `asalNegara` | Negara asal ditemukannya objek antik. |
| `private` | `int` | `tahunPembuatan` | Tahun pembuatan atau estimasi usia barang. |

#### 3.3 Subclass 2: `BarangPerhiasan` (Extends `Barang`)
Menampung data barang koleksi perhiasan antik dengan atribut spesifik material fisik.

| Modifikator Akses | Tipe Data | Nama Atribut | Deskripsi |
| :--- | :--- | :--- | :--- |
| `private` | `String` | `material` | Jenis material pembuat perhiasan (Emas, Perak, dsb). |
| `private` | `double` | `beratGram` | Berat fisik perhiasan dalam satuan gram. |

---

### 4. Alur Kerja Sistem
```mermaid
flowchart TD
    %% Inisialisasi
    Start([Start Program]) --> LoadData[Load Pre-defined Dummy Data ke ArrayList]
    LoadData --> MenuJunction(( ))

    %% Menu Utama
    MenuJunction --> ShowMenu[/Tampilkan Menu Utama:<br/>1. Tambah Barang<br/>2. Tampilkan Semua Barang<br/>3. Cari Barang berdasarkan ID<br/>4. Update Barang<br/>5. Hapus Barang<br/>6. Keluar/]
    ShowMenu --> ReadMenu[/Input Pilihan Menu 1-6/]
    ReadMenu --> CheckMenu{Pilihan Menu?}

    %% Fitur 1: Tambah
    CheckMenu -->|1| M1_Select[/Input Jenis Barang:<br/>1. Barang Antik<br/>2. Perhiasan/]
    M1_Select --> M1_Input[/Input Atribut Umum & Atribut Spesifik Subclass/]
    M1_Input --> M1_Process[Instansiasi Objek Subclass & Tambah ke ArrayList]
    M1_Process --> M1_Print[/Tampilkan Pesan: Data Berhasil Ditambahkan/]
    M1_Print --> MenuJunction

    %% Fitur 2: Tampilkan
    CheckMenu -->|2| M2_Fetch[Ambil Seluruh Data Objek dari ArrayList]
    M2_Fetch --> M2_Print[/Tampilkan Daftar Barang dalam Format Tabel/]
    M2_Print --> MenuJunction

    %% Fitur 3: Cari
    CheckMenu -->|3| M3_Input[/Input ID Barang/]
    M3_Input --> M3_Search[Cari Objek berdasarkan ID pada ArrayList]
    M3_Search --> M3_Check{Barang Ditemukan?}
    M3_Check -->|Ya| M3_PrintSuccess[/Tampilkan Rincian Detail Barang/]
    M3_Check -->|Tidak| M3_PrintFail[/Tampilkan Pesan: ID Tidak Ditemukan/]
    M3_PrintSuccess --> MenuJunction
    M3_PrintFail --> MenuJunction

    %% Fitur 4: Update
    CheckMenu -->|4| M4_InputID[/Input ID Barang yang Ingin Diupdate/]
    M4_InputID --> M4_Search[Cari Objek berdasarkan ID pada ArrayList]
    M4_Search --> M4_Check{Barang Ditemukan?}
    M4_Check -->|Ya| M4_InputData[/Input Data Atribut Baru/]
    M4_InputData --> M4_Update[Update Nilai Atribut Objek via Setter/]
    M4_Update --> M4_PrintSuccess[/Tampilkan Pesan: Data Berhasil Diupdate/]
    M4_Check -->|Tidak| M4_PrintFail[/Tampilkan Pesan: ID Tidak Ditemukan/]
    M4_PrintSuccess --> MenuJunction
    M4_PrintFail --> MenuJunction

    %% Fitur 5: Hapus
    CheckMenu -->|5| M5_InputID[/Input ID Barang yang Ingin Dihapus/]
    M5_InputID --> M5_Search[Cari Objek berdasarkan ID pada ArrayList]
    M5_Search --> M5_Check{Barang Ditemukan?}
    M5_Check -->|Ya| M5_Confirm[/Input Konfirmasi Penghapusan y/n/]
    M5_Confirm --> M5_ConfirmCheck{Konfirmasi 'y'?}
    M5_ConfirmCheck -->|Ya| M5_Delete[Hapus Objek dari ArrayList]
    M5_Delete --> M5_PrintSuccess[/Tampilkan Pesan: Data Berhasil Dihapus/]
    M5_ConfirmCheck -->|Tidak| M5_PrintCancel[/Tampilkan Pesan: Penghapusan Dibatalkan/]
    M5_Check -->|Tidak| M5_PrintFail[/Tampilkan Pesan: ID Tidak Ditemukan/]
    M5_PrintSuccess --> MenuJunction
    M5_PrintCancel --> MenuJunction
    M5_PrintFail --> MenuJunction

    %% Exception Handling & Exit
    CheckMenu -->|Pilihan Invalid| MErr_Print[/Tampilkan Pesan Error: Pilihan Tidak Valid/]
    MErr_Print --> MenuJunction

    CheckMenu -->|6| M6_Print[/Tampilkan Pesan: Terima Kasih, Program Selesai/]
    M6_Print --> End([End Program])
```

1. **Inisialisasi Data (`Read Pre-loaded Data`)** :
   Sistem secara otomatis mengisikan beberapa data awal (*dummy data*) ke dalam `ArrayList` saat aplikasi dijalankan, sehingga fitur penampilan data langsung dapat diuji tanpa pengisian dari awal.
2. **Tambah Barang (`Menu 1`)** :
   Pengguna memilih tipe entitas yang ingin ditambahkan (`BarangAntik` atau `BarangPerhiasan`). Sistem meminta input detail sesuai tipe dengan disertai contoh format (*hint*).
3. **Tampilkan Semua Barang (`Menu 2`)** :
   Sistem menampilkan daftar barang dalam tabel terformat beserta detail atribut khusus dari masing-masing tipe kelas.
4. **Cari Barang Berdasarkan ID (`Menu 3`)** :
   Pengguna menginput ID barang. Sistem melakukan pencarian linier dan menampilkan entitas jika ditemukan.
5. **Update Barang (`Menu 4`)** :
   Sistem menampilkan data yang tersimpan berdasarkan ID, lalu menerima input pembaruan atribut umum dan atribut khusus subclass.
6. **Hapus Barang (`Menu 5`)** :
   Sistem meminta masukan ID barang yang akan dihapus, lalu meminta konfirmasi penghapusan (`y/n`) sebelum memuat operasi hapus.

---

### 5. Penjelasan Penerapan Prinsip PBO Wajib

#### 5.1 Encapsulation dan Access Modifier
* Seluruh variabel instans dikapsulasi ketat dengan modifikator `protected` pada *Superclass* dan `private` pada *Subclass*.
* Akses maupun pembacaan variabel dikendalikan melalui metode *Getter* dan *Setter*.
* Metode *Setter* dilengkapi dengan validasi data internal (misal: penolakan string kosong dan angka bernilai negatif dengan melempar `IllegalArgumentException`).

#### 5.2 Inheritance
* Implementasi hirarki dilakukan dengan membuat *Superclass* `Barang.java` yang diturunkan kepada dua *Subclass* yaitu `BarangAntik.java` dan `BarangPerhiasan.java`.
* *Subclass* menggunakan kata kunci `super` pada konstruktor untuk memanggil konstruktor dari *Superclass*.

#### 5.3 Validasi Input Usability
* Penanganan masukan pengguna terpusat pada kelas utilitas `Validator.java` untuk mengantisipasi *exception* masukan tipe data salah (seperti `NumberFormatException`).
* Setiap prompt instruksi inputan pada `MainView.java` dilengkapi dengan contoh format masukan (*hint*) untuk memperjelas ekspektasi masukan bagi pengguna.

---

### 6. Penjelasan Penerapan Nilai Tambah

#### 6.1 Arsitektur Model-View-Controller (MVC)
Kode program dipisahkan secara modular ke dalam struktur *package* terorganisir berikut:

```text
com.mycompany.tokoantik
├── model/
│   ├── Barang.java             (Superclass Entitas Utama)
│   ├── BarangAntik.java        (Subclass Entitas Barang Antik)
│   └── BarangPerhiasan.java    (Subclass Entitas Perhiasan)
├── controller/
│   └── BarangController.java   (Logika Bisnis, CRUD, & Pengelolaan ArrayList)
├── view/
│   └── MainView.java           (Interface CLI dan Alur Interaksi Pengguna)
├── util/
│   └── Validator.java          (Utility Pemrosesan dan Validasi Input)
└── Main.java                   (Kelas Utas Entry Point Utama)
```

#### 6.2 Polymorphism
* **Method Overriding** :
  Metode `getJenisBarang()` dan `toString()` dari kelas `Barang` di-*override* pada kelas `BarangAntik` dan `BarangPerhiasan`. Hal ini memungkinkan pencetakan tabel informasi barang secara dinamis menyesuaikan tipe objek pada run-time.
* **Method Overloading** :
  Kelas `BarangController` mengimplementasikan *method overloading* pada metode penambahan barang:
  - `tambahBarangAntik(String nama, double harga, int stok, String asal, int tahun)`
  - `tambahBarangPerhiasan(String nama, double harga, int stok, String material, double berat)`

---

### 7. Tangkapan Layar Eksekusi Program

#### 7.1 Tampilan Menu Utama dan Read Data Bawaan

<img width="1196" height="437" alt="image" src="https://github.com/user-attachments/assets/578f455d-c68b-4603-b9d3-a3a9fe8352b2" />


#### 7.2 Tambah Data Baru dengan Petunjuk Format Input

<img width="1145" height="476" alt="image" src="https://github.com/user-attachments/assets/fb4ee61e-5c16-4edf-bdea-2679ea940e28" />


#### 7.3 Update Data Barang Spesifik Subclass

<img width="1151" height="672" alt="image" src="https://github.com/user-attachments/assets/6c3b441d-e5f7-4756-8fdd-545e56a0bba7" />


#### 7.4 Hapus Data Barang

<img width="1152" height="570" alt="image" src="https://github.com/user-attachments/assets/2eef8007-3a45-4ee9-a280-3697a28b8ae3" />

