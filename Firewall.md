# Video Konsep Dasar Firewall

https://www.youtube.com/watch?v=fET2PYkckLo

# Pengertian Firewall

Firewall adalah sistem keamanan jaringan yang berfungsi sebagai penghalang antara jaringan internal (misalnya, jaringan perusahaan atau rumah) dan jaringan eksternal (seperti internet). Firewall bekerja dengan memantau, menyaring, dan mengontrol lalu lintas data berdasarkan aturan keamanan yang telah ditentukan.

## Fungsi Utama Firewall:

1. Mencegah Akses Tidak Sah – Firewall dapat memblokir akses dari pihak yang tidak berwenang ke jaringan internal.
2. Menyaring Lalu Lintas Data – Memeriksa paket data yang masuk dan keluar, serta mengizinkan atau menolak berdasarkan aturan keamanan.
3. Mencegah Serangan Berbahaya – Melindungi jaringan dari serangan seperti DDoS (Distributed Denial of Service), malware, dan hacking.
4. Membantu Manajemen Bandwidth – Dapat digunakan untuk membatasi penggunaan bandwidth untuk aplikasi tertentu.
5. Menerapkan Kebijakan Keamanan – Bisa digunakan untuk mengatur akses internet bagi pengguna dalam suatu jaringan.

# Jenis-Jenis Firewall:

1. Packet Filtering Firewall – Menyaring paket data berdasarkan alamat IP, port, dan protokol.
2. Stateful Inspection Firewall – Melakukan pemeriksaan yang lebih dalam terhadap koneksi yang sedang berlangsung.
3. Proxy Firewall – Bertindak sebagai perantara antara pengguna dan internet untuk menyembunyikan identitas jaringan internal.
4. Next-Generation Firewall (NGFW) – Menggabungkan fitur firewall tradisional dengan kemampuan deteksi ancaman modern seperti Intrusion Prevention System (IPS).

# Cara Kerja Firewall (General)

Firewall bekerja dengan membatasi akses ke jaringan dan sistem berdasarkan kriteria tertentu. Ini dilakukan dengan mengatur aturan dan kebijakan yang menentukan siapa yang dapat mengakses jaringan dan sistem, dan jenis data apa yang diperbolehkan masuk dan keluar dari jaringan.

Contohnya, jika sebuah organisasi ingin memblokir akses ke situs web yang berbahaya atau berisi konten yang tidak pantas, maka firewall dapat diperintahkan untuk memblokir semua akses ke situs web tersebut. Firewall juga dapat membatasi jenis data yang diperbolehkan masuk dan keluar dari jaringan, seperti email atau file yang terkait dengan bisnis organisasi.

Selain itu, firewall dapat memonitor lalu lintas jaringan dan melaporkan aktivitas yang mencurigakan. Jika ada aktivitas yang mencurigakan seperti serangan siber, firewall akan memberikan peringatan dan memblokir akses ke sistem atau jaringan untuk melindungi informasi dan data organisasi.

Firewall dapat diimplementasikan dengan menggunakan perangkat lunak atau perangkat keras, atau kombinasi dari keduanya. Firewall perangkat keras adalah perangkat yang berdiri sendiri dan terhubung ke jaringan, sedangkan firewall perangkat lunak adalah program yang diinstal pada komputer atau server.

# Cara Kerja Firewall (Kubernetes)

Dalam Kubernetes, firewall berfungsi untuk mengontrol lalu lintas jaringan antara **pod, node, dan klien eksternal**. Firewall dapat diterapkan di berbagai level, seperti di tingkat Cloud Provider, Node (host), atau Kubernetes itu sendiri.

1. Firewall di Tingkat Infrastruktur (Cloud Provider / Network Layer)

   Jika Kubernetes berjalan di cloud (misalnya AWS, GCP, atau Azure), biasanya cloud provider menyediakan firewall dalam bentuk Security Groups (AWS), Network Security Groups (Azure), atau VPC Firewall Rules (GCP).

   Firewall ini bekerja dengan menentukan aturan ingress (masuk) dan egress (keluar).

   - Bisa digunakan untuk membatasi akses ke kube-apiserver, worker nodes, dan load balancer.
     Contoh aturan firewall di AWS Security Group:

   - Izinkan port 6443 untuk akses ke kube-apiserver hanya dari IP tertentu.
   - Izinkan hanya lalu lintas internal antar worker nodes.

2. Firewall di Tingkat Node (Host Firewall)

   Setiap node Kubernetes adalah server (VM atau bare metal) yang memiliki firewall bawaan, seperti:

   iptables (Linux)
   firewalld (Linux)
   Windows Defender Firewall (Windows Node)

   Cara kerja:

   - Kubernetes menggunakan kube-proxy yang mengatur iptables untuk mengelola akses antar pod dan layanan.
   - Administrator bisa menambahkan aturan tambahan untuk membatasi akses SSH, kubelet API, atau service tertentu.

   Contoh aturan iptables untuk membatasi akses SSH hanya dari IP tertentu:

   ```bash
   iptables -A INPUT -p tcp --dport 22 -s 192.168.1.100 -j ACCEPT
   iptables -A INPUT -p tcp --dport 22 -j DROP
   ```

3. Firewall di Tingkat Kubernetes (Network Policies)

   Di dalam Kubernetes sendiri, firewall dapat diterapkan dengan Network Policy.

   Cara kerja:

   - Menggunakan CNI (Container Network Interface) seperti Calico, Cilium, atau WeaveNet untuk menerapkan aturan firewall.
   - Network Policy mengontrol komunikasi antar pod berdasarkan label, namespace, IP, atau port.

   Contoh Network Policy untuk membatasi akses hanya dari pod tertentu:

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
   name: allow-from-app
   namespace: default
   spec:
   podSelector:
       matchLabels:
       app: database
   policyTypes:
       - Ingress
   ingress:
       - from:
           - podSelector:
               matchLabels:
               app: backend
       ports:
           - protocol: TCP
           port: 5432
   ```

   Penjelasan:

   - Hanya pod dengan label app=backend yang bisa mengakses pod app=database pada port 5432 (PostgreSQL).
   - Pod lain akan diblokir.

4. Firewall di Tingkat Service (Ingress/Egress Rules)

   Untuk layanan yang diekspos ke publik, Kubernetes menyediakan Ingress Controller dan Egress Rules.

   - Ingress Firewall: Mengontrol akses masuk ke dalam cluster melalui Ingress Controller (misal: Nginx Ingress, Traefik).

   - Egress Firewall: Mengontrol akses keluar dari pod ke internet atau layanan lain.

   Contoh cara kerja Ingress Controller dengan firewall di Kubernetes:

   - Menggunakan Nginx Ingress untuk hanya mengizinkan akses ke domain tertentu.
   - Menggunakan Calico Egress Rules untuk memblokir akses pod ke internet kecuali ke API eksternal tertentu.
