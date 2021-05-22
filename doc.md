# Notes REST API

## Kriteria Proyek

1. Web Server dapat menyimpan catatan
    
    Kriteria pertama adalah web server dapat menyimpan catatan yang ditambahkan melalui aplikasi web.

    Struktur catatan yang akan disimpan oleh server:
    ```json
    {
        "id": string,
        "title": string,
        "createdAt": string,
        "updatedAt": string,
        "tags": array of string,
        "body": string,
    }
    ```

    Contoh data:
    ```json
    {
        "id": "notes-V1StGXR8_Z5jdHi6B-myT",
        "title": "Sejarah JavaScript",
        "createdAt": "2020-12-23T23:00:09.686Z",
        "updatedAt": "2020-12-23T23:00:09.686Z",
        "tags": ["NodeJS", "JavaScript"],
        "body": "JavaScript pertama kali dikembangkan oleh Brendan Eich dari Netscape di bawah nama Mocha, yang nantinya namanya diganti menjadi LiveScript, dan akhirnya menjadi JavaScript. Navigator sebelumnya telah mendukung Java untuk lebih bisa dimanfaatkan para pemrogram yang non-Java.",
    }
    ```
    Jika permintaan client berhasil dilakukan, respons dari server harus memiliki status code 201 (created).
    ```json
    {
        "status": "success",
        "message": "Catatan berhasil ditambahkan",
        "data": {
            "noteId": "V09YExygSUYogwWJ"
        }
    }
    ```
    Bila permintaan gagal dilakukan, berikan status code 500 dan kembalikan dengan data JSON dengan format berikut:
    ```json
    {
        "status": "error",
        "message": "Catatan gagal untuk ditambahkan"
    }
    ```
1. Web Server dapat menampilkan catatan

    Ketika client melakukan permintaan pada path `‘/notes’` dengan method `‘GET’`, maka server harus mengembalikan status code 200 (ok).
    ```json
    {
        "status": "success",
        "data": {
            "notes": [
                {
                    "id":"notes-V1StGXR8_Z5jdHi6B-myT",
                    "title":"Catatan 1",
                    "createdAt":"2020-12-23T23:00:09.686Z",
                    "updatedAt":"2020-12-23T23:00:09.686Z",
                    "tags":[
                        "Tag 1",
                        "Tag 2"
                    ],
                    "body":"Isi dari catatan 1"
                },
                {
                    "id":"notes-V1StGXR8_98apmLk3mm1",
                    "title":"Catatan 2",
                    "createdAt":"2020-12-23T23:00:09.686Z",
                    "updatedAt":"2020-12-23T23:00:09.686Z",
                    "tags":[
                        "Tag 1",
                        "Tag 2"
                    ],
                    "body":"Isi dari catatan 2"
                }
            ]
        }
    }
    ```
    Client dapat menanggapi permintaan menampilkan catatan secara spesifik menggunakan id melalui path `‘/notes/{id}’` dengan method `‘GET’`.
    ```json
    {
        "status": "success",
        "data": {
            "note": {
                "id":"notes-V1StGXR8_Z5jdHi6B-myT",
                "title":"Catatan 1",
                "createdAt":"2020-12-23T23:00:09.686Z",
                "updatedAt":"2020-12-23T23:00:09.686Z",
                "tags":[
                    "Tag 1",
                    "Tag 2"
                ],
                "body":"Isi dari catatan 1"
            }
        }
    }
    ```
    Bila client melampirkan id catatan yang tidak ditemukan, server harus merespons dengan status code 404.
    ```json
    {
        "status": "fail",
        "message": "Catatan tidak ditemukan"
    }
    ```
1. Web Server dapat mengubah catatan

    Kriteria ketiga adalah web server harus dapat mengubah catatan. Perubahan yang dimaksud bisa berupa judul, isi, ataupun tag catatan. Ketika client meminta perubahan catatan, ia akan membuat permintaan ke path `'/notes/{id}'`, menggunakan method `'PUT'`.
    ```json
    {
        "title":"Judul Catatan Revisi",
        "tags":[
            "Tag 1",
            "Tag 2"
        ],
        "body":"Konten catatan"
    }
    ```
    Jika perubahan data berhasil dilakukan, server harus menanggapi dengan status code 200 (ok).
    ```json
    {
        "status": "success",
        "message": "Catatan berhasil diperbaharui"
    }
    ```
    Jika gagal akan menanggapi response:
    ```json
    {
        "status": "fail",
        "message": "Gagal memperbarui catatan. Id catatan tidak ditemukan"
    }
    ```
1. Web Server dapat menghapus catatan

    Kriteria terakhir adalah web server harus bisa menghapus catatan. Untuk menghapus catatan, client akan membuat permintaan pada path `‘/notes/{id}’` dengan method `‘DELETE’`. Ketika permintaan tersebut berhasil, maka server harus mengembalikan status code 200 (ok) serta data JSON berikut:
    ```json
    {
        "status": "success",
        "message": "Catatan berhasil dihapus"
    }
    ```
    Jika gagal akan mengembalikan response:
    ```json
    {
        "status": "fail",
        "message": "Catatan gagal dihapus. Id catatan tidak ditemukan"
    }
    ```

## Struktur Proyek

Struktur proyek keseluruhan akan tampak seperti ini:
```
notes-app-back-end
├── node_modules
├── src
│ ├── handler.js
│ ├── notes.js
│ ├── routes.js
│ └── server.js
├── .eslintrc.json
├── package-lock.json
└── package.json
```