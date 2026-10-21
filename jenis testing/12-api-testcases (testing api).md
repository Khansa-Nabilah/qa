# 15 — API Test Cases: Axxon One HTTP/gRPC API

Owner: —   |   Status: 🟡 Draft — sebagian sudah terbukti nyata di sesi debugging

> **Tujuan.** Test case untuk API Axxon One (middleware) yang menjadi sumber data Panel. Mencakup: Authentication, Camera/Device (gRPC `ListCameras`), Event/Detector, Archive, Snapshot. Beberapa test case di sini **sudah dieksekusi nyata** saat sesi debugging (ditandai ✅/❌ + bukti); sisanya siap dieksekusi.
>
> **Tools:** Postman / Insomnia. **Auth:** HTTP Basic Auth. **Transport:** gRPC-over-HTTP (JSON) via gateway.
>
> **Terkait:** REQ-04 (CCTV), REQ-06 (Events), REQ-10 (API multi-merek kamera), NFR-02 (response time), NFR-03 (kredensial). Lihat juga `01-requirement-analysis.md`, `04-test-environment-setup.md`.
> untuk mengambil webclient_url, ambil di domain

---

## Environment (dari pengujian nyata tim — `04-test-environment-setup.md`)

| Item | Nilai |
|---|---|
| Server | Axxon One 3.0.2.49 (DetectorPack 3.16.1.158, DriverPack 3.89.467) |
| Host | WIN-QR0NE6H0MNS (Windows) |
| Auth | HTTP Basic Auth |
| Kamera uji | 8 (DeviceIpint 2,3,4,5,11,12,13,14) — RTSP (3) & Hikvision (5) |
| Base URL | `https://<host>/` (jangan commit host/kredensial asli — pakai placeholder) |

> ⚠️ **JANGAN pernah commit kredensial asli** (username/password/token) ke git. Simpan di `.env` lokal yang di-gitignore (lihat `04`).

---

## Legenda

| Simbol | Arti |
|---|---|
| ☐ | Belum dieksekusi |
| ✅ | Pass |
| ❌ | Fail (catat di `06-defect-bug-log.md`) |
| ⚠️ | Blocked |

---

## API-A — Authentication

| ID | Request | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-A-01 | Request endpoint valid **dengan** Basic Auth benar | 200 OK, data kembali | ✅ | kembali dengan bearer token |
| API-A-02 | Request **tanpa** header Authorization | 401 Unauthorized |  ❌ | tetap 200 dan mendapat token meskipun basic auth kosong |
| API-A-03 | Request dengan username/password **salah** | 401/403, tidak bocor info | ✅ | 401 Unauthorized |
| API-A-04 | Request dengan Basic Auth ter-encode salah (bukan base64 valid) | 400/401, bukan 500 | ❌ | tetap 200 dan mendapat token meskipun basic auth kosong |
| API-A-05 | Cek apakah kredensial dikirim via HTTPS (bukan HTTP polos) | Koneksi terenkripsi (NFR-03) | ✅ | |

---

## API-B — Camera / Device (gRPC `ListCameras`) ⭐ sebagian SUDAH DIUJI

Method: `axxonsoft.bl.domain.DomainService.ListCameras`

### B.1 Happy path & struktur

| ID | Request (`data`) | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-B-01 | `data: {}` (kosong) | 200 OK, kembalikan SEMUA kamera (items[]) | ✅ | **Terbukti di sesi debugging** — 29 kamera balik |
| API-B-02 | `data: { "page_size": 100 }` | 200 OK, daftar kamera (maks 100) | ✅ | **Terbukti** — page_size aman |
| API-B-03 | `data: { "query": { "query": "LT09" } }` | 200 OK, items + `search_meta_data` | ✅ | **Terbukti** — objek query aman |
| API-B-04 | Cek bahwa `query` TIDAK memfilter, hanya memberi skor | items tetap lengkap, skor di `search_meta_data` | ✅ | **Temuan penting:** filter dilakukan klien, bukan server |
| API-B-05 | Cek struktur tiap item kamera | Ada field: access_point, display_name, display_id, vendor, dll | ✅ | **Terbukti** dari response nyata |

### B.2 ⚠️ Bug nyata & negative cases

| ID | Request (`data`) | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-B-10 | `data: { "view": "VIEW_MODE_STRIPPED" }` | Seharusnya 200 dengan data ringkas | ❌ | **500 Internal Server Error.** Field `view` memicu crash.|
| API-B-11 | `data: { "view": "VIEW_MODE_FULL" }` | Seharusnya 200 data lengkap | ✅ | berhasil melihat seluruh kamera |
| API-B-12 | `data: { "view": 0 }` dan `{ "view": 1 }` (enum integer) | Konfirmasi apakah integer menghindari 500 | ⚠️ | 1, 0, 2 dapat digunakan untuk melihat isinya <img width="584" height="389" alt="image" src="https://github.com/user-attachments/assets/9e883cd7-a9da-42a5-8cb6-77fe4f676048" /> |
| API-B-13 | `page_size` dikirim sebagai string `"100"` vs angka `100` | Keduanya 200 (atau catat beda perilaku) | ✅ | keduanya bisa |
| API-B-14 | `search_fields` sebagai string vs integer vs array | Catat mana yang diterima | ✅ | yang diterima yang string |
| API-B-15 | `page_size` sangat besar (mis. 100000) | Ditangani wajar, tidak crash/timeout | ✅ | menampilkan seluruh info |
| API-B-16 | `page_size` negatif / 0 | Error handling benar (bukan 500) | ⚠️ | 200, menampilkan next page token |
| API-B-17 | Field tidak dikenal di `data` (mis. `"foo": "bar"`) | Diabaikan atau error jelas, bukan 500 | ❌ | hasilnya 500, <img width="557" height="353" alt="image" src="https://github.com/user-attachments/assets/eb843404-22b1-4d5f-9d61-4c4548ba2bc2" /> good news, dia error |
| API-B-18 | `decorated_name_template` tanpa keyword `{...}` | Error jelas / skor 0, bukan 500 | ⚠️ | dia 200, tapi return kosong |

