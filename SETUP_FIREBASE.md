# 🔥 Panduan Setup Firebase

Panduan lengkap untuk setup Firebase Backend untuk aplikasi Manajemen Tata Usaha Sekolah.

## 📋 Langkah 1: Buat Project Firebase

1. Buka [Firebase Console](https://console.firebase.google.com/)
2. Klik **Add Project** atau **Create a project**
3. Masukkan nama project: `manajemen-tata-usaha` (atau nama lain)
4. Enable/Disable Google Analytics (optional)
5. Klik **Create Project**

---

## 🔐 Langkah 2: Setup Authentication

1. Di sidebar, klik **Authentication**
2. Klik **Get Started**
3. Pilih **Email/Password** sebagai Sign-in method
4. Enable **Email/Password**
5. Klik **Save**

### Buat User Admin

1. Klik tab **Users**
2. Klik **Add User**
3. Masukkan:
   - Email: `admin@sekolah.com`
   - Password: `admin123` (atau password yang lebih kuat)
4. Klik **Add User**
5. **Copy UID** user yang baru dibuat (akan digunakan di langkah berikutnya)

---

## 💾 Langkah 3: Setup Firestore Database

1. Di sidebar, klik **Firestore Database**
2. Klik **Create Database**
3. Pilih lokasi: **asia-southeast2 (Jakarta)** atau terdekat
4. Pilih mode: **Start in production mode**
5. Klik **Create**

### Setup Security Rules

1. Klik tab **Rules**
2. Paste kode berikut:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow authenticated users to read/write
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
    
    // Optional: More specific rules
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId || 
                     get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    match /surat_masuk/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
    
    match /surat_keluar/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
    
    match /kepegawaian/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
    
    match /keuangan/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
    
    match /siswa/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
    
    match /inventaris/{document} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
  }
}
```

3. Klik **Publish**

### Buat Collection Users

1. Klik tab **Data**
2. Klik **Start Collection**
3. Collection ID: `users`
4. Klik **Next**
5. Document ID: **Paste UID admin yang tadi di-copy**
6. Tambahkan fields:

| Field | Type | Value |
|-------|------|-------|
| displayName | string | Administrator |
| email | string | admin@sekolah.com |
| role | string | admin |
| createdAt | timestamp | (klik Add field timestamp) |

7. Klik **Save**

---

## 📦 Langkah 4: Setup Storage

1. Di sidebar, klik **Storage**
2. Klik **Get Started**
3. Pilih mode: **Start in production mode**
4. Pilih lokasi: **asia-southeast2 (Jakarta)** atau terdekat
5. Klik **Done**

### Setup Storage Rules

1. Klik tab **Rules**
2. Paste kode berikut:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
                     request.resource.size < 10 * 1024 * 1024; // Max 10MB
    }
    
    // Specific folders
    match /surat_masuk/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
                     request.resource.contentType.matches('image/.*|application/pdf|application/msword|application/vnd.openxmlformats-officedocument.wordprocessingml.document');
    }
    
    match /surat_keluar/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && 
                     request.resource.contentType.matches('image/.*|application/pdf|application/msword|application/vnd.openxmlformats-officedocument.wordprocessingml.document');
    }
    
    match /kepegawaian/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    
    match /siswa/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

3. Klik **Publish**

---

## ⚙️ Langkah 5: Dapatkan Konfigurasi Firebase

1. Klik icon **Settings** (⚙️) di sidebar
2. Klik **Project Settings**
3. Scroll ke bawah ke bagian **Your apps**
4. Klik icon **Web** (`</>`)
5. Masukkan nickname: `Manajemen TU Web App`
6. **Jangan** centang Firebase Hosting (kita setup manual)
7. Klik **Register App**
8. **Copy** konfigurasi Firebase yang muncul

Contoh:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

---

## 📝 Langkah 6: Konfigurasi Environment Variables

1. Buka project aplikasi
2. Buat file `.env` di root folder
3. Paste konfigurasi dengan format:

```env
VITE_FIREBASE_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789012
VITE_FIREBASE_APP_ID=1:123456789012:web:abcdef123456

VITE_APP_NAME=Manajemen Tata Usaha Sekolah
VITE_SCHOOL_NAME=SMP Negeri 1 Jakarta
```

4. **Jangan commit file `.env` ke Git!**

---

## ✅ Langkah 7: Test Koneksi

1. Jalankan aplikasi:
```bash
npm install
npm run dev
```

2. Buka `http://localhost:3000`
3. Login dengan:
   - Email: `admin@sekolah.com`
   - Password: `admin123`

