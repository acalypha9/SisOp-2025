<code>Nama : Fahroldhi Sukirno</code></br>
<code>NRP : 3124500046</code></br>
<code>Kelas : D3 Teknik Informatika</code></br>
<h1>Practices</h1>

<b>1. Perbedaan Multiprogramming dan Multitasking</b>

Multiprogramming dan multitasking merupakan dua konsep fundamental dalam sistem operasi yang berkaitan dengan pengelolaan dan eksekusi beberapa program atau tugas secara bersamaan. Meskipun tujuannya sama—yaitu memaksimalkan penggunaan CPU dan mengurangi waktu idle—kedua teknik ini memiliki pendekatan dan penerapan yang berbeda.

<h2>1.1 Multiprogramming</h2>

Multiprogramming merupakan teknik awal yang digunakan oleh sistem operasi, terutama pada komputer mainframe, untuk meningkatkan efisiensi CPU. Pada sistem multiprogramming, beberapa program dimuat ke dalam memori secara bersamaan. Ketika suatu program harus menunggu operasi input/output (I/O) atau terjadi interupsi, CPU tidak menganggur, melainkan dialihkan ke program lain yang siap dieksekusi. Dengan demikian, waktu idle CPU diminimalkan dan proses-proses dapat diselesaikan lebih cepat.

Pendekatan ini sangat berguna mengingat perbedaan kecepatan antara CPU yang sangat cepat dengan perangkat I/O yang cenderung lebih lambat. Dalam prakteknya, multiprogramming melibatkan teknik penjadwalan proses seperti First-Come-First-Served (FCFS), Shortest Job First (SJF), atau algoritma berbasis prioritas untuk menentukan urutan eksekusi. Meskipun multiprogramming meningkatkan efisiensi pemrosesan, teknik ini umumnya kurang menitikberatkan pada interaktivitas pengguna karena fokus utamanya adalah mengoptimalkan penggunaan CPU dalam lingkungan batch processing.

<h2>1.2 Multitasking</h2>

Multitasking merupakan evolusi dari multiprogramming yang dirancang untuk memberikan pengalaman interaktif bagi pengguna. Dengan multitasking, sistem operasi dapat menjalankan beberapa tugas seolah-olah berjalan secara simultan melalui mekanisme time-sharing. Di sini, CPU secara cepat melakukan context switching antar proses, sehingga setiap aplikasi mendapatkan jatah waktu yang cukup untuk memberikan respons yang cepat terhadap input pengguna.

Ada dua jenis multitasking yang umum diimplementasikan, yaitu:
<ul>
<li><b>Preemptive Multitasking</b>: Sistem operasi secara otomatis menghentikan sementara proses yang sedang berjalan untuk memberikan kesempatan pada proses lain yang lebih prioritas. Pendekatan ini memastikan sistem selalu responsif, terutama pada lingkungan dengan banyak proses interaktif.</li>
<li><b>Cooperative Multitasking</b>: Proses yang sedang berjalan secara sukarela melepaskan kontrol CPU setelah menyelesaikan bagiannya. Walaupun lebih sederhana, pendekatan ini berpotensi menyebabkan masalah jika salah satu proses tidak mengembalikan kontrol, sehingga dapat menghambat eksekusi proses lain.</li>
</ul>

Contoh nyata multitasking dapat dilihat pada komputer modern, di mana pengguna dapat menjalankan berbagai aplikasi—seperti mendengarkan musik, mengetik dokumen, dan membuka browser—secara bersamaan. Meskipun pada kenyataannya hanya satu proses yang dijalankan pada satu waktu, kecepatan context switching membuat semua aplikasi tampak berjalan paralel.

---

<b>2. Fungsi Sistem Operasi (OS)</b>

Sistem operasi (OS) adalah perangkat lunak inti yang menjadi penghubung antara perangkat keras dan aplikasi. Tanpa OS, interaksi pengguna dengan komputer akan sangat terbatas. OS mengelola sumber daya sistem secara efisien dan menyediakan lingkungan yang stabil bagi berbagai aplikasi untuk berjalan. Berikut adalah beberapa fungsi utamanya:

<h2>2.1 Manajemen Proses</h2>
<ul>
<li><b>Penjadwalan Proses</b>: Menggunakan algoritma seperti Round Robin, Shortest Job First, dan Multi-Level Queue untuk menentukan urutan eksekusi proses.</li>
<li><b>Context Switching</b>: Proses berpindah secara cepat dari satu proses ke proses lain agar setiap tugas mendapatkan alokasi waktu CPU yang adil.</li>
<li><b>Sinkronisasi dan Komunikasi Antarproses</b>: Menyediakan mekanisme untuk memastikan proses dapat berkolaborasi atau bersaing atas sumber daya tanpa konflik.</li>
</ul>

<h2>2.2 Manajemen Memori</h2>
<ul>
<li><b>Paging dan Segmentation</b>: Mengorganisir memori dalam blok-blok yang lebih kecil untuk mengurangi fragmentasi.</li>
<li><b>Swapping</b>: Memindahkan data antar memori utama dan penyimpanan sekunder untuk memastikan proses dapat berjalan meskipun memori terbatas.</li>
<li><b>Virtual Memory</b>: Mengizinkan program menggunakan lebih banyak memori dari yang tersedia secara fisik melalui penggunaan ruang disk sebagai perpanjangan dari RAM.</li>
</ul>

<h2>2.3 Manajemen File</h2>
<ul>
<li><b>Pengaturan File dan Direktori</b>: Menyusun file dalam hierarki yang mudah diakses dan dikelola.</li>
<li><b>Hak Akses dan Keamanan</b>: Mengontrol siapa yang dapat membaca, menulis, atau mengeksekusi file.</li>
<li><b>Backup dan Pemulihan Data</b>: Menjamin data tidak hilang akibat kerusakan perangkat atau kesalahan sistem.</li>
</ul>

---

<b>3. Virtualisasi Container</b>

<h2>3.1 Konsep Dasar Virtualisasi Container</h2>

Berbeda dengan virtual machine (VM) yang mengharuskan setiap instance menjalankan sistem operasi tamu lengkap, container hanya membawa aplikasi dan pustaka yang dibutuhkan. Container berjalan langsung di atas kernel sistem operasi host, yang membuat overhead yang dihasilkan jauh lebih rendah. Pendekatan ini memungkinkan banyak container untuk dijalankan secara bersamaan pada satu host tanpa mengganggu performa sistem secara keseluruhan.

<h2>3.2 Keunggulan Virtualisasi Container</h2>
<ul>
<li><b>Ringan dan Efisien</b>: Karena tidak perlu memuat keseluruhan sistem operasi, container membutuhkan lebih sedikit sumber daya dan memiliki waktu booting yang sangat cepat.</li>
<li><b>Portabilitas Tinggi</b>: Dengan mengemas semua dependensi dalam satu paket, container dapat dijalankan di berbagai lingkungan tanpa perlu konfigurasi ulang yang kompleks.</li>
<li><b>Skalabilitas yang Mudah</b>: Container dapat dengan cepat diduplikasi atau dikurangi jumlahnya sesuai dengan kebutuhan trafik atau beban kerja.</li>
<li><b>Isolasi yang Kuat</b>: Setiap container berjalan secara terpisah, sehingga jika terjadi kegagalan pada satu aplikasi, container lainnya tidak akan terpengaruh.</li>
</ul>

Dengan munculnya platform seperti Docker dan alat orkestrasi seperti Kubernetes, pengelolaan container dalam skala besar menjadi lebih mudah dan efisien, memungkinkan deployment otomatis, load balancing, dan pemulihan kegagalan secara cepat.
