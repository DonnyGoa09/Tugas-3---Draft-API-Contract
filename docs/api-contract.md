# API Contract - Task Management API

## User Stories

- Sebagai pengguna, saya ingin melihat daftar tugas agar pekerjaan mudah dipantau.
- Sebagai pengguna, saya ingin menambah tugas agar pekerjaan baru dapat dicatat.

## Resource Dictionary - Task

| Field | Type | Required saat create | Akses | Aturan | Contoh |
| --- | --- | --- | --- | --- | --- |
| `id` | integer | Tidak | Read-only | Dibuat server | `15` |
| `title` | string | Ya | Read/write | 3-150 karakter | `Membuat laporan` |
| `description` | string | Tidak | Read/write | Maksimal 500 karakter | `Laporan praktikum minggu 3` |
| `priority` | string | Ya | Read/write | `low`, `medium`, atau `high` | `high` |
| `category_id` | integer | Ya | Read/write | Category harus tersedia | `3` |
| `due_date` | string | Tidak | Read/write | Format `YYYY-MM-DD` | `2026-10-05` |
| `completed` | boolean | Tidak | Read/write | Default `false` | `false` |
| `created_at` | string | Tidak | Read-only | ISO 8601 datetime | `2026-09-28T09:30:00Z` |

## Endpoint Matrix

| Kebutuhan | Method | Endpoint | Success | Error |
| --- | --- | --- | --- | --- |
| Melihat daftar tugas | `GET` | `/api/tasks` | `200` | - |
| Membuat tugas | `POST` | `/api/tasks` | `201` | `404`, `422` |
| Melihat satu tugas | `GET` | `/api/tasks/{task}` | `200` | `404` |
| Mengubah tugas | `PATCH` | `/api/tasks/{task}` | `200` | `404`, `422` |
| Menghapus tugas | `DELETE` | `/api/tasks/{task}` | `204` | `404` |

## Create Request

```http
POST /api/tasks
Content-Type: application/json
Accept: application/json
```

```json
{
  "title": "Membuat laporan",
  "description": "Laporan praktikum minggu 3",
  "priority": "high",
  "category_id": 3,
  "due_date": "2026-10-05"
}
```

## Response Examples

### 200 OK

```json
{
  "data": [
    {
      "id": 15,
      "title": "Membuat laporan",
      "description": "Laporan praktikum minggu 3",
      "priority": "high",
      "category_id": 3,
      "due_date": "2026-10-05",
      "completed": false,
      "created_at": "2026-09-28T09:30:00Z"
    }
  ]
}
```

### 201 Created

```json
{
  "data": {
    "id": 15,
    "title": "Membuat laporan",
    "description": "Laporan praktikum minggu 3",
    "priority": "high",
    "category_id": 3,
    "due_date": "2026-10-05",
    "completed": false,
    "created_at": "2026-09-28T09:30:00Z"
  }
}
```

### 404 Not Found

```json
{
  "message": "Category not found",
  "errors": null
}
```

### 422 Unprocessable Content

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "title": [
      "The title field is required."
    ],
    "priority": [
      "The selected priority is invalid."
    ]
  }
}
```

## Design Decisions

- Memakai `PATCH` karena perubahan tugas biasanya hanya pada beberapa field.
- `id` dan `created_at` dibuat server sehingga tidak dikirim saat create.
- `category_id` harus menunjuk category yang tersedia.

## Consistency Review

- Path memakai plural noun `/tasks`.
- Type field sama pada semua contoh.
- Field read-only tidak ada pada create request.
- Response error memakai pola `message` dan `errors`.
- Tidak ada password, token, API key, atau data pribadi.

## Refleksi

1. Field dan bentuk error paling memengaruhi client karena dipakai langsung pada tampilan dan validasi.
2. Jika type berubah, parsing client dapat gagal dan data bisa tampil salah.
3. Endpoint menjadi route, aturan field menjadi validation, dan response menjadi API Resource di Laravel.

## Referensi

- Materi 3 - API Contract dan Resource Modelling.
- Praktikum 3 - Merancang API Contract.
- MDN - HTTP response status codes.
- Postman - Create examples of request responses.

