# Laporan Analisis Statis: Real Malware (Cryptojacking ELF)

## Triage
- **Nama Sampel**: y11 / Varian Mirai-XMRig
- **Nama File Biner**: `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49.elf`
- **Ukuran File**: 8.350.992 bytes (~8.35 MB)
- **Tipe File**: Executable and Linkable Format (ELF) 64-bit LSB (Console Application)
- **MD5 Hash**: `e1b3738920012a07dfce8659b3ff31d`
- **SHA-256 Hash**: `b20f39fc00d242e706b6c30367ad811c676e0575050a4ec2f30104b696944b49`

## Strings
Ekstraksi teks statis dari section `.rodata` menggunakan Ghidra berhasil mengidentifikasi komponen operasional utama malware:
- **Identitas Engine**: `XMRIG_VERSION` / `XMRig/6.26.0`, mengonfirmasi penggunaan alat penambang kripto sumber terbuka yang disalahgunakan.
- **Target Koin**: Daftar koin yang ditargetkan seperti `Ravencoin`, `Zephyr`, `Wownero`, `YadaCoin`, dan `Graft`.
- **Protokol Komunikasi**: Perintah protokol Stratum seperti `mining.authorize`, `mining.subscribe`, dan `mining.set_target` yang digunakan untuk berkomunikasi dengan *Mining Pool*.

## Import Table
Biner ini bertipe *Stripped Binary* (informasi simbol dihapus), namun dependensi fungsional berikut teridentifikasi melalui analisis statis dan *log runtime*:
- **Manajemen Jaringan**: `libuv/1.51.0` untuk koneksi jaringan *asynchronous* yang persisten.
- **Keamanan**: `OpenSSL/3.0.16` untuk komunikasi terenkripsi dengan server kendali.
- **Optimasi CPU**: `hwloc/2.12.1` untuk pemetaan arsitektur perangkat keras dan instruksi perangkat keras `AES-NI` untuk mempercepat kalkulasi hash kriptografi.

## Kesimpulan
Biner `.elf` ini diklasifikasikan sebagai **Malware Cryptojacking** yang memanfaatkan perangkat keras korban untuk menambang aset kripto secara ilegal. Hasil analisis statis dan observasi dinamis menunjukkan bahwa malware ini bersifat parasit; ia secara otomatis melakukan *profiling* arsitektur CPU target, mengaktifkan akselerasi perangkat keras, dan memicu aktivitas jaringan keluar yang persisten untuk melapor ke server penambangan. Perilaku ini menyebabkan lonjakan utilisasi CPU hingga mendekati 100%, yang secara permanen merugikan sumber daya sistem target demi keuntungan finansial pihak penyerang.