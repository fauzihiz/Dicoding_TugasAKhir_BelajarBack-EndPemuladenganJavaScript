# Bookshelf API

API sederhana untuk mengelola koleksi buku dengan fitur CRUD (Create, Read, Update, Delete).

## Kriteria Proyek

### 1. Konfigurasi Port
- Aplikasi harus berjalan pada **port 9000**
- Jika port 9000 tidak tersedia saat development, gunakan port lain dan ubah ke 9000 saat submission

### 2. Script Runner
Aplikasi harus dapat dijalankan dengan perintah:
```bash
npm run start
```

Konfigurasi `package.json`:
```json
{
  "name": "bookshelf-api",
  "scripts": {
    "start": "node src/server.js",
    "start-dev": "nodemon src/server.js"
  }
}
```

### 3. Menyimpan Buku

**Endpoint:**
```
POST /books
```

**Request Body:**
```json
{
  "name": "string",
  "year": "number",
  "author": "string",
  "summary": "string",
  "publisher": "string",
  "pageCount": "number",
  "readPage": "number",
  "reading": "boolean"
}
```

**Struktur Buku di Server:**
```json
{
  "id": "Qbax5Oy7L8WKf74l",
  "name": "Buku A",
  "year": 2010,
  "author": "John Doe",
  "summary": "Lorem ipsum dolor sit amet",
  "publisher": "Dicoding Indonesia",
  "pageCount": 100,
  "readPage": 25,
  "finished": false,
  "reading": false,
  "insertedAt": "2021-03-04T09:11:44.598Z",
  "updatedAt": "2021-03-04T09:11:44.598Z"
}
```

**Properti yang Digenerate Server:**
- `id`: Unique identifier (gunakan nanoid@3)
- `finished`: Boolean, `true` jika `pageCount === readPage`
- `insertedAt`: Timestamp saat buku ditambahkan
- `updatedAt`: Timestamp saat buku diperbarui

**Response Sukses (201):**
```json
{
  "status": "success",
  "message": "Buku berhasil ditambahkan",
  "data": {
    "bookId": "1L7ZtDUFeGs7VlEt"
  }
}
```

**Response Error:**

*Nama buku kosong (400):*
```json
{
  "status": "fail",
  "message": "Gagal menambahkan buku. Mohon isi nama buku"
}
```

*readPage > pageCount (400):*
```json
{
  "status": "fail",
  "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
}
```

### 4. Menampilkan Semua Buku

**Endpoint:**
```
GET /books
```

**Response Sukses (200):**
```json
{
  "status": "success",
  "data": {
    "books": [
      {
        "id": "Qbax5Oy7L8WKf74l",
        "name": "Buku A",
        "publisher": "Dicoding Indonesia"
      },
      {
        "id": "1L7ZtDUFeGs7VlEt",
        "name": "Buku B",
        "publisher": "Dicoding Indonesia"
      }
    ]
  }
}
```

**Response Jika Kosong:**
```json
{
  "status": "success",
  "data": {
    "books": []
  }
}
```

### 5. Menampilkan Detail Buku

**Endpoint:**
```
GET /books/{bookId}
```

**Response Sukses (200):**
```json
{
  "status": "success",
  "data": {
    "book": {
      "id": "aWZBUW3JN_VBE-9I",
      "name": "Buku A Revisi",
      "year": 2011,
      "author": "Jane Doe",
      "summary": "Lorem Dolor sit Amet",
      "publisher": "Dicoding",
      "pageCount": 200,
      "readPage": 26,
      "finished": false,
      "reading": false,
      "insertedAt": "2021-03-05T06:14:28.930Z",
      "updatedAt": "2021-03-05T06:14:30.718Z"
    }
  }
}
```

**Response Buku Tidak Ditemukan (404):**
```json
{
  "status": "fail",
  "message": "Buku tidak ditemukan"
}
```

### 6. Mengubah Data Buku

**Endpoint:**
```
PUT /books/{bookId}
```

**Request Body:**
```json
{
  "name": "string",
  "year": "number",
  "author": "string",
  "summary": "string",
  "publisher": "string",
  "pageCount": "number",
  "readPage": "number",
  "reading": "boolean"
}
```

**Response Sukses (200):**
```json
{
  "status": "success",
  "message": "Buku berhasil diperbarui"
}
```

**Response Error:**

*Nama buku kosong (400):*
```json
{
  "status": "fail",
  "message": "Gagal memperbarui buku. Mohon isi nama buku"
}
```

*readPage > pageCount (400):*
```json
{
  "status": "fail",
  "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
}
```

*ID tidak ditemukan (404):*
```json
{
  "status": "fail",
  "message": "Gagal memperbarui buku. Id tidak ditemukan"
}
```

### 7. Menghapus Buku

**Endpoint:**
```
DELETE /books/{bookId}
```

**Response Sukses (200):**
```json
{
  "status": "success",
  "message": "Buku berhasil dihapus"
}
```

**Response ID Tidak Ditemukan (404):**
```json
{
  "status": "fail",
  "message": "Buku gagal dihapus. Id tidak ditemukan"
}
```

## Instalasi dan Menjalankan

1. Install dependencies:
```bash
npm install
```

2. Install nanoid untuk generate ID:
```bash
npm install nanoid@3
```

3. Jalankan aplikasi:
```bash
npm run start
```

4. Aplikasi akan berjalan di `http://localhost:9000`

## Teknologi yang Digunakan

- Node.js
- Hapi.js (framework web)
- nanoid (untuk generate unique ID)

## Struktur Proyek

```
src/
├── server.js          # Entry point aplikasi
├── handler.js         # Handler untuk setiap endpoint
├── routes.js          # Definisi routes
└── books.js           # Storage untuk data buku
```
