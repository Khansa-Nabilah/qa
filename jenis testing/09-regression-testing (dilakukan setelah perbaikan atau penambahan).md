# Regression Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memastikan bahwa setelah ada **perbaikan bug atau fitur baru**: (a) perbaikan benar-benar bekerja, DAN (b) tidak merusak fitur lain yang tadinya sudah jalan. Pertanyaan inti: *"Apakah fix-nya berhasil tanpa menimbulkan masalah baru?"*
>
> **Apa yang diuji:** test case terkait fix (verifikasi), ditambah area sekitarnya (cek efek samping). Dijalankan setiap ada build/deploy baru.
>
> Suite ini dibangun dari bug yang ditemukan selama eksekusi UI & API. Terhubung ke `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Pass (fix berhasil / tidak ada regresi) |
| ❌ | Fail (fix gagal atau muncul regresi) |
| ⚠️ | Sebagian |

---

## Apa yang harus dilakukan?

1. Setiap dev memperbaiki bug / menambah fitur, catat area terdampak.
2. Jalankan **Smoke Test** dulu (lihat `10-smoke-sanity-testing.md`) sebelum regression penuh.
3. Jalankan ulang test case yang terkait langsung (verifikasi fix).
4. Jalankan juga test case di area sekitarnya (deteksi efek samping).
5. Catat hasil per siklus (tanggal/versi build).

---

## TAHAP A — Verifikasi Perbaikan Bug (Bug Fix Regression)

| ID | Pemicu (setelah fix) | Yang harus di-retest | Hasil | Catatan |
|---|---|---|---|---|
| REG-A-01 | Fix kamera 83 offline (INT-B-02) | UI-C-03, UI-D-01 Panel, + status beberapa kamera lain | ☐ | |
| REG-A-02 | Fix HLS→MP4 (FN-A4-02) | UI-A4-02/03 + live view kamera lain | ☐ | |
| REG-A-03 | Fix date picker Week (USA-A-04) | UI-A7-01 + laporan Harian/Bulanan | ☐ | |
| REG-A-04 | Fix API `view`→500 (NEG-C-01) | API-B-10/11 + request ListCameras lain | ☐ | |
| REG-A-05 | Fix filter Events Report (USA-A-02) | UI-A2 + filter di menu lain | ☐ | |
| REG-A-06 | Fix Panel blank saat offline (NEG-A-01) | UI-D-01 Panel + live view kamera offline lain | ☐ | |

---

## TAHAP B — Regression Inti (dijalankan tiap build)

| ID | Area | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| REG-B-01 | Login & logout | Berfungsi | ☐ | |
| REG-B-02 | CCTV list + search + pagination | Berfungsi | ☐ | |
| REG-B-03 | Events Report + filter + unduh | Berfungsi | ☐ | |
| REG-B-04 | Live view (Panel & AxxonNet) | Stream jalan | ☐ | |
| REG-B-05 | API ListCameras dasar (`data: {}`) | 200 OK | ☐ | |
| REG-B-06 | Analytics Dashboard metrik | Tampil benar | ☐ | |

---

**note:** Suite TAHAP A menunggu perbaikan dari dev — jalankan tiap bug ditutup untuk memverifikasi fix sekaligus cek regresi di area sekitarnya. TAHAP B adalah "jaring pengaman" yang wajib dijalankan setiap build untuk memastikan fungsi inti tidak rusak.

---

## Exit Criteria
- Semua bug yang diperbaiki terverifikasi fix (tidak Fail lagi).
- Tidak ada regresi — fitur lama tetap Pass.
- Hasil tiap siklus tercatat dengan versi build.
