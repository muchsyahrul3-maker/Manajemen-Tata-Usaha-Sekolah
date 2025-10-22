# 🚀 Panduan Deployment

Panduan lengkap untuk deploy aplikasi Manajemen Tata Usaha Sekolah ke berbagai platform hosting.

## 📋 Persiapan Sebelum Deploy

### 1. Build Production

```bash
npm run build
```

Pastikan build berhasil tanpa error. File hasil build ada di folder `dist/`.

### 2. Test Build Lokal

```bash
npm run preview
```

Buka `http://localhost:4173` untuk test build production di lokal.

### 3. Checklist Pre-Deployment

- ✅ File `.env` sudah dikonfigurasi dengan benar
- ✅ Firebase project sudah setup (Auth, Firestore, Storage)
- ✅ Firestore Rules dan Storage Rules sudah diatur
- ✅ User admin sudah dibuat di Firebase Authentication
- ✅ Build production berhasil tanpa error
- ✅ Test lokal berjalan dengan baik

---

## 🔥 Deploy ke Firebase Hosting

Firebase Hosting adalah pilihan terbaik karena terintegrasi langsung dengan Firebase Backend.

### Langkah-langkah:

#### 1. Install Firebase CLI

```bash
npm install -g firebase-tools
```

#### 2. Login ke Firebase

```bash
firebase login
```

Browser akan terbuka untuk login dengan akun Google Anda.

#### 3. Inisialisasi Firebase di Project

```bash
firebase init
```

Pilih:
- **Hosting: Configure files for Firebase Hosting**
- **Use an existing project** → Pilih project Firebase Anda
- **What do you want to use as your public directory?** → `dist`
- **Configure as a single-page app?** → `Yes`
- **Set up automatic builds and deploys with GitHub?** → `No` (atau Yes jika mau)
- **File dist/index.html already exists. Overwrite?** → `No`

#### 4. Build Project

```bash
npm run build
```

#### 5. Deploy

```bash
firebase deploy
```

Setelah selesai, aplikasi Anda akan live di:
```
https://your-project-id.web.app
```

### Update Aplikasi

Setiap kali ada perubahan:

```bash
npm run build
firebase deploy
```

---

## ▲ Deploy ke Vercel

Vercel sangat cepat dan mudah untuk deploy aplikasi React/Vite.

### Opsi 1: Via Vercel Dashboard (Recommended)

