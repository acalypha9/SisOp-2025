Practices
1. Perbedaan multiprograming dan multitasking
2. Fungsi OS
3. Virtualisasi container
Jawab
1. Perbedaan Multiprogramming dan Multitasking
Multiprogramming dan multitasking merupakan dua konsep fundamental dalam sistem operasi yang berkaitan dengan pengelolaan dan eksekusi beberapa program atau tugas secara bersamaan. Meskipun tujuannya sama—yaitu memaksimalkan penggunaan CPU dan mengurangi waktu idle—kedua teknik ini memiliki pendekatan dan penerapan yang berbeda.
1.1 Multiprogramming
Multiprogramming merupakan teknik awal yang digunakan oleh sistem operasi, terutama pada komputer mainframe, untuk meningkatkan efisiensi CPU. Pada sistem multiprogramming, beberapa program dimuat ke dalam memori secara bersamaan. Ketika suatu program harus menunggu operasi input/output (I/O) atau terjadi interupsi, CPU tidak menganggur, melainkan dialihkan ke program lain yang siap dieksekusi. Dengan demikian, waktu idle CPU diminimalkan dan proses-proses dapat diselesaikan lebih cepat.

Pendekatan ini sangat berguna mengingat perbedaan kecepatan antara CPU yang sangat cepat dengan perangkat I/O yang cenderung lebih lambat. Dalam prakteknya, multiprogramming melibatkan teknik penjadwalan proses seperti First-Come-First-Served (FCFS), Shortest Job First (SJF), atau algoritma berbasis prioritas untuk menentukan urutan eksekusi. Meskipun multiprogramming meningkatkan efisiensi pemrosesan, teknik ini umumnya kurang menitikberatkan pada interaktivitas pengguna karena fokus utamanya adalah mengoptimalkan penggunaan CPU dalam lingkungan batch processing.

1.2 Multitasking
Multitasking merupakan evolusi dari multiprogramming yang dirancang untuk memberikan pengalaman interaktif bagi pengguna. Dengan multitasking, sistem operasi dapat menjalankan beberapa tugas seolah-olah berjalan secara simultan melalui mekanisme time-sharing. Di sini, CPU secara cepat melakukan context switching antar proses, sehingga setiap aplikasi mendapatkan jatah waktu yang cukup untuk memberikan respons yang cepat terhadap input pengguna.
Ada dua jenis multitasking yang umum diimplementasikan, yaitu:
•	Preemptive Multitasking: Sistem operasi secara otomatis menghentikan sementara proses yang sedang berjalan untuk memberikan kesempatan pada proses lain yang lebih prioritas. Pendekatan ini memastikan sistem selalu responsif, terutama pada lingkungan dengan banyak proses interaktif.
•	Cooperative Multitasking: Proses yang sedang berjalan secara sukarela melepaskan kontrol CPU setelah menyelesaikan bagiannya. Walaupun lebih sederhana, pendekatan ini berpotensi menyebabkan masalah jika salah satu proses tidak mengembalikan kontrol, sehingga dapat menghambat eksekusi proses lain.

Contoh nyata multitasking dapat dilihat pada komputer modern, di mana pengguna dapat menjalankan berbagai aplikasi—seperti mendengarkan musik, mengetik dokumen, dan membuka browser—secara bersamaan. Meskipun pada kenyataannya hanya satu proses yang dijalankan pada satu waktu, kecepatan context switching membuat semua aplikasi tampak berjalan paralel.
________________________________________
2. Fungsi Sistem Operasi (OS)
Sistem operasi (OS) adalah perangkat lunak inti yang menjadi penghubung antara perangkat keras dan aplikasi. Tanpa OS, interaksi pengguna dengan komputer akan sangat terbatas. OS mengelola sumber daya sistem secara efisien dan menyediakan lingkungan yang stabil bagi berbagai aplikasi untuk berjalan. Berikut adalah beberapa fungsi utamanya:’

