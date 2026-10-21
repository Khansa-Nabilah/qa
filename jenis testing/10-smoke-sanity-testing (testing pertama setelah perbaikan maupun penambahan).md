# Smoke / Sanity Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Pemeriksaan cepat untuk memastikan **fungsi paling inti bekerja** sebelum pengujian mendalam.
> - **Smoke Test:** "Apakah build ini layak diuji lebih lanjut?" (fungsi utama hidup)
> - **Sanity Test:** "Apakah bagian spesifik yang baru diubah masih waras?"
>
> ⏱️ **Durasi:** ~10–15 menit. Kalau Smoke Test gagal → **JANGAN lanjut** testing detail; kembalikan ke dev.
>
> Dijalankan setiap ada build/deploy baru, SEBELUM Functional/Regression. Terhubung ke `09-regression-testing.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Pass |
| ❌ | Fail (STOP — lapor ke dev, jangan lanjut) |

---

## Apa yang harus dilakukan?

1. Jalankan setiap kali ada build/deploy baru, SEBELUM testing mendalam.
2. Fokus hanya pada fungsi paling kritis (login, lihat kamera, lihat event).
3. Cek Console browser (F12) — pastikan tidak ada error merah.
4. Semua ✅ → lanjut testing lain. Ada ❌ → STOP & lapor.

---

## TAHAP A — Smoke Panel (Frontend)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SMK-A-01 | Buka Panel → login | Berhasil masuk ke dashboard | ✅ | |
| SMK-A-02 | Buka menu CCTV | Daftar kamera muncul | ✅ | |
| SMK-A-03 | Buka Events Report | Data/tabel tampil | ✅ | |
| SMK-A-04 | Buka 1 live view (Real-time Monitoring) | Stream jalan | ✅ | |
| SMK-A-05 | Buka Analytics Dashboard | Metrik tampil | ✅ | |

---

## TAHAP B — Smoke AxxonNet (Middleware)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SMK-B-01 | Buka AxxonNet → login | Berhasil masuk | ✅ | |
| SMK-B-02 | Daftar kamera muncul | Tampil | ✅ | |
| SMK-B-03 | Buka 1 kamera live | Stream jalan | ✅ | |

---

## TAHAP C — Smoke API

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SMK-C-01 | API `ListCameras` (`data: {}`) | 200 OK, data kembali | ✅ | |
| SMK-C-02 | API dengan auth benar | 200 | ✅ | |

---

## TAHAP D — Umum

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| SMK-D-01 | Cek Console browser (F12) | Tidak ada error merah | ✅ | |

---

**note:** Smoke Test adalah "gerbang" sebelum testing lebih dalam. Jika salah satu item TAHAP A–C gagal (❌), hentikan pengujian dan laporkan ke dev — tidak ada gunanya menguji detail bila fungsi inti saja sudah rusak.

---

## Exit Criteria
- Semua item Smoke ✅ → build dinyatakan layak diuji lebih lanjut.
- Ada ❌ → build ditolak, dilaporkan ke dev sebelum testing dilanjutkan.
