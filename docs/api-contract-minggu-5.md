# API Contract — GameHunter CRUD

Resource: Game. Semua endpoint memakai JSON dan No Auth pada server lokal.

| Method | Endpoint | Success | Error |
| --- | --- | --- | --- |
| GET | /api/games | 200, array data | 422, query tidak valid |
| POST | /api/games | 201, object data | 422, validasi gagal |
| GET | /api/games/{id} | 200, object data | 404 |
| PUT/PATCH | /api/games/{id} | 200, object data | 422 atau 404 |
| DELETE | /api/games/{id} | 204, tanpa body | 404 |

Header: `Accept: application/json` dan `Content-Type: application/json`.

## Input

| Field | Create | Update |
| --- | --- | --- |
| title | Wajib, string 3–200 karakter | Opsional; jika dikirim, aturan sama |
| platform | Wajib, pc atau browser | Opsional; jika dikirim, aturan sama |
| is_free | Opsional, boolean; default false | Opsional, boolean; tidak boleh null |

PUT/PATCH hanya mengubah field yang dikirim. `id`, `source`, dan timestamps ditentukan server. Game baru memakai `source: manual`. Judul tidak diwajibkan unik.

## Success response

Field response tetap seperti Minggu 4: `id` integer, `title`, `platform`, dan `source` string, `is_free` boolean, serta `updated_at` ISO 8601.

Contoh POST:

```json
{"title":"Game Praktikum CRUD","platform":"pc","is_free":true}
```

Response 201:

```json
{
  "data": {
    "id": 3,
    "title": "Game Praktikum CRUD",
    "platform": "pc",
    "is_free": true,
    "source": "manual",
    "updated_at": "2026-10-12T09:53:18.000000Z"
  }
}
```

ID dan waktu dapat berbeda. DELETE 204 tidak memakai wrapper atau body.

## Error response

404:

```json
{"message":"Resource not found","errors":null}
```

422 untuk title kosong dan platform console:

```json
{
  "message": "The title field is required. (and 1 more error)",
  "errors": {
    "title": ["The title field is required."],
    "platform": ["The selected platform is invalid."]
  }
}
```

Filter daftar tetap `?platform=pc` atau `?platform=browser`. Query lain menghasilkan 422. Daftar kosong tetap 200 dengan `data: []`.
