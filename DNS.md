# Pengertian

Domain Name System atau DNS adalah sistem yang menerjemahkan nama domain seperti www.google.com menjadi IP address (contoh: 192.0.0.1) agar bisa dipahami oleh komputer saat Anda mengakses sebuah website menggunakan nama domain.

# Cara Kerja

DNS bekerja dalam beberapa langkah menggunakan proses yang disebut DNS lookup atau resolution.

Misalnya saya ingin membuka website tokopedia . Anda kemudian mengetikkan nama domain www.tokopedia.com ke kolom alamat web browser. Nah, di sini saya sedang melakukan proses yang disebut **DNS Request (Permintaan DNS)**.

Komputer saya lalu akan mengecek penyimpanan lokalnya untuk mencari apakah ada record (data) untuk domain tersebut. DNS record adalah IP address yang terkait dengan **FQDN**.

> FQDN adalah nama domain lengkap yang menentukan lokasi komputer atau host dalam jaringan, termasuk di internet maupun jaringan lokal. Struktur FQDN terdiri dari hostname dan nama domain.
>
> ![fqdn](/img/fqdn.webp)
>
> - Hostname. Label yang ditetapkan pada perangkat atau layanan di jaringan. Hostname merupakan bagian dari nama domain yang membuat alamat IP jadi mudah diingat. Contohnya, “www” adalah hostname untuk www.hostinger.co.id, dan “en” adalah hostname untuk en.wikipedia.org.
>
> - Subdomain. Bagian ini terletak di sisi kiri domain utama, dan terkadang menunjukkan bagian dari domain yang lebih besar. Misalnya, support.hostinger.com memiliki subdomain “support” yang merupakan bagian dari hostinger.com. Namun, tidak semua domain memiliki elemen ini.
>
> - Nama domain. Terdiri dari domain tingkat kedua dan top-level domain (TLD). Misalnya, di hostinger.co.id, “hostinger” adalah domain tingkat kedua, dan “.co.id” adalah TLD.

Selanjutnya komputer akan mencari dalam file host dan cache. File host adalah file teks biasa yang mengarahkan hostname ke IP address dalam sistem operasi, sedangkan cache adalah data sementara yang disimpan oleh hardware atau software.

begini gambaran kerja dns dalam mengakses website:

![alt](/img/cara-kerja-dns.jpg)

1. DNS Resolver

   perantara utama antara komputer Anda dan DNS server lainnya. Fungsinya adalah untuk meneruskan permintaan ke DNS server lainnya lalu mengirimkannya kembali setelah dipenuhi.

2. Root Nameserver

   Root nameserver atau root DNS server adalah server yang paling tinggi dalam alur kerja DNS. Fungsinya bisa diibaratkan seperti ruang arsip. Setelah menerima permintaan dari recursive DNS resolver, root nameserver akan memeriksa TLD milik domain yang Anda buka. Kemudian, recursive resolver akan diarahkan olehnya ke namaserver TLD yang tepat.

3. TLD Nameserver

   DNS server yang bertugas untuk menyimpan dan mengelola informasi domain yang menggunakan TLD tertentu. Top-level Domain atau TLD adalah bagian akhir domain, seperti .com, .org, .online, dan .net.

4. Authoritative Nameserver

   Authoritative name server atau authoritative DNS server adalah server terakhir dalam proses resolusi DNS. Server ini menyimpan semua informasi yang terkait dengan domain yang Anda kunjungi, termasuk alamat IP.

   Recursiver resolver kemudian akan memperoleh IP address milik domain yang Anda kunjungi, lalu mengirimkannya kembali ke komputer Anda sehingga website yang Anda akses akan terbuka.

   Terakhir, resolver akan melakukan DNS caching, yaitu menyimpan IP address yang berhasil diperolehnya dari authoritative name server sebagai data cache. Jadi, ketika Anda kembali website yang sama, prosesnya bisa lebih singkat karena record bisa langsung diambil dari cache.

# DNS dalam kubernetes

DNS (Domain Name System) dalam Kubernetes adalah fitur yang digunakan untuk memberikan resolusi nama kepada layanan dan pod dalam kluster Kubernetes.

DNS membuat aplikasi dalam kluster saling berkomunikasi menggunakan **nama yang mudah diingat**, bukan alamat IP yang bersifat dinamis.

## Fungsi DNS dalam kubernetes

1. sebagai Resolusi service name

   DNS Kubernetes memungkinkan layanan diakses melalui nama domain seperti my-service.my-namespace.svc.cluster.local, tanpa perlu mengetahui IP address-nya. Nama domain ini mempermudah komunikasi antar-layanan dalam kluster.

2. Resolusi Nama Pod

   Kubernetes DNS juga dapat digunakan untuk mengakses pod secara langsung, terutama jika pod memiliki hostname atau subdomain yang telah dikonfigurasi.

3. Komunikasi Antar-Namespace

   Dengan DNS, Anda dapat mengakses layanan di namespace yang berbeda menggunakan format:

   ```bash
   <service-name>.<namespace>.svc.cluster.local
   ```

<br>
<br>

# Peran DNS dalam Security System

Domain Name System (DNS) adalah sistem yang menerjemahkan nama domain menjadi alamat IP. Dalam konteks keamanan, DNS memiliki peran penting dalam melindungi sistem dari ancaman siber, mencegah akses ke situs berbahaya, dan memastikan komunikasi jaringan yang aman. peran DNS dalam security sistem

## **DNS Filtering**

