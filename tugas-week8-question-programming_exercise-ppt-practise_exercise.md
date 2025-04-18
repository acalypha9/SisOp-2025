<code>Nama : Fahroldhi Sukirno</code></br>
<code>NRP : 3124500046</code></br>
<code>Kelas : D3 Teknik Informatika</code></br>
<code>Dosen Pengajar: Dr Ferry Astika Saputra ST, M.Sc</code></br>  

<h1>Week 8</h1>

## 1. Jelaskan dalam 2 pargraph disertai dengan gambar tentang konsep single thread dan multithread!

Konsep single-thread dan multithread menggambarkan bagaimana proses dieksekusi dalam sebuah program. Pada gambar pertama (single-threaded process), terlihat bahwa sebuah proses hanya memiliki satu thread yang berjalan. Thread ini terdiri dari register, stack, dan program counter (PC) sendiri, tetapi berbagi memori utama seperti code, data, dan file dengan proses induknya. Artinya, hanya satu tugas yang dapat dilakukan pada satu waktu, sehingga jika satu thread mengalami penundaan atau berhenti, seluruh proses akan ikut terpengaruh.

Sebaliknya, pada gambar kedua (multithreaded process), satu proses memiliki lebih dari satu thread yang berjalan secara bersamaan. Masing-masing thread memiliki stack, register, dan PC sendiri, namun tetap berbagi bagian-bagian penting dari memori seperti code, data, dan file. Pendekatan ini memungkinkan multitasking dalam satu proses, meningkatkan efisiensi dan performa karena beberapa bagian dari program bisa dijalankan secara paralel. Multithreading sering digunakan pada aplikasi modern untuk menjaga antarmuka tetap responsif saat menjalankan tugas-tugas berat di latar belakang.

<ul>
<li><b>Single-threaded</b></li><br>
  
