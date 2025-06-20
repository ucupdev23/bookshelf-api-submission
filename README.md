# Bookshelf API Submission - Dicoding Backend Developer

[![Node.js](https://img.shields.io/badge/Node.js-18.x-green?logo=node.js)](https://nodejs.org/)
[![HapiJS](https://img.shields.io/badge/HapiJS-Framework-blue?logo=hapijs)](https://hapi.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Proyek ini adalah submission akhir untuk kelas "Belajar Membuat Aplikasi Back-End untuk Pemula" dari Dicoding Academy. Mengimplementasikan RESTful API untuk mengelola koleksi buku digital, sesuai dengan kriteria yang telah ditentukan.

## Daftar Isi

- [Deskripsi Proyek](#deskripsi-proyek)
- [Teknologi yang Digunakan](#teknologi-yang-digunakan)
- [Fitur API](#fitur-api)
  - [1. Menambahkan Buku](#1-menambahkan-buku)
  - [2. Mendapatkan Seluruh Buku](#2-mendapatkan-seluruh-buku)
  - [3. Mendapatkan Detail Buku](#3-mendapatkan-detail-buku)
  - [4. Mengubah Data Buku](#4-mengubah-data-buku)
  - [5. Menghapus Buku](#5-menghapus-buku)
- [Cara Menjalankan Proyek (untuk Reviewer)](#cara-menjalankan-proyek-untuk-reviewer)
- [Struktur Proyek](#struktur-proyek)
- [Kontribusi](#kontribusi)
- [Kontak](#kontak)

## Deskripsi Proyek

Bookshelf API ini menyediakan fungsionalitas CRUD (Create, Read, Update, Delete) untuk data buku. Data buku disimpan secara in-memory (dalam array JavaScript) pada server, memenuhi persyaratan bahwa tidak menggunakan database eksternal seperti SQL atau NoSQL.

Aplikasi ini berjalan pada **Port 9000** dan dapat diuji menggunakan Postman Collection yang disediakan oleh Dicoding.

## Teknologi yang Digunakan

-   **Node.js**: Runtime JavaScript (Direkomendasikan Node.js LTS 18.13.0)
-   **Hapi Framework**: Framework web modular untuk membangun aplikasi dan layanan
-   **nanoid**: Library kecil, aman, dan *URL-friendly* untuk menghasilkan ID unik

## Fitur API

Berikut adalah daftar endpoint API yang diimplementasikan:

### 1. Menambahkan Buku

-   **Method**: `POST`
-   **URL**: `/books`
-   **Request Body**:
    ```json
    {
        "name": "string",        // Judul buku (Wajib)
        "year": 2023,            // Tahun terbit
        "author": "string",      // Nama penulis
        "summary": "string",     // Ringkasan buku
        "publisher": "string",   // Penerbit
        "pageCount": 100,        // Jumlah halaman
        "readPage": 25,          // Halaman yang sudah dibaca
        "reading": false         // Status sedang dibaca
    }
    ```
-   **Respons Sukses (201 Created)**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil ditambahkan",
        "data": {
            "bookId": "string" // ID unik buku yang baru ditambahkan
        }
    }
    ```
-   **Respons Gagal**:
    -   `name` tidak dilampirkan: `400 Bad Request` - `{"status": "fail", "message": "Gagal menambahkan buku. Mohon isi nama buku"}`
    -   `readPage` > `pageCount`: `400 Bad Request` - `{"status": "fail", "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"}`

### 2. Mendapatkan Seluruh Buku

-   **Method**: `GET`
-   **URL**: `/books`
-   **Query Parameters (Opsional, untuk filtering)**:
    -   `?name=<keyword>`: Menampilkan buku yang namanya mengandung `<keyword>` (non-case sensitive).
    -   `?reading=0` atau `?reading=1`: Menampilkan buku yang sedang tidak dibaca (`0`) atau sedang dibaca (`1`).
    -   `?finished=0` atau `?finished=1`: Menampilkan buku yang belum selesai dibaca (`0`) atau sudah selesai dibaca (`1`).
-   **Respons Sukses (200 OK)**:
    ```json
    {
        "status": "success",
        "data": {
            "books": [
                {
                    "id": "string",
                    "name": "string",
                    "publisher": "string"
                }
                // ... daftar buku lainnya
            ]
        }
    }
    ```
    (Jika tidak ada buku, `books` akan berupa array kosong `[]`)

### 3. Mendapatkan Detail Buku

-   **Method**: `GET`
-   **URL**: `/books/{bookId}`
-   **Respons Sukses (200 OK)**:
    ```json
    {
        "status": "success",
        "data": {
            "book": {
                "id": "string",
                "name": "string",
                "year": 2011,
                "author": "string",
                "summary": "string",
                "publisher": "string",
                "pageCount": 200,
                "readPage": 26,
                "finished": false,
                "reading": false,
                "insertedAt": "string (ISO Date)",
                "updatedAt": "string (ISO Date)"
            }
        }
    }
    ```
-   **Respons Gagal**:
    -   Buku tidak ditemukan: `404 Not Found` - `{"status": "fail", "message": "Buku tidak ditemukan"}`

### 4. Mengubah Data Buku

-   **Method**: `PUT`
-   **URL**: `/books/{bookId}`
-   **Request Body**: Sama dengan saat menambahkan buku.
-   **Respons Sukses (200 OK)**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil diperbarui"
    }
    ```
-   **Respons Gagal**:
    -   `name` tidak dilampirkan: `400 Bad Request` - `{"status": "fail", "message": "Gagal memperbarui buku. Mohon isi nama buku"}`
    -   `readPage` > `pageCount`: `400 Bad Request` - `{"status": "fail", "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"}`
    -   ID buku tidak ditemukan: `404 Not Found` - `{"status": "fail", "message": "Gagal memperbarui buku. Id tidak ditemukan"}`

### 5. Menghapus Buku

-   **Method**: `DELETE`
-   **URL**: `/books/{bookId}`
-   **Respons Sukses (200 OK)**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil dihapus"
    }
    ```
-   **Respons Gagal**:
    -   ID buku tidak ditemukan: `404 Not Found` - `{"status": "fail", "message": "Buku gagal dihapus. Id tidak ditemukan"}`

## Cara Menjalankan Proyek (untuk Reviewer)

Ikuti langkah-langkah berikut untuk menjalankan dan menguji Bookshelf API ini:

1.  **Clone repositori ini:**
    ```bash
    git clone https://github.com/ucupdev23/bookshelf-api-submission.git
    cd bookshelf-api-submission
    ```

2.  **Instal semua dependensi:**
    Pastikan Node.js (direkomendasikan LTS 18.13.0) sudah terinstal di sistem Anda.
    ```bash
    npm install
    ```

3.  **Jalankan aplikasi:**
    ```bash
    npm run start
    ```
    Server akan berjalan di `http://localhost:9000`. Anda akan melihat pesan di konsol seperti: `Server berjalan pada http://0.0.0.0:9000`

4.  **Uji API dengan Postman:**
    Anda dapat menguji semua endpoint API menggunakan Postman Collection dan Environment yang disediakan oleh Dicoding. Pastikan untuk mengimpor keduanya ke Postman Anda.

## Struktur Direktor

Berikut adalah struktur direktori dari proyek ini:

-   `bookshelf-api-submission`
    -   `src/` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
        -   `books.js` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Array in-memory untuk menyimpan data buku
        -   `handler.js` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Berisi logika (handler) untuk setiap endpoint API
        -   `routes.js` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Mendefinisikan semua rute (endpoints) API
        -   `server.js` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Konfigurasi dan inisialisasi server Hapi
    -   `gitignore` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Mengabaikan file/folder yang tidak perlu diunggah ke Git
    -   `package.json` &nbsp; &nbsp; &nbsp; &nbsp; # Daftar dependensi dan script proyek
    -   `package-lock.json` &nbsp; &nbsp; &nbsp; &nbsp; # Mengunci versi dependensi
    -   `README.md` &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; # Dokumentasi proyek ini

---

## Kontribusi

Proyek ini adalah submission akhir untuk kelas "Belajar Membuat Aplikasi Back-End untuk Pemula" dari Dicoding. Jika Anda memiliki saran atau menemukan *bug*, silakan buka *issue* atau *pull request*.

---

## Kontak

Jika Anda memiliki pertanyaan lebih lanjut, Anda bisa menghubungi saya:

* **Nama:** Yusuf Abdilhaq
* **Email:** ucup.dev23@gmail.com
* **LinkedIn:** https://www.linkedin.com/in/yusuf-abdilhaq/

---

**Terima kasih telah mengunjungi repositori ini!**