DNS Filtering adalah proses pemblokiran atau pengizinan akses ke suatu domain berdasarkan aturan tertentu yang diterapkan pada sistem resolusi DNS. Proses ini sering digunakan untuk keamanan siber, kontrol akses, atau pemblokiran konten berbahaya. Berikut adalah tahapan utama dalam proses DNS filtering:

1.  Saat pengguna mencoba mengakses sebuah situs web (misalnya, example.com), perangkat mereka akan mengirimkan permintaan DNS (DNS query) ke server DNS untuk menerjemahkan nama domain menjadi alamat IP.

2.  Permintaan DNS dikirim ke DNS resolver, yang biasanya merupakan server DNS dari ISP atau layanan DNS pihak ketiga seperti Google DNS (8.8.8.8), Cloudflare DNS (1.1.1.1), atau OpenDNS.

3.  Server DNS memiliki daftar aturan atau database yang berisi domain yang diperbolehkan (allow list) dan diblokir (block list).

4.  Jika domain diblokir, resolver akan:

    - Mengembalikan IP palsu yang mengarah ke halaman peringatan (misalnya, "Website ini telah diblokir").
    - Tidak memberikan jawaban (mengembalikan NXDOMAIN atau "domain not found").

    Jika domain diizinkan, resolver akan meneruskan permintaan ke DNS autoritatif dan mengembalikan alamat IP yang benar.

Filtering bisa dilakukan dengan berbagai metode:

- **Blacklisting**: Domain yang berbahaya atau tidak diinginkan diblokir berdasarkan daftar hitam (contoh: situs malware, phishing, atau pornografi).
- **Whitelisting**: Hanya domain tertentu yang diperbolehkan, sementara yang lainnya diblokir.
- **Category-based Filtering**: Menyaring berdasarkan kategori, misalnya memblokir situs perjudian atau media sosial.
- **Threat Intelligence Filtering**: Menggunakan database ancaman terbaru untuk mencegah akses ke domain yang mencurigakan.

DNS Filtering bekerja dengan menyaring permintaan DNS berdasarkan kebijakan keamanan tertentu. Metode ini efektif untuk meningkatkan keamanan, mengurangi ancaman malware, dan mengontrol akses ke konten tertentu, baik di lingkungan rumah, kantor, atau jaringan besar.

## DNS Firewall dan Threat Intelligence

DNS Firewall dan Threat Intelligence adalah dua mekanisme keamanan yang digunakan untuk melindungi jaringan dari ancaman siber dengan cara mencegah akses ke domain berbahaya. Berikut adalah penjelasan cara kerja keduanya:

### Cara Kerja DNS Firewall

DNS Firewall adalah sistem keamanan yang berfungsi untuk memfilter dan memblokir permintaan DNS ke domain yang dianggap berbahaya atau tidak diizinkan. Berikut langkah-langkah cara kerjanya:

1. Pengguna Mengajukan Permintaan DNS

   Saat pengguna mencoba mengakses sebuah situs web, perangkat mereka mengirimkan permintaan DNS ke server resolver untuk menerjemahkan domain menjadi alamat IP.

2. DNS Firewall Mengecek Database Keamanan

   DNS Firewall memiliki database domain berbahaya (blacklist) yang terus diperbarui.
   Database ini bisa berasal dari berbagai sumber, termasuk Threat Intelligence Feeds.
   DNS Firewall akan membandingkan permintaan domain dengan daftar yang ada.

3. Tindakan yang Dilakukan oleh DNS Firewall
   Jika domain terdeteksi sebagai ancaman, maka:

   - Memblokir akses dengan mengembalikan respons NXDOMAIN (domain tidak ditemukan).
   - Mengalihkan pengguna ke halaman peringatan (misalnya, “Situs ini diblokir”).
   - Melakukan logging dan notifikasi ke administrator untuk analisis lebih lanjut.
   - Jika domain tidak berbahaya, DNS resolver akan meneruskan permintaan ke DNS autoritatif untuk mendapatkan alamat IP situs tujuan.

### Cara Kerja Threat Intelligence dalam DNS Firewall

Threat Intelligence berperan dalam mendeteksi dan memblokir ancaman berdasarkan analisis data real-time. Berikut cara kerjanya:

1. Pengumpulan Data Ancaman

   Threat Intelligence mengumpulkan informasi dari berbagai sumber, seperti:

   - Reputasi Domain & IP (misalnya, domain yang sering digunakan untuk phishing).
   - Laporan serangan siber global (DDoS, malware, ransomware).
   - Threat Feeds dari organisasi keamanan (Cisco Talos, FireEye, CrowdStrike, dll.).
   - Honeypot dan Machine Learning untuk mendeteksi pola ancaman baru.

2. Analisis dan Klasifikasi Ancaman

   Data yang dikumpulkan akan dianalisis dengan teknik seperti:

   - Signature-based detection (mencocokkan domain/IP dengan daftar blacklist).
   - Behavioral analysis (mendeteksi pola aktivitas mencurigakan).
   - AI & Machine Learning untuk menemukan ancaman zero-day.

3. Penyebaran Database ke DNS Firewall

   Setelah dianalisis, data ancaman dikirim ke DNS Firewall dalam bentuk Threat Feeds.

   Database ini terus diperbarui secara otomatis
   untuk menangani ancaman terbaru.

4. Pencegahan Ancaman Secara Proaktif

   Saat ada permintaan DNS ke domain berbahaya, DNS Firewall langsung memblokir akses berdasarkan data dari Threat Intelligence.

   Administrator dapat menganalisis log serangan untuk mengidentifikasi pola serangan yang berulang.
