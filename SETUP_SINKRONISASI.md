# Sinkronisasi Online (multi user dan real-time)

1. Buka console.firebase.google.com, Add project, matikan Google Analytics.
2. Build > Realtime Database > Create Database (lokasi Singapore), mulai dengan locked mode.
3. Salin Database URL dari halaman Data, misalnya `https://nama-proyek-default-rtdb.asia-southeast1.firebasedatabase.app`.
4. Tab Rules, ganti isinya dengan aturan berikut. Ganti `KODE_RAHASIA_ANDA` dengan kode karangan Anda (minimal 12 karakter, huruf, angka, `_` atau `-`), lalu Publish:

```
{
  "rules": {
    ".read": false,
    ".write": false,
    "KODE_RAHASIA_ANDA": {
      ".read": true,
      ".write": true,
      "$lain": { ".validate": "$lain === 'u' || $lain === 'ruangan' || $lain === 'tamu' || $lain === 'kat' || $lain === 'tipe' || $lain === 'paket' || $lain === 'rsv' || $lain === 'lock' || $lain === '_ts'" }
    }
  }
}
```

5. HP pertama (Admin): buka aplikasi, login admin / admin123, menu Akun > Hubungkan server, isi URL dan kode yang sama dengan Rules.
6. HP lain: di layar login tekan Hubungkan ke server, isi URL dan kode yang sama, lalu login dengan akun yang dibuat Admin.

Aturan `$lain` menolak data di luar struktur aplikasi. Jangan menghapus `lock` (kunci slot jam) dan `_ts` (penyamaan jam antar-HP dengan jam server).

Catatan keamanan: siapa pun yang tahu URL dan kode dapat membaca dan menulis data. Jaga kerahasiaan kode dan jangan dibagikan di luar tim.

Peran admin/approver/staf/manajer dicek di aplikasi, bukan di server. Hash sandi (PBKDF2) ikut tersimpan di database, jadi kode rahasia harus diperlakukan seperti sandi bersama. Penegakan di sisi server butuh Firebase Authentication dan rules berbasis `auth.uid`.

## Backup
Admin sebaiknya rutin memakai Akun > Backup data. File backup bisa dipulihkan lewat Akun > Restore dari file (bila terhubung ke server, data server ikut diganti).
