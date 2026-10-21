# Security Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memverifikasi **keamanan** sistem: akses tidak sah, autentikasi, kerahasiaan data, dan ketahanan terhadap input berbahaya. Pertanyaan inti: *"Bisakah orang tak berhak masuk, atau input jahat merusak sistem?"*
>
> ⚠️ **Etika & izin:** lakukan HANYA di environment testing/staging dengan izin. Jangan uji di production. Jangan pakai data/kredensial asli.
>
> **Apa yang diuji:** proteksi halaman, kebijakan password (NFR-04), anti-XSS/SQLi, autentikasi API, kerahasiaan kredensial (NFR-03), HTTPS, dan pemisahan hak akses (role). Terhubung ke `15-api-testcases.md` API-A.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Aman |
| ❌ | Rentan (catat sebagai bug/temuan keamanan) |
| ⚠️ | Perlu klarifikasi |

---

## Apa yang harus dilakukan?

1. Coba akses halaman tanpa login (ketik URL langsung) → harus diblokir.
2. Uji kebijakan & validasi password (NFR-04).
3. Masukkan input berbahaya di field (XSS, SQLi style) → harus di-escape/ditolak.
4. Uji API tanpa / dengan kredensial salah → harus 401/403.
5. Cek kredensial VMS tidak plain text (NFR-03) & semua lewat HTTPS.
6. Cek pemisahan hak akses (user biasa vs admin).

---

## TAHAP A — Autentikasi & Sesi

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SEC-A-01 | Akses URL menu langsung tanpa login | Diblokir → diarahkan ke login | ✅ | diarahkan ke page login |
| SEC-A-02 | Ubah password dengan password lemah | Ditolak (NFR-04) | ✅ | `11` UI-A8-02 |
| SEC-A-03 | Diamkan hingga session timeout | Diminta login ulang | ✅ | `11` UI-D-04 |
| SEC-A-04 | Pakai token/session lama setelah logout | Token invalid, akses ditolak | ✅ | diarahkan ke halaman page |
| SEC-A-05 | Login berkali-kali dengan password salah | Ada pembatasan/lockout | ⚠️ | tidak ada pembatasan atau lockout, stuck di login page sampai benar |

---

## TAHAP B — Input Berbahaya

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SEC-B-01 | Input `<script>alert(1)</script>` di search/filter | Di-escape, tidak dieksekusi (anti-XSS) | ✅ | tidak ada hasil |
| SEC-B-02 | Input `' OR 1=1 --` di field pencarian | Ditolak/di-escape (anti-SQLi) | ✅ | tidak ada hasil |
| SEC-B-03 | Input karakter spesial/panjang ekstrem | Ditangani, tidak crash | ✅ | tidak crash |

---

## TAHAP C — Keamanan API

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SEC-C-01 | API tanpa header Authorization | 401 Unauthorized | ✗ | Tetap 200 + dapat token (`12` API-A-02) |
| SEC-C-02 | API dengan kredensial salah | 401/403 | ✓ | 401 Unauthorized (`12` API-A-03) |
| SEC-C-03 | Cek semua endpoint via HTTPS | Terenkripsi | ✓ | via HTTPS (`12` API-A-05, F-04) |
| SEC-C-04 | API dengan Basic Auth ter-encode salah | 400/401, bukan diterima | ✗ | Tetap 200 + token (`12` API-A-04) |
| SEC-D-04 | Cek token tidak muncul di log | Tidak bocor (NFR-03) | ✗ | Token muncul di log Postman (`12` API-F-03) |

---

## TAHAP D — Kerahasiaan Data & Hak Akses

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SEC-D-01 | Cek penyimpanan kredensial VMS | Tidak plain text (NFR-03) | ⚠️ | password dan username dalam bentuk plain text |
| SEC-D-02 | Cek data sensitif tidak muncul di URL/log | Tidak bocor | ✅ | tidak ada di url |
| SEC-D-03 | Login user biasa → akses menu admin | Ditolak (role-based) | ⚠️ | login admin, bisa menjadi super admin|

---

**note:** Area keamanan masih banyak yang **belum diuji** (☐) — terutama proteksi akses langsung (SEC-A-01), anti-XSS/SQLi (SEC-B), dan autentikasi API (SEC-C). Yang sudah terverifikasi baru kebijakan password (SEC-A-02) dan session timeout (SEC-A-03). Ini area prioritas tinggi untuk dilengkapi, apalagi terkait task PAM.

---

## Exit Criteria
- Tidak ada akses tanpa autentikasi yang berhasil.
- Input berbahaya tidak dieksekusi (anti-XSS/SQLi).
- Kredensial & koneksi aman (NFR-03), hak akses terpisah sesuai role.
