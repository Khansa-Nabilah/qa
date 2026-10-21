# Functional Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memverifikasi bahwa **setiap fitur berjalan sesuai requirement** (REQ-01…REQ-10). Pertanyaan inti: *"Apakah fiturnya melakukan apa yang seharusnya?"*
>
> **Apa yang diuji:** jalur normal (happy path) tiap fitur di Panel dan AxxonNet, plus fungsi API inti. Fokus pada *fungsi*, bukan tampilan (UI/Visual) atau kemudahan pakai (Usability).
>
> Dokumen ini merangkum cakupan fungsional lintas `10-ui-testcases.md` (UI) dan `15-api-testcases.md` (API), terhubung ke `01-requirement-analysis.md`. Hasil eksekusi dicatat di `05-test-execution-log.md`, bug di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi (Not Executed) |
| ✅ | Pass |
| ❌ | Fail (catat bug di `06-defect-bug-log.md`) |
| ⚠️ | Blocked / berfungsi sebagian |

Setiap test case: isi kolom **Hasil** (✅/❌/⚠️) dan **Catatan** saat eksekusi.

---

## Apa yang harus dilakukan?

1. Ambil daftar requirement (REQ-01…REQ-08) dari `01-requirement-analysis.md`.
2. Untuk tiap fitur, lakukan aksi normal → verifikasi hasil sesuai *Expected Result*.
3. Tandai Pass/Fail + lampirkan bukti (screenshot).
4. Fitur gagal → catat di `06-defect-bug-log.md` dan beri referensi silang.

---

## TAHAP A — Fungsional Panel (Frontend)

### A1. Autentikasi (REQ-01)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A1-01 | Login dengan email + password benar | Masuk ke dashboard | ✅ | lihat `11` UI-A1-01 |
| FN-A1-02 | Logout | Kembali ke halaman login, sesi berakhir | ✅ | lihat `11` UI-A1-04 |

### A2. Events Report (REQ-06)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A2-01 | Buka menu Events Report | Tabel tampil lengkap dengan kolom | ✅ | lihat `11` UI-A2-01 |
| FN-A2-02 | Terapkan filter (event/kamera/tanggal) + Filter Data | Tabel ter-update sesuai filter | ✅ | lihat `11` UI-A2-02…04 |
| FN-A2-03 | Klik Unduh Data | File Excel terunduh & isinya sama dengan tabel | ✅ | lihat `11` UI-A2-07/08 |

### A3. CCTV (REQ-04)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A3-01 | Buka menu CCTV | Daftar kamera + kolom lengkap tampil | ✅ | lihat `11` UI-A3-01 |
| FN-A3-02 | Search, filter status, pagination 10/100 | Semua fungsi bekerja | ✅ | lihat `11` UI-A3-02…05 |
| FN-A3-03 | Klik Sync Data | Data tersinkron + indikator | ✅ | lihat `11` UI-A3-06 |
| FN-A3-04 | Action → Livestream & Edit | Livestream tampil; edit tersimpan | ✅ | lihat `11` UI-A3-07…09 |

### A4. Real-time Monitoring (REQ-05)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A4-01 | Pilih kamera → live | Stream tampil di card | ✅ | lihat `11` UI-A4-01 |
| FN-A4-02 | Putar kamera format HLS | Video jalan sebagai HLS | ⚠️ | HLS selalu auto-switch ke MP4 (`11` UI-A4-02) |
| FN-A4-03 | Putar kamera format MP4 | Video jalan | ✅ | `11` UI-A4-03 |

### A5. Dashboard & Laporan (REQ-02, REQ-03, REQ-07)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A5-01 | Buka Analytics Dashboard | Semua metrik tampil & masuk akal | ✅ | `11` UI-A5 |
| FN-A5-02 | Dashboard Summary → Lihat Data | Total kendaraan & traffic tampil | ✅ | `11` UI-A6 |
| FN-A5-03 | Daily Counting Report | Tabel detektor tampil | ✅ | `11` UI-A7 |

### A6. Account (REQ-08)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A6-01 | Ubah Profile (Nama/Email) | Tersimpan | ✅ | `11` UI-A8-01 |
| FN-A6-02 | Ubah password (valid) | Berhasil diubah | ✅ | `11` UI-A8-03 |

### A7. Master Data

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-A7-01 | Buka menu Branches | Data cabang tampil | ✅ | `11` UI-A9-01 |

---

## TAHAP B — Fungsional AxxonNet (Middleware)

### B1. Web Client & Daftar Kamera

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-B1-01 | Buka Web client | Semua kamera tampil di list | ✅ | `11` UI-B1-01 |
| FN-B1-02 | Sort A-Z, search, favorites | Semua fungsi bekerja | ✅ | `11` UI-B1-02…04 |

### B2. Live View & Archive

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-B2-01 | Klik kamera → live | Video live tampil | ✅ | `11` UI-B2-01 |
| FN-B2-02 | Mode archive → scrub & play | Playback berfungsi | ✅ | `11` UI-B4 |
| FN-B2-03 | Ganti layout grid | Multi-stream jalan | ⚠️ | Grid besar isi tak terlihat (`11` UI-B3-02) |

---

## TAHAP C — Fungsional API (Middleware)

### C1. ListCameras (REQ-04/REQ-10)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| FN-C1-01 | `ListCameras` dengan `data: {}` | 200 OK, semua kamera | ✅ | `12` API-B-01 |
| FN-C1-02 | `ListCameras` dengan `page_size`/`query` | 200 OK | ✅ | `12` API-B-02/03 |
| FN-C1-03 | `ListCameras` dengan field `view` | Harusnya 200 | ⚠️ | STRIPPED(string)→500, tapi FULL & integer 0/1/2→200 (`12` API-B-10/11/12) |

---

**note:** Dua temuan fungsional yang perlu tindak lanjut — (1) HLS yang selalu berpindah ke MP4 (FN-A4-02), dan (2) field `view` pada API `ListCameras` yang memicu 500 (FN-C1-03). Keduanya sebaiknya dicatat sebagai defect di `06-defect-bug-log.md`.

---

## Exit Criteria
- Semua fitur utama (REQ-01…REQ-08) sudah diuji minimal happy path-nya.
- Semua hasil Fail sudah tercatat di `06-defect-bug-log.md`.
- Cakupan fungsional terhubung ke RTM `08-traceability-matrix.md`.
