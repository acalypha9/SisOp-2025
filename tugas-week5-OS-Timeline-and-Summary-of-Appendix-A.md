# Practices

## 1. Pengujian Benchmark CPU dalam Satuan IOPS dan FLOPS

### 1. PENGUJIAN FLOPS & IOPS PADA CPU

#### 1. Pendahuluan
Benchmarking CPU merupakan metode yang digunakan untuk mengukur kinerja prosesor dalam menangani berbagai instruksi. FLOPS (Floating Point Operations Per Second) dan IOPS (Integer Operations Per Second) adalah dua metrik yang sering digunakan untuk mengukur performa CPU dalam perhitungan numerik dan operasi bilangan bulat. Pada laporan ini, kita akan menguji performa CPU menggunakan alat flops-iops yang tersedia di GitHub pada sistem operasi Ubuntu yang berjalan di VirtualBox.

#### 2. Tujuan
Tujuan dari laporan ini adalah untuk:
- Mengukur kinerja CPU dalam menangani operasi floating point (FLOPS) dan operasi bilangan bulat (IOPS) pada lingkungan virtual.
- Menganalisis hasil benchmark pada sistem operasi Ubuntu yang dijalankan di VirtualBox.

#### 3. Metode Pengujian

##### 3.1 Perangkat dan Perangkat Lunak
- **Perangkat Keras**: Laptop dengan prosesor Intel Core i5-11300H.
- **Sistem Operasi Host**: Windows.
- **Sistem Operasi Virtual**: Ubuntu.
- **Virtual Machine Software**: Oracle VM VirtualBox.
- **Alat Pengujian**: flops-iops dari [GitHub](https://github.com/ferryastika/flops-iops).

##### 3.2 Konfigurasi Virtual Machine
- **Jumlah Core CPU**: 4 Core.
- **RAM**: 8 GB.
- **Virtualization Support**: fitur VT-x/AMD-V aktif.

##### 3.3 Langkah-Langkah Pengujian
1. **Persiapan Lingkungan**
   - Instalasi dependensi:
     ```sh
     sudo apt update
     sudo apt install -y build-essential
     ```
   - Clone repositori flops-iops:
     ```sh
     git clone https://github.com/ferryastika/flops-iops.git
     cd flops-iops
     ```
   - Build program:
     ```sh
     make
     ```
2. **Pelaksanaan Pengujian**
   - Menjalankan benchmark FLOPS dan IOPS:
     ```sh
     ./iops32
     ./iops64
     ./flops32
     ./flops64
     ```

#### 4. Hasil dan Analisis
Setelah menjalankan pengujian, sistem akan menampilkan hasil benchmark dalam bentuk angka IOPS dan FLOPS. Berikut contoh hasil yang diperoleh dari pengujian dalam lingkungan virtual:

##### FLOPS 32-bit:
- Max Throughput CPU: **11.32 GIOPS**
- Max Single Core Throughput: **2.91 GIOPS**

![image](https://github.com/user-attachments/assets/d8340589-62a6-44d2-bccf-f500071fd2b9)


##### FLOPS 64-bit:
- Max Throughput CPU: **27.82 GIOPS**
- Max Single Core Throughput: **7.04 GIOPS**

![image](https://github.com/user-attachments/assets/24f93606-74a3-42be-8c71-92fb7b930890)


##### IOPS 32-bit:
- Max Throughput CPU: **11.48 GIOPS**
- Max Single Core Throughput: **3.25 GIOPS**

![image](https://github.com/user-attachments/assets/d1039dce-244f-49e4-93bb-b868226bb6b8)


##### IOPS 64-bit:
- Max Throughput CPU: **27.71 GIOPS**
- Max Single Core Throughput: **6.95 GIOPS**

![image](https://github.com/user-attachments/assets/2db9b2b7-ad67-4161-9d37-6a6e04fcdea3)


Dari hasil tersebut, dapat dilihat bahwa performa dalam lingkungan virtual lebih rendah dibandingkan pengujian langsung di hardware fisik. Hal ini disebabkan oleh overhead virtualisasi dan alokasi sumber daya yang terbatas pada VM.

#### 5. Kesimpulan
- FLOPS dan IOPS memberikan gambaran tentang performa CPU dalam menangani operasi floating-point dan operasi bilangan bulat dalam lingkungan virtual.
- Pengujian ini dapat membantu dalam mengevaluasi dampak virtualisasi terhadap kinerja prosesor.

---

## 2. Rangkuman Appendix-A

### A.1 Pendahuluan
Untuk memahami konsep dasar sistem operasi seperti penjadwalan CPU, manajemen memori, dan sistem berkas, kita dapat melihat bagaimana konsep-konsep ini diterapkan dalam beberapa sistem operasi awal yang berpengaruh.

Beberapa sistem operasi ini dirancang khusus untuk komputer besar (mainframes), sementara yang lain untuk komputer yang lebih kecil. Sebagian sistem tersebut unik dan tidak banyak digunakan, tetapi memiliki pengaruh signifikan pada sistem yang berkembang setelahnya.

**Tujuan utama dari bagian ini:**
- Menjelaskan bagaimana fitur sistem operasi berkembang dan berpindah dari komputer besar ke sistem yang lebih kecil.
- Menguraikan karakteristik beberapa sistem operasi historis yang paling berpengaruh.

### A.2 Migrasi Fitur dalam Sistem Operasi
Fitur yang awalnya dikembangkan untuk mainframe (komputer besar) kemudian digunakan dalam minicomputer dan akhirnya diterapkan pada mikrokomputer hingga perangkat genggam. Sebagai contoh:
- **MULTICS** adalah pelopor dalam konsep keamanan dan proteksi memori, yang kini digunakan di berbagai sistem operasi modern seperti Windows, Linux, dan macOS.

### A.3 Sistem Operasi Berpengaruh

![timeline_os](https://github.com/user-attachments/assets/db98207c-12d0-4f15-8e74-bfcc8c6417fa)


#### 1. MULTICS (Multiplexed Information and Computing Services)
- Dikembangkan oleh MIT, Bell Labs, dan General Electric pada tahun 1960-an.
- Memiliki fitur keamanan tingkat tinggi, virtual memory, dan hierarki sistem berkas.
- Menjadi inspirasi bagi Unix.

#### 2. UNIX
- Dikembangkan oleh Ken Thompson dan Dennis Ritchie di Bell Labs.
- Desain yang lebih sederhana dan modular.
- Menjadi dasar bagi sistem operasi modern seperti Linux dan macOS.

#### 3. OS/360
- Dikembangkan oleh IBM untuk komputer mainframe.
- Mendukung batch processing dan time-sharing.

#### 4. THE (Technische Hogeschool Eindhoven) Operating System
- Dikembangkan oleh Edsger Dijkstra.
- Memperkenalkan konsep **struktur lapisan**.

#### 5. XDS-940
- Sistem operasi time-sharing.
- Mempengaruhi desain sistem operasi multi-user berikutnya.

#### 6. CTSS (Compatible Time-Sharing System)
- Mengizinkan beberapa pengguna untuk mengakses komputer secara bersamaan.

#### 7. CP/M (Control Program for Microcomputers)
- Salah satu sistem operasi pertama untuk mikrokomputer.
- Menjadi dasar bagi MS-DOS.

#### 8. MS-DOS dan Windows
- MS-DOS berkembang dari CP/M dan digunakan secara luas pada komputer IBM.
- Windows diperkenalkan sebagai GUI untuk MS-DOS.

#### 9. MacOS
- Sistem operasi pertama yang secara luas mengadopsi GUI.
- Mempengaruhi desain sistem operasi modern.

### A.4 Konsep dan Inovasi dari Sistem Operasi Bersejarah
Beberapa konsep utama yang terus digunakan dalam sistem modern meliputi:
1. **Time-Sharing dan Multitasking**
   - Dikembangkan dari CTSS dan XDS-940.
2. **Hierarki Sistem Operasi (Layered Structure)**
   - Diperkenalkan oleh THE Operating System.
3. **Virtual Memory**
   - Dikembangkan dari MULTICS dan OS/360.
4. **Sistem Berkas Hierarkis**
   - Dikembangkan dalam UNIX dan MULTICS.
5. **Keamanan dan Proteksi**
   - MULTICS memperkenalkan keamanan yang kuat.
6. **Graphical User Interface (GUI)**
   - Pertama kali diperkenalkan dalam macOS dan Windows.
