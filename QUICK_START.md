# 🚀 Quick Start Guide

Panduan cepat untuk menjalankan aplikasi Manajemen Tata Usaha Sekolah dalam 5 menit!

## ⚡ Langkah Cepat

### 1️⃣ Install Dependencies (1 menit)

```bash
cd "Manajemen Tata Usaha"
npm install
```

### 2️⃣ Setup Firebase (2 menit)

1. Buka [Firebase Console](https://console.firebase.google.com/)
2. Buat project baru
3. Enable **Authentication** (Email/Password)
4. Buat **Firestore Database**
5. Enable **Storage**
6. Copy konfigurasi Firebase

### 3️⃣ Konfigurasi Environment (30 detik)

Buat file `.env`:

```env
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id

VITE_APP_NAME=Manajemen Tata Usaha Sekolah
VITE_SCHOOL_NAME=Nama Sekolah Anda
```

### 4️⃣ Setup Firestore Rules (30 detik)

Di Firebase Console → Firestore → Rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 5️⃣ Setup Storage Rules (30 detik)

Di Firebase Console → Storage → Rules:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 6️⃣ Buat User Admin (30 detik)

1. Firebase Console → Authentication → Add User
   - Email: `admin@sekolah.com`
   - Password: `admin123`

2. Copy UID user tersebut

3. Firestore → Create Collection `users`
   - Document ID: [UID yang di-copy]
   - Fields:
     ```
     displayName: "Administrator"
     email: "admin@sekolah.com"
     role: "admin"
     ```

### 7️⃣ Jalankan Aplikasi! (10 detik)

```bash
npm run dev
```

Buka browser: `http://localhost:3000`

Login dengan:
- Email: `admin@sekolah.com`
- Password: `admin123`

---

## ✅ Selesai!

Aplikasi sudah berjalan! 🎉

## 📚 Dokumentasi Lengkap

- [README.md](./README.md) - Dokumentasi lengkap
- [SETUP_FIREBASE.md](./SETUP_FIREBASE.md) - Setup Firebase detail
- [DEPLOYMENT.md](./DEPLOYMENT.md) - Panduan deploy

## 🆘 Troubleshooting

### Error: "Firebase not initialized"
→ Cek file `.env` sudah dibuat dan berisi konfigurasi yang benar

### Error: "Permission denied"
→ Cek Firestore Rules dan Storage Rules sudah di-setup

### Login gagal
→ Pastikan user sudah dibuat di Firebase Authentication

---

**Selamat menggunakan! 🎓**
