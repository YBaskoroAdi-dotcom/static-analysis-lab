# Static Analysis Lab: Investigasi Malware Cryptojacking

Selamat datang di repositori laboratorium analisis statis saya. Repositori ini didedikasikan untuk mendokumentasikan proses eksplorasi dan pembedahan *binary* PE dan ELF yang saya temukan di lapangan.

## Status Proyek
Saat ini saya sedang berada dalam tahap awal investigasi terhadap sebuah sampel *binary* yang dicurigai sebagai agen *cryptojacking*. Saya mendokumentasikan setiap temuan secara *real-time* seiring dengan berkembangnya analisis yang saya lakukan.

## Temuan Awal
Saya baru saja mengamankan satu sampel biner (format ELF) dari repositori MalwareBazaar. Berikut adalah beberapa detail awal yang baru saja saya ekstrak:
- **Nama File**: `y11` (SHA256: `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49`)
- **Tipe File**: ELF 64-bit LSB (Executable)
- **Compiler**: GCC 13.2.1 (Alpine Linux)
- **Status Analisis**: Sedang berlangsung (Tahap identifikasi karakteristik awal)

## Dokumentasi Analisis
Saya sedang menyusun laporan teknis secara bertahap di folder `/docs`. Saat ini, saya baru saja menyelesaikan fase *triage* dan ekstraksi *strings* awal menggunakan Ghidra. Temuan sementara mengindikasikan adanya referensi ke protokol *mining* Stratum dan beberapa *string* mencurigakan terkait XMRig.

## Rencana Analisis Selanjutnya
Untuk memvalidasi temuan awal, saya akan melanjutkan ke tahap berikutnya:
1. Melakukan dekompilasi fungsi utama untuk memahami alur logika eksekusi.
2. Memetakan *Import Table* untuk melihat interaksi dengan sistem operasi.
3. Mencoba melakukan eksekusi dinamis di lingkungan *sandbox* terisolasi guna memverifikasi apakah benar terjadi lonjakan penggunaan CPU secara ekstrem.

## Disclaimer
Analisis ini dilakukan murni untuk tujuan pembelajaran akademis dalam mata kuliah Reverse Engineering. Seluruh prosedur dilakukan di dalam lingkungan VM yang terisolasi sepenuhnya untuk menjaga keamanan sistem utama.