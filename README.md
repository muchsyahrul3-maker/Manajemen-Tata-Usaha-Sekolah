# 🎓 Sistem Manajemen Tata Usaha Sekolah

Aplikasi web modern untuk mengelola administrasi sekolah secara digital, lengkap dengan fitur surat-menyurat, kepegawaian, keuangan, data siswa, inventaris, dan laporan.

## ✨ Fitur Utama

### 📊 Dashboard
- Ringkasan statistik real-time (siswa, guru, surat, keuangan)
- Grafik visualisasi data
- Notifikasi aktivitas terbaru
- Tampilan responsif dan interaktif

### 📧 Manajemen Surat-Menyurat
- **Surat Masuk**: Input, arsip, dan tracking surat masuk
- **Surat Keluar**: Kelola surat keluar dengan nomor otomatis
- Upload file dokumen (PDF, DOCX, JPG)
- Pencarian cepat berdasarkan nomor/pengirim/perihal
- Status tracking (Baru, Diproses, Selesai)

### 👥 Data Kepegawaian
- Database guru dan staf lengkap
- Upload dokumen (SK, ijazah, sertifikat)
- Filter berdasarkan jabatan dan status
- Manajemen data kontak dan riwayat

### 💰 Manajemen Keuangan
- Pencatatan pemasukan dan pengeluaran
- Laporan keuangan otomatis
- Grafik arus kas
- Kategori transaksi yang fleksibel

### 🎓 Data Siswa
- Database siswa lengkap dengan biodata
- Upload dokumen (akte, raport, foto)
- Filter per kelas
- Data wali siswa

### 📦 Inventaris Barang
- Pencatatan aset dan barang sekolah
- Tracking kondisi barang
- Lokasi penyimpanan
- Kategori inventaris

### 📄 Laporan & Export
- Generate laporan otomatis
- Export ke PDF dan Excel
- Laporan per periode
- Template profesional

### 🔐 Manajemen Akun
- Multi-role (Admin TU, Guru, Kepala Sekolah)
- Autentikasi aman dengan Firebase
- Dark/Light mode
- Pengaturan profil

## 🛠️ Teknologi

- **Frontend**: React 18 + Vite
- **Styling**: TailwindCSS
- **Backend**: Firebase (Firestore, Auth, Storage)
- **State Management**: Zustand
- **Charts**: Recharts
- **Icons**: Lucide React
- **PDF Export**: jsPDF
- **Excel Export**: XLSX
- **Notifications**: React Hot Toast

## 📋 Prasyarat

- Node.js 16+ dan npm/yarn
- Akun Firebase (gratis)
- Browser modern (Chrome, Firefox, Edge, Safari)

## 🚀 Instalasi

### 1. Clone atau Download Project

```bash
cd "Manajemen Tata Usaha"
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Konfigurasi Firebase

1. Buat project baru di [Firebase Console](https://console.firebase.google.com/)
2. Aktifkan **Authentication** (Email/Password)
3. Buat **Firestore Database** (mode production)
4. Aktifkan **Storage**
5. Copy konfigurasi Firebase Anda

### 4. Setup Environment Variables

Buat file `.env` di root folder:

```env
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id

VITE_APP_NAME=Manajemen Tata Usaha Sekolah
VITE_SCHOOL_NAME=Nama Sekolah Anda
```

### 5. Setup Firestore Rules

Di Firebase Console > Firestore Database > Rules, paste:

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

### 6. Setup Storage Rules

Di Firebase Console > Storage > Rules:

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

### 7. Buat User Admin

Di Firebase Console > Authentication > Users, tambah user:
- Email: `admin@sekolah.com`
- Password: `admin123`

Kemudian di Firestore, buat collection `users` dengan document ID = UID user tersebut:

```json
{
  "displayName": "Administrator",
  "email": "admin@sekolah.com",
  "role": "admin",
  "createdAt": [timestamp]
}
```

## 🎯 Menjalankan Aplikasi

### Development Mode

```bash
npm run dev
```

Aplikasi akan berjalan di `http://localhost:3000`

### Build untuk Production

```bash
npm run build
```

File production akan ada di folder `dist/`

## 🌐 Deploy ke Hosting

### Deploy ke Firebase Hosting

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login ke Firebase:
```bash
firebase login
```

3. Inisialisasi Firebase:
```bash
firebase init hosting
```

Pilih:
- Use existing project
- Public directory: `dist`
- Single-page app: `Yes`
- Automatic builds: `No`

4. Build dan Deploy:
```bash
npm run build
firebase deploy
```

### Deploy ke Vercel

1. Install Vercel CLI:
```bash
npm install -g vercel
```

2. Deploy:
```bash
npm run build
vercel --prod
```

Atau gunakan [Vercel Dashboard](https://vercel.com):
- Import repository
- Framework Preset: Vite
- Build Command: `npm run build`
- Output Directory: `dist`
- Tambahkan Environment Variables

### Deploy ke Netlify

1. Install Netlify CLI:
```bash
npm install -g netlify-cli
```

2. Deploy:
```bash
npm run build
netlify deploy --prod
```

Atau drag & drop folder `dist` ke [Netlify Drop](https://app.netlify.com/drop)

## 👤 Login Credentials

**Admin/Tata Usaha:**
- Email: `admin@sekolah.com`
- Password: `admin123`

**Guru:**
- Email: `guru@sekolah.com`
- Password: `guru123`

## 📱 Fitur Responsif

Aplikasi fully responsive dan dapat diakses dari:
- 💻 Desktop/Laptop
- 📱 Tablet
- 📱 Smartphone

## 🎨 Tema

- ☀️ Light Mode (default)
- 🌙 Dark Mode (toggle di header)

## 📊 Struktur Database (Firestore Collections)

```
📁 users
  - uid: { displayName, email, role, ... }

📁 surat_masuk
  - { nomorSurat, tanggalTerima, pengirim, perihal, status, fileURL, ... }

📁 surat_keluar
  - { nomorSurat, tanggalSurat, tujuan, perihal, fileURL, ... }

📁 kepegawaian
  - { nama, nip, jabatan, statusKepegawaian, email, telepon, ... }

📁 keuangan
  - { tanggal, jenis, kategori, jumlah, keterangan, ... }

📁 siswa
  - { nama, nis, nisn, kelas, jenisKelamin, namaWali, ... }

📁 inventaris
  - { namaBarang, kodeBarang, kategori, jumlah, kondisi, lokasi, ... }
```

## 🔒 Keamanan

- ✅ Firebase Authentication
- ✅ Firestore Security Rules
- ✅ Storage Security Rules
- ✅ Environment Variables untuk API Keys
- ✅ Role-based Access Control

## 🐛 Troubleshooting

### Error: Firebase not initialized
- Pastikan file `.env` sudah dibuat dan berisi konfigurasi yang benar
- Restart development server setelah membuat `.env`

### Error: Permission denied
- Cek Firestore Rules dan Storage Rules
- Pastikan user sudah login

### Data tidak muncul
- Cek koneksi internet
- Cek Firebase Console apakah data sudah ada
- Buka Browser Console untuk melihat error

## 📞 Support

Untuk pertanyaan dan dukungan:
- 📧 Email: support@sekolah.com
- 📱 WhatsApp: 08xx-xxxx-xxxx

## 📝 License

© 2024 Manajemen Tata Usaha Sekolah. All rights reserved.

## 🙏 Credits

Dibuat dengan ❤️ menggunakan:
- React
- TailwindCSS
- Firebase
- Recharts
- Lucide Icons

---

**Selamat menggunakan Sistem Manajemen Tata Usaha Sekolah!** 🎉
