# Integration / End-to-End Testing — AxxonNet ↔ Panel

> **Tujuan.** Memverifikasi **alur data lengkap** antar komponen: kamera → AxxonNet (middleware) → database → Panel (frontend). Pertanyaan inti: *"Apakah data yang dihasilkan middleware tampil konsisten & benar di frontend?"*
>
> ⭐ **Paling bernilai** karena di sinilah bug tersembunyi muncul — data bisa benar di satu sisi tapi salah saat ditampilkan di sisi lain.
>
> **Apa yang diuji:** konsistensi event, kamera, status, snapshot, dan konversi waktu (UTC→WIB) antara AxxonNet dan Panel. Terkait REQ-09/REQ-10, NFR-01, NFR-05. Terhubung ke `10-ui-testcases.md` TAHAP C dan kontrak `11-event-schema-contract.md`.

---

## Legenda Status

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Konsisten |
| ❌ | Tidak konsisten (catat bug) |
| ⚠️ | Perlu perhatian (konsisten dengan syarat) |

---

## Apa yang harus dilakukan?

1. Pilih 1 event/data di AxxonNet, catat detail: kamera, timestamp, jenis, snapshot.
2. Telusuri data yang sama di Panel → bandingkan apakah cocok persis.
3. Uji sinkronisasi status kamera (online/offline) antar kedua sistem.
4. Perhatikan transformasi data, terutama konversi timezone UTC→WIB.
5. Catat setiap ketidakcocokan sebagai bug integrasi.

---

## TAHAP A — Konsistensi Event

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| INT-A-01 | Catat 1 event di AxxonNet → cari di Panel Events Report | Timestamp, kamera, snapshot cocok | ✅ | AxxonNet UTC vs Panel WIB (`11` UI-C-01) |
| INT-A-02 | Bandingkan snapshot event AxxonNet vs Panel | Gambar sama | ✅ | `11` UI-C-04 |
| INT-A-03 | Event baru terdeteksi → cek muncul di Analytics | Metrik ikut ter-update | ✅ | metriknya juga ikut bertambah |
| INT-A-04 | Event baru → cek muncul di Daily Counting Report | Terhitung benar | ✅ | muncul |
| INT-A-05 | Hitung jumlah event di AxxonNet vs yang tampil Panel | Tidak ada event hilang (NFR-01) | ✅ | event di axxonnet cenderung lebih banyak dikarenakan mereka ada detector lainnya berupa plate recognition, namun untuk vehicle yang ada di events ada di panel juga|

---

## TAHAP B — Konsistensi Kamera & Status

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| INT-B-01 | Hitung jumlah kamera AxxonNet vs Panel CCTV | Konsisten / selisih terjelaskan | ✅ | 87 berdetektor; sisanya tanpa detektor (`11` UI-C-02) |
| INT-B-02 | Set/temukan kamera offline di AxxonNet → cek Panel | Status offline tercermin di Panel | ❌ | **BUG** kamera 83 "aktif" di Panel (`11` UI-C-03) |
| INT-B-03 | Jalankan Sync Data di CCTV → bandingkan dengan AxxonNet | Data tersinkron | ✅ | terdapat gap antara sinkronisasi data hub dan panel dikarenakan antrian yang random|
| INT-B-04 | Nonaktifkan kamera di AxxonNet → cek perubahan di Panel | Perubahan tercermin | ❌ | terlalu sensitif, perlu izin dari dev |

---

## TAHAP C — Konsistensi Waktu (Timezone) ⚠️

| ID | Langkah | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| INT-C-01 | Cek konversi waktu archive UTC→WIB | Selisih tepat 7 jam, tampil benar di Panel | ✅ | Dikonfirmasi via API: Axxon UTC, Kamera & Panel WIB (`12` API-D-02) |
| INT-C-02 | Cek event dekat tengah malam (mis. 23:30 WIB) | Tidak bergeser tanggal karena konversi | ✅ | meskipun di events dia jam 22 tapi dia sudah masuk ke esok harinya |

---

**note:** Dua temuan integrasi paling penting — (1) **kamera 83 mati di AxxonNet tapi tampil "aktif" di Panel** (INT-B-02), menandakan sinkronisasi status tidak akurat; dan (2) **perbedaan UTC (AxxonNet) vs WIB (Panel)** sebesar 7 jam (INT-C-01), yang berisiko menampilkan waktu/tanggal salah jika konversi tidak konsisten. Aturan resmi timezone ada di `11-event-schema-contract.md` §3.

---

## Exit Criteria
- Data event, kamera, dan status konsisten antara AxxonNet & Panel.
- Konversi timezone benar & terdokumentasi (tidak ada pergeseran tanggal).
- Tidak ada event hilang dalam perjalanan middleware → frontend (NFR-01).
