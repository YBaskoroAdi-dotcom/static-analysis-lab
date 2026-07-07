# Laporan Analisis Statis: Real Malware Varian (y11.elf)

## Triage
- [cite_start]**Nama Sampel**: y11 / Varian Mirai-XMRig [cite: 86, 94]
- [cite_start]**Nama File Biner**: `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49.elf` [cite: 88]
- [cite_start]**Ukuran File**: 8.350.992 bytes (Sekitar 8.35 MB) [cite: 89]
- [cite_start]**Tipe File**: Executable and Linkable Format (ELF) 64-bit LSB (Console Application) [cite: 90, 231]
- [cite_start]**MD5 Hash**: `e1b3738920012a07dfce8659b3ff31d` [cite: 93]
- [cite_start]**SHA-256 Hash**: `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49` [cite: 92]
- [cite_start]**Sumber Sampel**: Repositori Publik MalwareBazaar (abuse.ch) [cite: 84, 738]

## Strings
[cite_start]Melalui ekstraksi strings statis pada area memori `.rodata` menggunakan Ghidra, ditemukan bukti-bukti krusial terkait indikator kompromi (IoC) operasional biner[cite: 193, 335]:
- [cite_start]`XMRIG_VERSION` / `XMRig/6.26.0`: Konfirmasi mutlak penyalahgunaan engine penambang kripto open-source versi 6.26.0[cite: 194, 337].
- [cite_start]Nama koin target: `Ravencoin`, `Zephyr`, `Wownero`, `YadaCoin`, `ArQmA`, dan `Graft`[cite: 194, 344, 350, 354, 358, 362, 366].
- [cite_start]Algoritma hashing: `cryptonight/v8`, `cryptonight_monerov7`, dan `rx/0 (RandomX)`[cite: 194, 438, 462].
- [cite_start]Perintah instruksi kolam penambangan ilegal (Stratum Protocol): `mining.authorize`, `mining.subscribe`, `mining.set_target`, dan `mining.notify`[cite: 194, 547, 587, 595, 601].

## Import Table
[cite_start]Karena file biner ini berstatus *Stripped Binary* (informasi simbol debugging telah dihapus oleh peretas untuk mempersulit rekayasa balik manual), tabel impor fungsi konvensional disembunyikan[cite: 144, 235, 236, 651]. [cite_start]Namun, hasil dekompilasi statis dan dependensi internal biner berhasil mengidentifikasi komponen berikut[cite: 651]:
- [cite_start]**Library Terintegrasi**: Biner dikompilasi secara statis menggunakan GCC (Alpine 13.2.1) dengan membawa library internal `libuv/1.51.0` (manajemen jaringan asynchronous), `OpenSSL/3.0.16` (komunikasi aman terenkripsi), dan `hwloc/2.12.1` (mendeteksi topologi perangkat keras CPU)[cite: 143, 652].
- [cite_start]**Fungsi Khusus**: Ekstraksi biner memanggil instruksi akselerasi kriptografi `AES-NI` (Advanced Encryption Standard New Instructions) langsung di tingkat perangkat keras[cite: 653].

## Kesimpulan awal
[cite_start]Biner Linux `.elf` ini secara meyakinkan diklasifikasikan sebagai **Malware Cryptojacking**[cite: 15, 221, 731]. [cite_start]Berdasarkan karakteristik analisis statis, tujuan utama program jahat ini adalah menyusup ke sistem operasi target, mendeteksi arsitektur spesifikasi CPU secara otomatis, dan membajak seluruh sumber daya komputasi perangkat keras korban tanpa izin[cite: 27, 658, 683, 731]. [cite_start]Sumber daya tersebut diperas secara ekstrem untuk melakukan kalkulasi algoritma hash penambangan kripto CryptoNight/RandomX demi menghasilkan keuntungan finansial bagi penyerang secara diam-diam[cite: 658, 727, 731].