1. Push code ke GitHub/GitLab/Bitbucket
2. Buka [vercel.com](https://vercel.com)
3. Klik **New Project**
4. Import repository Anda
5. Konfigurasi:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`
6. Tambahkan **Environment Variables** (copy dari `.env`):
   ```
   VITE_FIREBASE_API_KEY
   VITE_FIREBASE_AUTH_DOMAIN
   VITE_FIREBASE_PROJECT_ID
   VITE_FIREBASE_STORAGE_BUCKET
   VITE_FIREBASE_MESSAGING_SENDER_ID
   VITE_FIREBASE_APP_ID
   VITE_APP_NAME
   VITE_SCHOOL_NAME
   ```
7. Klik **Deploy**

### Opsi 2: Via Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy
npm run build
vercel --prod
```

URL aplikasi: `https://your-app.vercel.app`

---

## 🌐 Deploy ke Netlify

Netlify juga pilihan populer dengan fitur continuous deployment.

### Opsi 1: Via Netlify Dashboard

1. Push code ke GitHub/GitLab/Bitbucket
2. Buka [netlify.com](https://netlify.com)
3. Klik **New site from Git**
4. Pilih repository
5. Konfigurasi:
   - **Build command**: `npm run build`
   - **Publish directory**: `dist`
6. Tambahkan **Environment Variables** di Site Settings
7. Deploy

### Opsi 2: Drag & Drop

1. Build project:
   ```bash
   npm run build
   ```
2. Buka [app.netlify.com/drop](https://app.netlify.com/drop)
3. Drag folder `dist` ke area drop

### Opsi 3: Via Netlify CLI

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login
netlify login

# Deploy
npm run build
netlify deploy --prod
```

### Konfigurasi Redirects (Penting!)

Buat file `public/_redirects`:

```
/*    /index.html   200
```

Atau buat `netlify.toml`:

```toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

---

## 🐳 Deploy ke VPS (Manual)

Jika Anda punya VPS sendiri (DigitalOcean, AWS, dll).

### 1. Setup Server

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install Nginx
sudo apt install nginx -y
```

### 2. Upload Project

```bash
# Di komputer lokal
npm run build
scp -r dist/* user@your-server-ip:/var/www/tata-usaha
```

### 3. Konfigurasi Nginx

```bash
sudo nano /etc/nginx/sites-available/tata-usaha
```

Paste:

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/tata-usaha;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/tata-usaha /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 4. Setup SSL (Optional tapi Recommended)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your-domain.com
```

---

## 🔐 Environment Variables di Production

**PENTING**: Jangan commit file `.env` ke Git!

### Firebase Hosting

Environment variables sudah otomatis dari Firebase config.

### Vercel/Netlify

Tambahkan di dashboard:
- Vercel: Project Settings → Environment Variables
- Netlify: Site Settings → Build & Deploy → Environment

---

## 📊 Monitoring & Analytics

### Firebase Analytics

Tambahkan di `src/config/firebase.js`:

```javascript
import { getAnalytics } from 'firebase/analytics'

export const analytics = getAnalytics(app)
```

### Google Analytics

Tambahkan di `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

---

## 🔄 CI/CD (Continuous Deployment)

### GitHub Actions untuk Firebase

Buat `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Firebase

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: '${{ secrets.GITHUB_TOKEN }}'
          firebaseServiceAccount: '${{ secrets.FIREBASE_SERVICE_ACCOUNT }}'
          channelId: live
          projectId: your-project-id
```

---

## ⚡ Optimasi Performance

### 1. Code Splitting

Sudah otomatis dengan Vite.

### 2. Lazy Loading

Update `src/App.jsx`:

```javascript
import { lazy, Suspense } from 'react'

const Dashboard = lazy(() => import('./pages/Dashboard'))
const SuratMasuk = lazy(() => import('./pages/SuratMasuk'))
// ... dst

// Wrap routes dengan Suspense
<Suspense fallback={<div>Loading...</div>}>
  <Routes>
    {/* routes */}
  </Routes>
</Suspense>
```

### 3. Image Optimization

Gunakan format WebP dan lazy loading untuk gambar.

### 4. Caching

Firebase Hosting dan Vercel/Netlify sudah otomatis handle caching.

---

## 🐛 Troubleshooting Deployment

### Build Error

```bash
# Clear cache
rm -rf node_modules package-lock.json
npm install
npm run build
```

### 404 on Refresh

Pastikan routing sudah dikonfigurasi:
- Firebase: otomatis (SPA)
- Vercel: otomatis
- Netlify: tambahkan `_redirects` atau `netlify.toml`
- VPS: konfigurasi Nginx `try_files`

### Environment Variables Tidak Terbaca

- Pastikan prefix `VITE_` untuk semua env vars
- Restart build setelah menambah env vars
- Di Vercel/Netlify, redeploy setelah menambah env vars

---

## ✅ Checklist Post-Deployment

- ✅ Aplikasi bisa diakses via URL
- ✅ Login berfungsi
- ✅ CRUD operations berfungsi
- ✅ Upload file berfungsi
- ✅ Export PDF/Excel berfungsi
- ✅ Dark mode berfungsi
- ✅ Responsive di mobile
- ✅ SSL/HTTPS aktif
- ✅ Performance score bagus (test di PageSpeed Insights)

---

## 📞 Support

Jika ada masalah saat deployment, hubungi:
- Email: support@sekolah.com
- WhatsApp: 08xx-xxxx-xxxx

---

**Selamat! Aplikasi Anda sudah live! 🎉**
