# Negative / Error Handling Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Memverifikasi perilaku sistem saat diberi **input salah atau kondisi tidak normal**. Pertanyaan inti: *"Apakah sistem gagal dengan anggun (graceful) — pesan jelas, tidak crash, tidak 500?"*
>
> **Apa yang diuji:** input kosong/salah/ekstrem, kondisi tak normal (kamera offline, internet putus, data kosong), dan error handling API (input salah harus 4xx, BUKAN 500).
>
> Terhubung ke `10-ui-testcases.md` TAHAP D dan `15-api-testcases.md` API-B.2. Bug dicatat di `06-defect-bug-log.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Ditangani dengan baik (graceful) |
| ❌ | Gagal ditangani (crash/500/pesan buruk) |
| ⚠️ | Sebagian (satu sisi baik, sisi lain tidak) |

---

## Apa yang harus dilakukan?

1. Sengaja beri input salah: kosong, format salah, nilai ekstrem, karakter aneh.
2. Ciptakan kondisi tak normal: kamera offline, internet putus, data kosong.
3. Verifikasi sistem: pesan jelas, tidak crash, tidak error 500.
4. Khusus API: input salah harus menghasilkan 4xx yang sesuai, BUKAN 500.

---

## TAHAP A — Kondisi Tak Normal (UI)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| NEG-A-01 | Buka kamera offline | "No signal" rapi, tidak crash | ⚠️ | ✅ AxxonNet / ❌ Panel blank (`11` UI-D-01) |
| NEG-A-02 | Putus internet saat live view | Reconnect / pesan jelas | ✅ | `11` UI-D-02 |
| NEG-A-03 | Filter tanggal tanpa data | "Data kosong", bukan error | ✅ | `11` UI-D-03 |
| NEG-A-04 | Unduh Excel saat data kosong | Tidak error (file kosong wajar) | ✅ | `11` UI-D-05 |

---

## TAHAP B — Input Form Salah (UI)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| NEG-B-01 | Submit form field wajib kosong | Validasi menahan submit | ✅ | `11` UI-A1-03 |
| NEG-B-02 | tanggal yang diisi berupa tanggal masa depan (belum terjadi) | tidak ada hasil | ✅ | |
| NEG-B-03 | filter kosong | muncul pop up untuk memilih filter| ✅ | <img width="721" height="440" alt="image" src="https://github.com/user-attachments/assets/369fa8b9-01af-4e93-b4a3-cd8cf55537ef" /> |
| NEG-B-04 | filter kamera kosong di events report | muncul warning | ❌ | hasilnya seluruh kamera <img width="742" height="389" alt="image" src="https://github.com/user-attachments/assets/c2b2dc14-c304-423e-88e5-71f4b79db95f" /> |


---

## TAHAP C — Error Handling API

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| NEG-C-01 | `ListCameras` dengan field `view` | 200 / 4xx jelas, BUKAN 500 | ❌ | STRIPPED(string)→500; FULL & integer→200 (`12` API-B-10/11/12) <img width="548" height="446" alt="image" src="https://github.com/user-attachments/assets/9efcfc7b-2c0f-45c7-9b4b-a38b51468ce6" /> |
| NEG-C-02 | `page_size` negatif / 0 | Error jelas, bukan 500 | ⚠️ | 200 + next_page_token, tidak error (`12` API-B-16) <img width="647" height="472" alt="image" src="https://github.com/user-attachments/assets/ebeec895-177d-439c-8f46-eef962b48539" /> |
| NEG-C-03 | `page_token` acak/invalid | Error jelas, bukan 500 | ✅ | 502 Bad Gateway (`12` API-B-22) <img width="587" height="469" alt="image" src="https://github.com/user-attachments/assets/83db330c-e90a-496a-bd2d-37f5a51d7438" /> |
| NEG-C-04 | Field tidak dikenal di request | Diabaikan / error jelas | ❌ | 500 Internal Server Error (`12` API-B-17) <img width="571" height="419" alt="image" src="https://github.com/user-attachments/assets/f3cdeb35-46af-4ac5-aa78-a8784d492f87" /> |

---

**note:** Dua temuan negatif paling penting — (1) Panel menampilkan **blank screen** alih-alih "no signal" saat kamera offline (NEG-A-01).
---

## Exit Criteria
- Tidak ada input salah yang menyebabkan crash atau error 500.
- Semua kondisi tak normal menampilkan pesan yang jelas & rapi.
- Error handling API mengembalikan kode status yang sesuai (4xx, bukan 5xx).
