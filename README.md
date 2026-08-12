
<img width="1919" height="1077" alt="Screenshot 2026-08-12 110326" src="https://github.com/user-attachments/assets/fc4086cd-b604-48d1-889e-b02156f77414" />

<img width="1919" height="1079" alt="Screenshot 2026-08-12 110312" src="https://github.com/user-attachments/assets/449d1d4a-989e-4d5e-b2fc-e23665fedbc2" />

<img width="1900" height="1054" alt="Screenshot 2026-08-12 110044" src="https://github.com/user-attachments/assets/5ff98209-c99d-4423-9b2d-4634651faace" />

<img width="1919" height="1079" alt="Screenshot 2026-08-12 105929" src="https://github.com/user-attachments/assets/f0d44a40-cc83-4b71-97d6-12e7c2e49944" />

<img width="1919" height="1079" alt="Screenshot 2026-08-12 105801" src="https://github.com/user-attachments/assets/3cc7a3ad-1fd4-458c-b503-dcdc4f0016f4" />

<img width="1919" height="1079" alt="Screenshot 2026-08-12 105747" src="https://github.com/user-attachments/assets/3260b56f-42e0-4aec-93cd-9f27dc89be6a" />

<img width="1919" height="1079" alt="Screenshot 2026-08-12 105725" src="https://github.com/user-attachments/assets/86e8e214-e1b5-40bf-aa33-5e64e4c57ab2" />





























# Todo API (Express + Sequelize + PostgreSQL)

Struktur MVC simple, auth pake session (bukan JWT), buat login & CRUD todo.

## Struktur folder
```
todo-api/
├── app.js                  # entry point
├── config/
│   └── database.js         # koneksi sequelize
├── models/
│   ├── user.model.js
│   ├── todo.model.js
│   └── index.js
├── controllers/
│   ├── auth.controller.js
│   └── todo.controller.js
├── middlewares/
│   └── auth.middleware.js  # cek session login
├── routes/
│   ├── auth.routes.js
│   └── todo.routes.js
├── seeders/
│   └── seed.js              # data dummy user & todo
└── utils/
    └── response.js         # format response konsisten
```

## Cara install & jalanin

1. Bikin database postgres dulu:
```sql
CREATE DATABASE todo_db;
```

2. Copy `.env.example` jadi `.env`, terus sesuaikan kredensial DB kamu.

3. Install dependency:
```bash
npm install
```

4. Jalankan server:
```bash
npm run dev
```
Tabel `users` dan `todos` bakal otomatis kebuat pas server pertama kali nyala (lewat `sequelize.sync()`).

## Seeder (data dummy)

Buat isi database dengan data awal (2 user + beberapa todo), tinggal jalanin:
```bash
npm run seed
```
Ini bakal bikin (atau skip kalo udah ada, aman dijalanin berkali-kali):
- User `rizki` & `budi`, password sama-sama `password123`
- Beberapa todo dummy punya masing-masing user

Setelah itu langsung bisa login pake salah satu user di atas buat testing endpoint todo.

## Endpoint

### Auth
| Method | Endpoint            | Body                          | Keterangan          |
|--------|----------------------|--------------------------------|----------------------|
| POST   | /api/auth/register   | `{ username, password }`      | Daftar user baru     |
| POST   | /api/auth/login      | `{ username, password }`      | Login, bikin session |
| POST   | /api/auth/logout     | -                              | Logout               |

### Todo (wajib login / punya session valid)
| Method | Endpoint         | Body                          | Keterangan            |
|--------|-------------------|--------------------------------|-------------------------|
| GET    | /api/todos        | -                               | List semua todo user   |
| POST   | /api/todos        | `{ title }`                    | Tambah todo baru       |
| PUT    | /api/todos/:id    | `{ title?, is_done? }`         | Update todo            |
| DELETE | /api/todos/:id    | -                               | Hapus todo             |

## Format response
Semua response pake format seragam:
```json
{
  "code": 200,
  "success": true,
  "message": "...",
  "data": { }
}
```

## Catatan
- Auth pake `express-session`, session id disimpan di cookie `connect.sid`. Kalo test pake Postman/Insomnia, pastiin cookie di-enable biar session kebawa antar-request.
- Kalo mau deploy production, ganti session store default (in-memory) ke store yang persist, misal `connect-pg-simple` biar session gak ilang tiap restart server.