4. Jika berhasil login, Firebase sudah terkoneksi! ✅

---

## 🎯 Langkah 8: Buat User Tambahan (Optional)

### Via Firebase Console

1. Authentication → Users → Add User
2. Masukkan email dan password
3. Buat document di collection `users` dengan UID yang sama

### Via Aplikasi (Setelah Login sebagai Admin)

Anda bisa membuat fitur register user di aplikasi nanti.

---

## 📊 Langkah 9: Setup Indexes (Optional)

Jika ada error "requires an index", ikuti link yang muncul di console untuk auto-create index.

Atau manual:
1. Firestore → Indexes
2. Klik **Create Index**
3. Pilih collection dan fields yang perlu di-index

---

## 🔒 Security Best Practices

### 1. Jangan Hardcode API Keys

✅ **Benar**: Gunakan environment variables
```javascript
apiKey: import.meta.env.VITE_FIREBASE_API_KEY
```

❌ **Salah**: Hardcode di code
```javascript
apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
```

### 2. Update Security Rules

Sesuaikan rules dengan kebutuhan role-based access:

```javascript
// Example: Only admin can delete
match /kepegawaian/{document} {
  allow read: if request.auth != null;
  allow create, update: if request.auth != null;
  allow delete: if request.auth != null && 
                  get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
}
```

### 3. Limit File Upload Size

Sudah diatur di Storage Rules (max 10MB).

### 4. Enable App Check (Advanced)

Untuk production, enable App Check untuk mencegah abuse.

---

## 🐛 Troubleshooting

### Error: "Firebase: Error (auth/configuration-not-found)"

**Solusi**: Pastikan Authentication sudah di-enable di Firebase Console.

### Error: "Missing or insufficient permissions"

**Solusi**: 
1. Cek Firestore Rules
2. Pastikan user sudah login
3. Cek console browser untuk detail error

### Error: "Storage object not found"

**Solusi**:
1. Cek Storage Rules
2. Pastikan file path benar
3. Cek apakah file sudah ter-upload

### Data tidak muncul

**Solusi**:
1. Cek koneksi internet
2. Buka Network tab di browser DevTools
3. Cek apakah ada error di Console
4. Pastikan collection name sesuai (`surat_masuk`, `kepegawaian`, dll)

---

## 📈 Monitoring & Analytics

### Enable Analytics (Optional)

1. Project Settings → Integrations
2. Enable Google Analytics
3. Follow setup wizard

### Monitor Usage

1. Firebase Console → Usage and billing
2. Cek quota Firestore, Storage, Authentication

**Free Tier Limits:**
- Firestore: 50K reads, 20K writes, 20K deletes per day
- Storage: 5GB, 1GB download per day
- Authentication: Unlimited

---

## 💰 Upgrade ke Blaze Plan (Optional)

Jika usage melebihi free tier:

1. Project Settings → Usage and billing
2. Klik **Modify Plan**
3. Pilih **Blaze Plan** (pay as you go)
4. Tambahkan billing information

**Estimasi Biaya** (untuk sekolah kecil-menengah):
- Firestore: ~$1-5/bulan
- Storage: ~$1-3/bulan
- **Total**: ~$2-10/bulan

---

## ✅ Checklist Setup Firebase

- ✅ Project Firebase dibuat
- ✅ Authentication enabled (Email/Password)
- ✅ User admin dibuat
- ✅ Firestore Database dibuat
- ✅ Firestore Rules di-setup
- ✅ Collection `users` dibuat dengan data admin
- ✅ Storage enabled
- ✅ Storage Rules di-setup
- ✅ Konfigurasi Firebase di-copy
- ✅ File `.env` dibuat dan dikonfigurasi
- ✅ Test koneksi berhasil

---

## 📞 Bantuan

Jika ada kesulitan:
- Dokumentasi Firebase: https://firebase.google.com/docs
- Stack Overflow: https://stackoverflow.com/questions/tagged/firebase
- Firebase Support: https://firebase.google.com/support

---

**Firebase Setup Selesai! 🎉**

Lanjut ke [README.md](./README.md) untuk menjalankan aplikasi.
