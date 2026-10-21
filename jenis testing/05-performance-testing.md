# Performance / Load Testing — AxxonNet (Middleware) & Panel (Frontend)

> **Tujuan.** Mengukur **kecepatan, stabilitas, dan batas** sistem — terutama saat menangani banyak kamera/data sekaligus. Pertanyaan inti: *"Seberapa banyak beban yang bisa ditangani sebelum sistem melambat atau gagal?"*
>
> ⚠️ **TITIK KRITIS: 100 kamera.** Ini area paling rawan untuk sistem video seperti AxxonNet + Panel.
>
> **Apa yang diuji:** response time, penggunaan memory/CPU dari waktu ke waktu, batas jumlah stream, perilaku di bawah beban. Terkait NFR-01 (reliability) & NFR-02 (response time). Terhubung ke `10-ui-testcases.md` TAHAP B3 dan `15-api-testcases.md` API-F.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Memenuhi target |
| ❌ | Gagal / melebihi batas |
| ⚠️ | Perlu perhatian (perlu pemantauan lanjutan) |

---

## Apa yang harus dilakukan?

1. Buka DevTools → tab **Performance**, **Memory**, **Network**.
2. Jalankan skenario beban: banyak kamera live, grid besar, data 100 baris.
3. Catat: response time, jumlah stream maksimal sebelum gagal, tren memory.
4. Biarkan berjalan 10–15 menit untuk mendeteksi memory leak.
5. Bandingkan hasil dengan target NFR-02 (konfirmasi target ke mentor).

---

## TAHAP A — Beban Live View (100 kamera)

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| PRF-A-01 | Panel: buka live view banyak kamera serentak | Semua stream jalan tanpa blank | ❌ | Mentok ~11 kamera, sisanya blank (`11` UI-A4-04) |
| PRF-A-02 | AxxonNet: buka grid 2×2 | Stream jalan bersamaan | ✅ | `11` UI-B3-01 |
| PRF-A-03 | AxxonNet: buka grid 8×8 (banyak kamera) | Tidak crash & isi kamera terlihat | ⚠️ | Tidak crash tapi isi tak terlihat (`11` UI-B3-02) |
| PRF-A-04 | Ganti-ganti layout berkali-kali | Stream lama berhenti (koneksi tak menumpuk) | ✅ | `11` UI-B3-04 |

---

## TAHAP B — Stabilitas & Memory AxxonNet dan panel

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| PRF-B-01 | Biarkan grid aktif 10–15 menit, pantau Memory (DevTools) | Tidak ada memory leak signifikan | ✅ | <img width="236" height="158" alt="image" src="https://github.com/user-attachments/assets/a15332a0-94aa-4917-9875-d08bf4f2707a" /> Naik-turun seperti gigi gergaji (gergaji) karena dia sehat |
| PRF-B-02 | Pantau CPU browser saat grid penuh | Tidak 100% terus-menerus | ✅ | angka naik turun dari 3.0 ~ 41.0 <img width="537" height="160" alt="image" src="https://github.com/user-attachments/assets/1fa0e218-3778-4b9b-ac52-84a9620a017e" /> |
| PRF-B-03 | Buka beberapa tab Panel sekaligus | Tidak saling mengganggu/crash | ✅ | tidak melambat maupun crash |

---

## TAHAP C — Response Time (NFR-02) Panel

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| PRF-C-01 | Ukur waktu load daftar 100 kamera (CCTV) | ≤ target NFR-02 | ✅ | <img width="404" height="305" alt="image" src="https://github.com/user-attachments/assets/4869e9b8-1045-469f-b008-4cc79d8727cb" /> dalam waktu milisecond |
| PRF-C-02 | Ukur response time API `ListCameras` | ≤ target NFR-02 | ✅ | 665 ms (`12` API-F-01) |
| PRF-C-03 | Events Report tampilkan 100 baris | Load cepat, tidak freeze | ✅ | <img width="762" height="172" alt="image" src="https://github.com/user-attachments/assets/495f9c43-d129-4146-ac50-0da98dd3870b" /> |
| PRF-C-04 | Unduh Excel data besar | Selesai tanpa timeout | ❌ | gagal saat mencoba mendownload sebanyak 37685 baris <img width="784" height="479" alt="image" src="https://github.com/user-attachments/assets/49f6b8af-1fed-4fa7-a1b8-7135cbf72ba7" /> |
| PRF-C-05 | Request `ListCameras` berulang (30x) | Stabil, tidak error/timeout | ✅ | 30x tanpa error/crash (`12` API-F-02) |

---

**note (khusus 100 kamera):**
- Batas praktis live view Panel saat ini **~11 kamera**; lebih dari itu sebagian blank screen (PRF-A-01). rencananya bakal dibatasin.
- Grid 8×8 AxxonNet **tidak crash tapi isi tidak tampil** (PRF-A-03), layar tidak cukup.

---

## Exit Criteria
- Batas jumlah stream live terdokumentasi (Panel & AxxonNet).
- Tidak ada memory leak signifikan dalam pemantauan 15 menit.
- Response time memenuhi NFR-02 setelah target disepakati.