![image](https://github.com/user-attachments/assets/82dae058-d0b6-431c-94fc-69e40fa98e5b)

<li><b>Multithreaded</b></li>

![image](https://github.com/user-attachments/assets/8e449b27-d8ba-4c25-bc5b-6fed2cfa354e)


</ul>

### Perbandingan

| **Aspek**           | **Single Thread**                                         | **Multithread**                                                |
| ------------------- | --------------------------------------------------------- | -------------------------------------------------------------- |
| **Cara Eksekusi**   | Tugas dijalankan secara berurutan (serial)                | Tugas dapat dijalankan secara bersamaan (paralel)              |
| **Responsivitas**   | Responsifitas bisa menurun bila ada tugas berat yang lama | Lebih responsif karena beberapa tugas diproses secara simultan |
| **Manajemen Debug** | Lebih mudah karena alur eksekusi yang jelas dan sederhana | Lebih kompleks akibat interaksi antar thread                   |
| **Skalabilitas**    | Terbatas, terutama pada aplikasi dengan banyak permintaan | Lebih baik untuk aplikasi kompleks dan pemrosesan multitasking |

## 2. Programming Exercise

#### **A. SumTask.java (Windows)**
```java

/**
 * Fork/join parallelism in Java
 *
 * Figure 4.18
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

import java.util.concurrent.*;

public class SumTask extends RecursiveTask<Integer> {
  static final int SIZE = 10000;
  static final int THRESHOLD = 1000;

  private int begin;
  private int end;
  private int[] array;

  public SumTask(int begin, int end, int[] array) {
    this.begin = begin;
    this.end = end;
    this.array = array;
  }

  protected Integer compute() {
    if (end - begin < THRESHOLD) {
      int sum = 0;
      for (int i = begin; i <= end; i++)
        sum += array[i];

      return sum;
    } else {
      int mid = begin + (end - begin) / 2;

      SumTask leftTask = new SumTask(begin, mid, array);
      SumTask rightTask = new SumTask(mid + 1, end, array);

      leftTask.fork();
      rightTask.fork();

      return rightTask.join() + leftTask.join();
    }
  }

  public static void main(String[] args) {
    ForkJoinPool pool = new ForkJoinPool();
    int[] array = new int[SIZE];

    java.util.Random rand = new java.util.Random();

    for (int i = 0; i < SIZE; i++) {
      array[i] = rand.nextInt(10);
    }

    SumTask task = new SumTask(0, SIZE - 1, array);

    int sum = pool.invoke(task);

    System.out.println("The sum is " + sum);
  }
}
```

#### **Output**

![image](https://github.com/user-attachments/assets/2b393d72-3e40-47c9-8f44-ae4b6c20e3a1)

#### **Apa yang terjadi ?**
Program ini bertujuan untuk menghitung jumlah seluruh elemen dalam array berisi 10.000 angka acak menggunakan Fork/Join Framework di Java.

Kelas <code>SumTask</code> memperluas <code>RecursiveTask\<Integer></code> dan digunakan untuk membagi tugas secara rekursif:

Jika jumlah elemen yang akan dijumlahkan kurang dari 1000, program menjumlahkan langsung dengan perulangan.

Jika lebih, array dibagi dua, dan dua tugas baru dibuat untuk masing-masing bagian.

Kedua tugas dijalankan secara paralel menggunakan <code>fork()</code>, dan hasilnya digabung dengan <code>join()</code>.

Tugas utama dijalankan oleh <code>ForkJoinPool</code>, yang secara otomatis mengatur pembagian thread. Dengan pendekatan ini, program memanfaatkan multithreading untuk mempercepat proses penjumlahan pada prosesor multi-core melalui paralelisme tugas (<b>task parallelism</b>).

---

#### **B. thrd-posix.c (Linux)**
```c

/**
 * A pthread program illustrating how to
 * create a simple thread and some of the pthread API
 * This program implements the summation function where
 * the summation operation is run as a separate thread.
 *
 * Most Unix/Linux/OS X users
 * gcc thrd.c -lpthread
 *
 * Figure 4.11
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

int sum;

void *runner(void *param);

int main(int argc, char *argv[])
{
pthread_t tid;
pthread_attr_t attr;

if (argc != 2) {
	fprintf(stderr,"usage: a.out <integer value>\n");
	return -1;
}

if (atoi(argv[1]) < 0) {
	fprintf(stderr,"Argument %d must be non-negative\n",atoi(argv[1]));
	return -1;
}

pthread_attr_init(&attr);

pthread_create(&tid,&attr,runner,argv[1]);

pthread_join(tid,NULL);

printf("sum = %d\n",sum);
}

void *runner(void *param) 
{
int i, upper = atoi(param);
sum = 0;

	if (upper > 0) {
		for (i = 1; i <= upper; i++)
			sum += i;
	}

	pthread_exit(0);
}
```

#### **Output**

![image](https://github.com/user-attachments/assets/d952e54a-8ef2-4b02-896d-09a190f2e71c)


#### **Apa yang terjadi ?**
Program ini dirancang untuk menghitung jumlah bilangan dari 1 hingga N menggunakan thread tambahan. Nilai N dimasukkan oleh pengguna sebagai argumen saat menjalankan program. Dengan menggunakan pustaka <code>pthread</code>, program membuat satu thread baru untuk melakukan proses penjumlahan, sedangkan thread utama hanya menunggu hasilnya.

Setelah thread tambahan menyelesaikan tugasnya, hasil penjumlahan disimpan dalam variabel global bernama sum, lalu ditampilkan ke layar oleh thread utama.

Contohnya, jika pengguna menjalankan program dengan argumen 10, maka thread akan menghitung 1 + 2 + ... + 10, dan hasil 55 akan dicetak.

Program ini menunjukkan bagaimana multithreading dapat digunakan untuk memisahkan tugas-tugas tertentu, meskipun dalam contoh ini hanya digunakan satu thread tambahan.

---
#### **C. thrd-win32.c (Windows)**
```c
/**
 * This program creates a separate thread using the CreateThread() system call.
 *
 * Figure 4.13
 *
 * @author Gagne, Galvin, Silberschatz
 * Operating System Concepts  - Tenth Edition
 * Copyright John Wiley & Sons - 2018
 */

#include <stdio.h>
#include <windows.h>

DWORD Sum;

DWORD WINAPI Summation(PVOID Param) {
  DWORD Upper = *(DWORD *)Param;

  for (DWORD i = 0; i <= Upper; i++)
    Sum += i;

  return 0;
}

int main(int argc, char *argv[]) {
  DWORD ThreadId;
  HANDLE ThreadHandle;
  int Param;

  if (argc != 2) {
    fprintf(stderr, "An integer parameter is required\n");
    return -1;
  }

  Param = atoi(argv[1]);

  if (Param < 0) {
    fprintf(stderr, "an integer >= 0 is required \n");
    return -1;
  }

  ThreadHandle = CreateThread(NULL, 0, Summation, &Param, 0, &ThreadId);

  if (ThreadHandle != NULL) {
    WaitForSingleObject(ThreadHandle, INFINITE);
    CloseHandle(ThreadHandle);
    printf("sum = %d\n", Sum);
  }
}

```

#### **Output**

![Screenshot 2025-04-18 180126](https://github.com/user-attachments/assets/b6b1bdfb-18b9-4e00-87b2-2791b1c1b7ca)


#### **Apa yang terjadi ?**
Program ini menggunakan Windows thread untuk menghitung jumlah bilangan dari <code>0</code> hingga <code>N</code>, di mana <code>N</code> adalah angka yang dimasukkan oleh pengguna sebagai argumen. Fungsi <code>Summation</code> dijalankan oleh thread baru dan melakukan penjumlahan, kemudian hasilnya disimpan di variabel global <code>Sum</code>. Thread utama (<code>main</code>) menunggu hingga thread selesai, lalu mencetak hasilnya ke layar.

Contoh: jika dijalankan dengan <code>program.exe 5</code>, maka output-nya adalah <code>sum = 15</code>.

---
## 3. PPT evolusi teknologi processor Intel

[evolusi_prosesor_intel.pptx](https://github.com/user-attachments/files/19784913/evolusi_prosesor_intel.pptx)

---

## 4. Multithreading: Contoh Penerapan, Perhitungan Kecepatan, Jenis Paralelisme, Perbedaan Thread, dan Context Switching Kernel

Soal ini menjelaskan dengan lengkap beberapa topik penting terkait multithreading, yakni:

1. Tiga contoh kode pemrograman yang menunjukkan bagaimana multithreading bisa memberikan performa yang lebih cepat dibanding solusi single-threaded.  
2. Perhitungan peningkatan kecepatan (speedup) menggunakan Hukum Amdahl untuk aplikasi dengan 60% bagian yang bisa diparalelkan, baik untuk dua core maupun empat core.  
3. Penjelasan apakah web server multithreaded dari contoh 4.1 menerapkan task parallelism atau data parallelism.  
4.  Dua perbedaan utama antara user-level thread dan kernel-level thread, serta situasi di mana masing-masing lebih cocok digunakan.  
5. Uraian langkah demi langkah tentang apa saja yang dilakukan kernel saat berpindah dari satu thread ke thread lainnya (context switching).

---
### 1. Contoh Penerapan Multithreading yang Lebih Cepat

Multithreading memungkinkan kita menjalankan banyak tugas secara bersamaan, sehingga program bisa lebih cepat. Berikut tiga contoh aplikasinya:

#### Contoh 1: Web Server Multithreaded

Server web bisa menangani banyak permintaan klien secara bersamaan. Setiap permintaan klien dijalankan dalam thread terpisah, sehingga tidak perlu menunggu satu permintaan selesai untuk memproses yang lain.

```python
import threading
import socket

def handle_client(client_socket):
    request = client_socket.recv(1024)
    client_socket.send(b"Server response")
    client_socket.close()

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('0.0.0.0', 9999))
server.listen(5)

while True:
    client, addr = server.accept()
    client_thread = threading.Thread(target=handle_client, args=(client,))
    client_thread.start()
```

Tanpa thread, server akan memproses satu klien dalam satu waktu, yang menyebabkan server lambat dan tidak responsif.

#### Contoh 2: Pemrosesan Video Secara Real-Time

Dalam aplikasi seperti live streaming atau deteksi wajah, memisahkan proses pengambilan gambar, pemrosesan, dan tampilan ke dalam thread yang berbeda akan menjaga kelancaran performa.

```python
import cv2
import threading

frame = None

def capture_video():
    global frame
    cap = cv2.VideoCapture(0)
    while True:
        ret, frame = cap.read()

def process_video():
    global frame
    while True:
        if frame is not None:
            pass

threading.Thread(target=capture_video).start()
threading.Thread(target=process_video).start()
```

Kalau semua dilakukan dalam satu thread, bisa terjadi delay atau frame drop karena proses yang berat.

#### Contoh 3: Kompresi Banyak File Secara Paralel

Saat mengompres banyak file, kita bisa kompresi masing-masing file di thread terpisah, yang mempercepat proses terutama pada CPU multi-core.

```python
import threading
import zipfile

def compress_file(filename):
    with zipfile.ZipFile(filename + '.zip', 'w') as zipf:
        zipf.write(filename)

files = ['file1.txt', 'file2.txt', 'file3.txt']
threads = []

for file in files:
    t = threading.Thread(target=compress_file, args=(file,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()
```

Kalau dikompres satu per satu, CPU tidak digunakan secara maksimal. Multithreading membuat proses lebih cepat dan efisien.

---

### 2. Perhitungan Speedup dengan Hukum Amdahl

Hukum Amdahl memberikan perkiraan berapa banyak peningkatan kecepatan (speedup) yang bisa dicapai ketika sebagian program dapat diparalelkan. Rumusnya adalah:

$$
\text{Speedup}(N) = \frac{1}{(1 - P) + \frac{P}{N}}
$$


di mana:

$$
P = bagian \ program \ yang \ bisa \ diparalelkan \ (dalam\ desimal)
$$

$$
N = jumlah\ inti\ prosesor\ (core)
$$

$$
(1−P) = bagian\ program\ yang\ tetap\ berjalan\ secara\ serial
$$

Dalam soal ini:

$$
P = 0.60\ (atau\ 60\%)
$$
$$
(1 − P) = 0.40\ (atau\ 40\% )
$$


### Dua Inti Prosesor (N = 2)

$$
\text{Speedup}(2) = \frac{1}{(1 - 0.60) + \frac{0.60}{2}} = \frac{1}{0.40 + 0.30} = \frac{1}{0.70} \approx 1.43
$$

Artinya, kecepatan meningkat sekitar <b>1.43 kali</b> lipat dibanding versi single-core.

### Empat Inti Prosesor (N = 4)

$$
\text{Speedup}(4) = \frac{1}{(1 - 0.60) + \frac{0.60}{4}} = \frac{1}{0.40 + 0.15} = \frac{1}{0.55} \approx 1.82
$$

Artinya, kecepatan meningkat sekitar <b>1.82 kali</b> lipat.


---

### 3. Jenis Paralelisme pada Web Server Multithreaded

Web server multithreaded seperti yang dijelaskan dalam nomor 1 menunjukkan paralelisme tugas (task parallelism), bukan paralelisme data.


**1. Paralelisme Tugas (Task Parallelism)**  
Paralelisme tugas berarti setiap thread melakukan tugas yang berbeda atau tugas yang sama pada data yang berbeda, biasanya secara independen.

Dalam konteks web server multithreaded:

- Setiap thread menangani permintaan klien yang berbeda.

- Masing-masing permintaan bisa melibatkan tugas berbeda seperti:

  - Mengirim halaman web,

  - Menangani upload file,

  - Memproses form.

- Setiap thread bekerja sendiri-sendiri, tidak berbagi data yang sama secara langsung.

**2. Paralelisme Data (Data Parallelism)**  
Paralelisme data terjadi ketika beberapa thread bekerja bersama-sama pada satu kumpulan data, misalnya membagi array besar untuk diproses secara paralel. Ini tidak terjadi dalam web server tersebut.

---

### 4. Perbedaan antara User-Level Thread dan Kernel-Level Thread

Berikut dua perbedaan utama antara user-level thread dan kernel-level thread:

### 1. Pengelolaan dan Penjadwalan

- **User-Level Threads:**  
  - Dikelola sepenuhnya oleh pustaka di ruang pengguna.  
  - Kernel tidak melihat masing-masing thread, sehingga context switching (pergantian antar thread) biasanya lebih cepat dan ringan dari sisi overhead.

- **Kernel-Level Threads:**  
  - Dikelola oleh sistem operasi (kernel).  
  - Kernel bisa menjadwalkan thread pada berbagai core CPU, memberikan keuntungan pada sistem multicore meskipun context switching bisa lebih berat karena melibatkan kernel.

### 2. Perilaku terhadap Operasi Blocking

- **User-Level Threads:**  
  - Jika salah satu thread melakukan operasi blocking (misalnya, menunggu I/O), seluruh proses bisa saja terblokir karena kernel hanya melihat proses secara keseluruhan.  
  - Cocok untuk tugas-tugas yang tidak terlalu banyak operasi blocking.

- **Kernel-Level Threads:**  
  - Jika satu thread blocked, kernel bisa dengan segera menjadwalkan thread lain dari proses yang sama, menjaga agar aplikasi tetap responsif.  
  - Lebih ideal untuk aplikasi yang sering melakukan operasi I/O dan membutuhkan eksekusi paralel di sistem multicore.

**Kesimpulan:**

- **User-Level Threads** cocok untuk aplikasi dengan banyak tugas ringan dan jika operasi blocking dapat dihindari atau diminimalkan.
- **Kernel-Level Threads** lebih unggul untuk aplikasi yang membutuhkan penanganan I/O intensif dan pemanfaatan penuh CPU multicore.

---

### 5. Proses Context Switching Antar Kernel-Level Threads

Berikut adalah langkah-langkah yang dilakukan kernel saat berpindah dari satu thread ke thread lain (context switching):

1. **Menyimpan Konteks Thread yang Sedang Berjalan:**  
   Kernel menyimpan informasi penting seperti register CPU, program counter, dan status thread ke dalam struktur yang disebut Thread Control Block (TCB).

2. **Memperbarui Scheduler:**  
   Kernel mengubah status thread yang sedang berjalan (misalnya menjadi 'ready' atau 'waiting') dan memilih thread berikutnya yang akan dijalankan berdasarkan kebijakan penjadwalan.

3. **Memuat Konteks Thread Berikutnya:**  
   Kernel mengambil informasi yang tersimpan (register, program counter, dan lain-lain) dari TCB thread yang baru dipilih, lalu menempatkannya ke CPU.

4. **Mengganti Konteks Memori (Jika Diperlukan):**  
   Jika thread berikutnya berasal dari proses yang berbeda, kernel akan mengubah ruang alamat yang digunakan, yaitu dengan mengatur ulang unit manajemen memori (MMU). Jika thread itu milik proses yang sama, ruang alamat biasanya tetap sama.

5. **Melanjutkan Eksekusi:**  
   Setelah semua informasi penting telah dipulihkan, CPU akan melanjutkan eksekusi thread yang baru mulai dari titik terakhir ia berhenti.

Setiap kali ada permintaan dari klien, dibuat thread baru untuk mengurusnya. Jadi, satu permintaan tidak menunggu thread lain selesai, semuanya berjalan secara bersamaan.
