# Sistem Gaji — Wonderland Park Samarinda (PWA)

Frontend statis (GitHub Pages) + backend Google Apps Script sebagai JSON API. Database tetap Google Sheets.

## 1. Pasang backend (Apps Script)
1. Buka proyek Apps Script yang sekarang dipakai. Ganti isi `Code.gs` dengan `Code.gs` dari folder ini.
2. **Hapus file `Index.html` dari proyek Apps Script** (tidak dipakai lagi).
3. Deploy ulang: **Deploy > Manage deployments > Edit (ikon pensil) > Version: New version > Deploy**.
   - Execute as: **Me**
   - Who has access: **Anyone**
4. Salin URL Web App yang berakhiran `/exec`.

> Instalasi baru (database belum ada): jalankan fungsi `initDatabase` sekali dari editor Apps Script.
> Setelah itu login dengan akun awal dari sheet `Users`, lalu **segera ganti password** di menu akun.

## 2. Isi alamat API di frontend
Edit `config.js`:
```js
window.APP_CONFIG = { API_URL: 'https://script.google.com/macros/s/XXXX/exec' };
```

## 3. Deploy ke GitHub Pages
```bash
git init && git add . && git commit -m "PWA Sistem Gaji"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```
Di GitHub: **Settings > Pages > Source: Deploy from a branch > main / (root)**. Alamat: `https://<user>.github.io/<repo>/`.

## 4. Pasang sebagai aplikasi (PC & HP)
Buka alamat GitHub Pages lewat **HTTPS**, lalu:
- **PC (Chrome / Edge):** klik ikon **Install** (monitor + panah) di ujung kanan address bar, atau tombol **📲 Pasang Aplikasi** (halaman login / menu samping), atau menu ⋮ > *Install Sistem Gaji*. Aplikasi terbuka di jendela sendiri dan muncul di Start Menu / Desktop.
- **Android (Chrome):** tombol **📲 Pasang Aplikasi**, atau menu ⋮ > *Install app* / *Add to Home screen*.
- **iPhone/iPad (Safari saja):** Bagikan ⬆️ > *Tambah ke Layar Utama*. Tombol **📲 Pasang Aplikasi** menampilkan panduannya.

Tombol pasang otomatis hilang kalau aplikasi sudah terpasang.

## Catatan
- Absensi/Lembur: klik kanan (desktop) = **tekan lama** (HP) pada sel untuk mengisi jam lembur & catatan.
- Setelah mengubah file frontend, naikkan `VERSION` di `sw.js` agar HP mengambil versi baru.
- Login tersimpan di perangkat sampai sesi berakhir (lama sesi di Pengaturan → `SESSION_JAM`).
- Data tidak tersedia offline (hanya tampilan aplikasi); butuh internet untuk memuat/menyimpan data.

## Hak akses per role
| Role | Menu yang bisa dibuka |
|---|---|
| Owner | Semua menu |
| HR | Semua kecuali Rekap Absensi Majoo |
| **Admin** | **Hanya Absensi Harian, Rekap Absensi Majoo, Rekap Lembur** (lihat + input) |
| Karyawan | Dashboard, Absensi, Lembur, Ranking KPI, Slip (milik sendiri) |

Pembatasan Admin dijaga di dua lapis: menu disembunyikan di frontend **dan** ditolak di server (`Code.gs`),
sehingga tidak bisa ditembus lewat pemanggilan API langsung. Untuk Admin, data karyawan yang dikirim server
juga sudah tanpa gaji, NIK, HP, dan alamat. Hapus data periode, tambah periode, dan kelola karyawan khusus Owner.