### B.3 Pagination

| ID | Request | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-B-20 | Set `page_size` kecil (mis. 5), ambil `next_page_token` | Dapat token halaman berikutnya | ✅ | dapat token next page |
| API-B-21 | Kirim ulang dengan `page_token` tsb | Halaman berikutnya, tidak overlap | ✅ | <img width="580" height="358" alt="image" src="https://github.com/user-attachments/assets/61f3fd82-ab26-4669-8d6a-3e2c8528853c" /> |
| API-B-22 | `page_token` tidak valid/acak | Error jelas, bukan 500 | ✅ | 502 bad gateway |

---

## API-C — Event / Detector

Event types diketahui: `SmokeDetected`, `MotionDetected`, `faceAppeared`, `VehicleRecognized`.

| ID | Request | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-C-01 | Ambil daftar event (query dasar) | 200, daftar event dengan struktur konsisten | ✅ | event type dari seluruh server |
| API-C-02 | Cek struktur event terhadap schema standar | Ada id, origin, source, timestamp, type (lihat `11`) | ✅ | ada semua dengan type nya vehicle recognized <img width="740" height="491" alt="image" src="https://github.com/user-attachments/assets/800874d2-2cfb-43af-8f4f-d891a79bdecd" /> |
| API-C-03 | Filter event per kamera (`source`) | Hanya event kamera itu | ✅ | ada semua dengan type nya vehicle recognized <img width="740" height="491" alt="image" src="https://github.com/user-attachments/assets/800874d2-2cfb-43af-8f4f-d891a79bdecd" /> |
| API-C-04 | Filter event per rentang waktu | Hanya dalam rentang | ✅ | |
| API-C-05 | Filter per `type` (mis. VehicleRecognized) | Hanya type itu | ✅ | |
| API-C-06 | Cek `timestamp` event | Format UTC (`...Z`) | ✅ | ia dalam bentuk utc <img width="494" height="167" alt="image" src="https://github.com/user-attachments/assets/c4a22d88-5f32-4c6e-b806-68545995df0f" /> |
| API-C-07 | Query rentang waktu kosong (tidak ada event) | 200 dengan list kosong, bukan error | ✅ | keluar false dan 200 <img width="589" height="455" alt="image" src="https://github.com/user-attachments/assets/d10f6314-2bb4-456f-b177-d1f673a9e117" /> |
| API-C-08 | Cek tidak ada event duplikat | `id` unik | ✅ | id masing2 unik |

---

## API-D — Archive / Playback 

| ID | Request | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-D-01 | Minta rekaman kamera pada rentang waktu valid | 200, data/stream archive | ✅ | mendapat hasilnya secara rstp <img width="740" height="488" alt="image" src="https://github.com/user-attachments/assets/4ec8e2c6-5d85-43e7-8252-80e0f276a5ae" />|
| API-D-02 | Cek waktu yang diminta vs waktu yang dikembalikan | Konsisten dalam UTC | ✅ | Axxon pakai UTC, sedang Kamera dan Panel WIB (selisih 7 jam) |
| API-D-03 | Minta rekaman di waktu yang tidak ada rekaman | Respons jelas (kosong), bukan error | ✅ | 404 not found |
| API-D-04 | Minta rentang waktu terbalik (end < start) | Error handling benar | ✅ | 400 bad request|

---

## API-E — Snapshot

| ID | Request | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-E-01 | Ambil snapshot untuk 1 event | 200, gambar valid | ✅ | gambar snapshot tertangkap <img width="562" height="479" alt="image" src="https://github.com/user-attachments/assets/40c4e4ac-e830-4c71-afad-2f1a6f4c89d2" /> |
| API-E-02 | Cek gambar sesuai event (kamera & waktu benar) | Gambar relevan | ✅ | karena pemanggilan api nya mengambilan snapshot saat itu juga, jadi dia engga ada di axxonnet, tapi kamera dan waktunya benar |
| API-E-03 | Ambil snapshot event tidak ada | 404 / respons jelas, bukan 500 | ✅ | saat ke kamera yang non-existent maka dia akan 403|

---

## API-F — Non-Functional (NFR)

| ID | Pengujian | Expected Result | Hasil | Catatan |
|---|---|---|---|---|
| API-F-01 | Ukur response time `ListCameras` | ≤ X detik | ✅ | 665 ms <img width="727" height="419" alt="image" src="https://github.com/user-attachments/assets/ffa630b1-93ae-4f0d-a009-0e5f7781c340" /> |
| API-F-02 | Request `ListCameras` berulang cepat | Stabil, tidak error/timeout | ✅ | dilakukan selama 30x, tidak error maupun crash |
| API-F-03 | Kredensial tidak muncul di response/log | Tidak bocor (NFR-03) | ❌ | token authorization muncul di log postman |
| API-F-04 | Semua endpoint via HTTPS | Terenkripsi | ✅ | via https |

---

## Pemetaan ke Requirement

| REQ/NFR | Test Case API |
|---|---|
| REQ-04 (CCTV / daftar kamera) | API-B-01…B-22 |
| REQ-06 (Events) | API-C-01…C-08 |
| REQ-10 (API multi-merek kamera) | API-B, API-C |
| NFR-02 (response time) | API-F-01, F-02 |
| NFR-03 (kredensial aman) | API-A-05, F-03, F-04 |
| (schema kontrak) | API-C-02 → `11-event-schema-contract.md` |
