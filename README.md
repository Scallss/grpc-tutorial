Pascal Hafidz Fajri
2306222476

## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

Unary RPC mengirimkan satu request dan menerima satu response (seperti pada ProcessPayment). Ini cocok untuk operasi sederhana seperti autentikasi atau validasi. Server streaming RPC mengirimkan satu request dan menerima aliran respons berkelanjutan (seperti pada GetTransactionHistory). Ini ideal untuk pengambilan data historis atau notifikasi berkelanjutan. Bi-directional streaming RPC memungkinkan kedua pihak mengirim aliran pesan secara bersamaan (seperti pada Chat). Ini sangat sesuai untuk aplikasi realtime seperti obrolan, game multiplayer, atau layanan kolaborasi yang membutuhkan komunikasi dua arah yang cepat.

## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Implementasi layanan gRPC di Rust memerlukan beberapa pertimbangan keamanan penting:

- Autentikasi untuk memverifikasi identitas klien (misalnya token JWT atau OAuth2)
- Otorisasi untuk mengontrol akses ke layanan dan metode tertentu berdasarkan peran pengguna
- Enkripsi data melalui TLS untuk melindungi data dalam transit

Rust memberikan keuntungan keamanan bawaan dengan sistem tipe dan pengelolaan memori yang aman, tetapi developer tetap perlu menerapkan validasi input yang ketat, logging untuk audit keamanan, dan perlindungan terhadap serangan umum seperti DDoS atau man-in-the-middle.

## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Beberapa tantangan utama meliputi:

- Mengelola konkurensi dan sinkronisasi antara aliran pesan masuk dan keluar
- Menangani kegagalan koneksi dan pemulihan dengan elegan
- Memastikan pemrosesan efisien dari pesan dalam jumlah besar
- Mengimplementasikan logika timeout yang tepat, buffering pesan, dan backpressure
- Mencegah kebocoran memori atau kehabisan sumber daya saat klien mengirim pesan lebih cepat dari kemampuan server memprosesnya

Tantangan-tantangan ini dapat mengakibatkan degradasi kinerja atau bahkan kegagalan sistem jika tidak ditangani dengan baik.

## 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Kelebihan:

- Integrasi seamless dengan ekosistem async Tokio
- Manajemen backpressure default melalui channel dengan kapasitas terbatas
- Abstraksi yang mempermudah pembuatan dan pengelolaan aliran

Kekurangan:

- Overhead tambahan dari channel komunikasi yang dapat mempengaruhi kinerja pada beban tinggi
- Potensi kebocoran memori jika pengirim tidak dikelola dengan baik saat terjadi kesalahan
- Keterbatasan dalam kontrol aliran yang lebih terperinci untuk kasus penggunaan khusus


## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Struktur kode dapat diorganisir dengan:

- Memisahkan layanan, implementasi, dan logika bisnis menjadi modul terpisah
- Mengembangkan implementasi trait layanan di atas abstraksi yang mendukung pengujian dan simulasi
- Menggunakan dependency injection daripada instansiasi langsung
- Mengabstraksi kode bersama (seperti utilitas validasi, manajemen error, dan logika otorisasi) ke dalam crate terpisah

Pendekatan ini meningkatkan pengembangan dengan mengurangi duplikasi, memungkinkan pengujian unit yang lebih baik, dan memudahkan perluasan fungsionalitas dengan meminimalkan perubahan pada kode yang ada.


## 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Beberapa langkah tambahan yang diperlukan:

- Validasi input untuk memverifikasi ID pengguna, jumlah, dan detail pembayaran lainnya
- Integrasi dengan penyedia pembayaran atau gateway pihak ketiga
- Implementasi mekanisme rollback dan transaksi untuk menjaga integritas data
- Pencatatan audit komprehensif untuk kepatuhan dan pelacakan
- Logika bisnis canggih untuk otorisasi pembayaran, deteksi penipuan, dan pengelolaan batas pengeluaran
- Mekanisme notifikasi untuk memberi tahu pengguna tentang status transaksi melalui berbagai saluran


## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

gRPC memberikan:

- Kontrak yang jelas antara layanan melalui file Proto, memungkinkan pengembangan paralel
- Kinerja lebih baik dan penggunaan bandwidth lebih efisien berkat HTTP/2 dan Protocol Buffers
- Dukungan streaming dua arah
- Generasi kode klien dan server otomatis

Namun, adopsi gRPC juga menimbulkan:

- Kompleksitas tambahan dalam pengujian dan pemantauan
- Tantangan interoperabilitas dengan layanan web yang memerlukan gateway API untuk penerjemahan format
- Potensi kesulitan dengan beberapa firewall atau proxy yang tidak mendukung HTTP/2

## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

HTTP/2 menawarkan:

- Multiplexing permintaan melalui satu koneksi, mengurangi overhead TCP
- Kompresi header yang mengurangi overhead jaringan
- Prioritisasi aliran yang memungkinkan sumber daya penting diproses lebih dulu

Kekurangan HTTP/2:

- Kompleksitas implementasi yang lebih tinggi
- Kurangnya dukungan proxy yang luas
- Tantangan debugging karena format biner

WebSocket memberikan komunikasi full-duplex tetapi tidak memiliki fitur multiplexing HTTP/2 dan framework kontrak terstruktur seperti yang ditawarkan gRPC, meskipun WebSocket mungkin lebih cocok untuk aplikasi berbasis browser karena dukungan browser yang lebih baik.

## 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

REST API mengikuti paradigma request-response sinkron, di mana klien harus memulai setiap interaksi dan menunggu respons server sebelum melanjutkan. Ini cocok untuk operasi CRUD sederhana tetapi kurang optimal untuk pembaruan real-time.

Sebaliknya, gRPC dengan streaming dua arahnya memungkinkan komunikasi asinkron berkelanjutan antara klien dan server tanpa perlu polling berkelanjutan, meningkatkan responsivitas untuk aplikasi real-time seperti chat atau monitoring. Meskipun REST dapat meniru beberapa kemampuan ini dengan teknik seperti long polling atau WebSocket, gRPC menawarkan pendekatan yang lebih terintegrasi dan efisien dengan dukungan streaming bawaan dan penggunaan bandwidth yang lebih rendah.

## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Protocol Buffers memberikan keuntungan:

- Validasi tipe yang ketat
- Ukuran payload yang lebih kecil
- Kinerja serialisasi/deserialisasi yang lebih cepat
- Pengembangan yang lebih aman melalui kontrak eksplisit yang mencegah banyak kesalahan runtime

Kekurangan Protocol Buffers:

- Kurang fleksibel dibandingkan JSON
- Memerlukan kompilasi ulang ketika skema berubah

Sedangkan JSON menawarkan:

- Kemudahan debugging
- Fleksibilitas lebih tinggi untuk evolusi API
- Dukungan luas di berbagai platform dan bahasa

Tapi, Kekurangan JSON:

- Rentan terhadap kesalahan runtime karena kurangnya penegakan tipe data
- Payload yang lebih besar

Pilihan antara keduanya bergantung pada kebutuhan proyek, dengan gRPC lebih cocok untuk sistem internal berperforma tinggi dan REST/JSON untuk API publik yang memerlukan fleksibilitas dan aksesibilitas yang lebih luas.