a. Manajemen Proses
OS mengatur siklus hidup proses—dari pembuatan hingga penghentian. Fungsi ini melibatkan:
•	Penjadwalan Proses: Menggunakan algoritma seperti Round Robin, Shortest Job First, dan Multi-Level Queue untuk menentukan urutan eksekusi proses.
•	Context Switching: Proses berpindah secara cepat dari satu proses ke proses lain agar setiap tugas mendapatkan alokasi waktu CPU yang adil.
•	Sinkronisasi dan Komunikasi Antarproses: Menyediakan mekanisme untuk memastikan proses dapat berkolaborasi atau bersaing atas sumber daya tanpa konflik.
b. Manajemen Memori
Manajemen memori OS mencakup alokasi memori kepada proses secara efisien, serta pengelolaan memori virtual untuk memperluas kapasitas fisik RAM. Teknik yang digunakan antara lain:
•	Paging dan Segmentation: Mengorganisir memori dalam blok-blok yang lebih kecil untuk mengurangi fragmentasi.
•	Swapping: Memindahkan data antar memori utama dan penyimpanan sekunder untuk memastikan proses dapat berjalan meskipun memori terbatas.
•	Virtual Memory: Mengizinkan program menggunakan lebih banyak memori dari yang tersedia secara fisik melalui penggunaan ruang disk sebagai perpanjangan dari RAM.
c. Manajemen File
Fungsi ini menyediakan struktur penyimpanan data yang terorganisir dan aman, dengan beberapa tugas penting:
•	Pengaturan File dan Direktori: Menyusun file dalam hierarki yang mudah diakses dan dikelola.
•	Hak Akses dan Keamanan: Mengontrol siapa yang dapat membaca, menulis, atau mengeksekusi file.
•	Backup dan Pemulihan Data: Menjamin data tidak hilang akibat kerusakan perangkat atau kesalahan sistem.
d. Manajemen Perangkat Keras
OS bertugas memastikan semua perangkat keras dapat berkomunikasi dengan aplikasi melalui:
•	Driver Perangkat: Perangkat lunak khusus yang menerjemahkan perintah dari OS ke perintah yang dipahami perangkat keras.
•	Pengaturan Antarmuka Hardware: Mengelola koneksi antara CPU dan perangkat input/output seperti printer, keyboard, dan disk drive.
e. Keamanan dan Proteksi
Keamanan dalam OS melibatkan berbagai mekanisme untuk melindungi data dan sistem:
•	Otentikasi dan Otorisasi: Memastikan hanya pengguna yang berwenang yang dapat mengakses sistem.
•	Isolasi Proses: Memastikan setiap proses berjalan secara terpisah untuk mencegah penyalahgunaan sumber daya.
•	Enkripsi dan Firewall: Menjaga data agar tidak disusupi oleh pihak yang tidak diinginkan.
f. Antarmuka Pengguna (User Interface)
Untuk memudahkan interaksi, OS menyediakan antarmuka:
•	Antarmuka Grafis (GUI): Menampilkan ikon, menu, dan jendela yang memudahkan pengguna dalam mengoperasikan komputer.
•	Antarmuka Baris Perintah (CLI): Memungkinkan pengguna menjalankan perintah secara langsung melalui teks, yang sering digunakan oleh administrator sistem untuk kontrol yang lebih mendetail.
Fungsi-fungsi tersebut bekerja secara sinergis untuk menciptakan lingkungan komputasi yang tidak hanya stabil dan efisien, tetapi juga aman dan mudah dioperasikan oleh pengguna dari berbagai tingkat keahlian.
________________________________________
3. Virtualisasi Container
Virtualisasi container merupakan teknologi modern yang mengubah cara aplikasi dikembangkan, diuji, dan dideploy. Dengan mengemas aplikasi beserta seluruh dependensinya ke dalam satu unit terisolasi—disebut container—teknologi ini memungkinkan pemanfaatan sumber daya yang lebih optimal dan konsistensi lingkungan antar pengembangan dan produksi.
3.1 Konsep Dasar Virtualisasi Container
Berbeda dengan virtual machine (VM) yang mengharuskan setiap instance menjalankan sistem operasi tamu lengkap, container hanya membawa aplikasi dan pustaka yang dibutuhkan. Container berjalan langsung di atas kernel sistem operasi host, yang membuat overhead yang dihasilkan jauh lebih rendah. Pendekatan ini memungkinkan banyak container untuk dijalankan secara bersamaan pada satu host tanpa mengganggu performa sistem secara keseluruhan.
3.2 Keunggulan Virtualisasi Container
Beberapa keunggulan utama dari virtualisasi container antara lain:
•	Ringan dan Efisien: Karena tidak perlu memuat keseluruhan sistem operasi, container membutuhkan lebih sedikit sumber daya dan memiliki waktu booting yang sangat cepat.
•	Portabilitas Tinggi: Dengan mengemas semua dependensi dalam satu paket, container dapat dijalankan di berbagai lingkungan—baik itu pada mesin lokal, server, atau cloud—tanpa perlu konfigurasi ulang yang kompleks.
•	Skalabilitas yang Mudah: Container dapat dengan cepat diduplikasi atau dikurangi jumlahnya sesuai dengan kebutuhan trafik atau beban kerja. Ini sangat mendukung penerapan arsitektur microservices, di mana setiap layanan berjalan secara independen dan dapat diskalakan sesuai kebutuhan.
•	Isolasi yang Kuat: Setiap container berjalan secara terpisah, sehingga jika terjadi kegagalan pada satu aplikasi, container lainnya tidak akan terpengaruh. Isolasi ini juga meningkatkan keamanan, karena setiap container memiliki batasan akses terhadap sumber daya sistem.
•	Dukungan untuk CI/CD: Container mendukung pengembangan berkelanjutan (continuous integration/continuous deployment) dengan memungkinkan update, rollback, dan deployment aplikasi dilakukan secara cepat dan efisien tanpa mengganggu keseluruhan sistem.
3.3 Perbandingan dengan Virtual Machine
Meskipun baik container maupun virtual machine digunakan untuk mengisolasi aplikasi, perbedaannya cukup mendasar:
•	Overhead Sistem: Virtual machine memerlukan hypervisor dan menjalankan sistem operasi tamu lengkap, yang menghasilkan overhead lebih besar dibandingkan container yang hanya mengemas komponen penting dari aplikasi.
•	Waktu Startup: Container memiliki waktu startup yang jauh lebih cepat karena tidak perlu mem-boot sistem operasi secara penuh, sedangkan VM membutuhkan waktu lebih lama.
•	Skalabilitas dan Efisiensi Sumber Daya: Container memungkinkan lebih banyak instance dijalankan pada satu host karena penggunaan sumber daya yang lebih ringan, sehingga lebih ideal untuk lingkungan cloud-native dan arsitektur microservices.
•	Manajemen dan Orkestrasi: Dengan munculnya platform seperti Docker dan alat orkestrasi seperti Kubernetes, pengelolaan container dalam skala besar menjadi lebih mudah dan efisien, memungkinkan deployment otomatis, load balancing, dan pemulihan kegagalan secara cepat.
