<code>Nama : Fahroldhi Sukirno</code></br>
<code>NRP : 3124500046</code></br>
<code>Kelas : D3 Teknik Informatika</code></br>
<code>Dosen Pengajar: Dr Ferry Astika Saputra ST, M.Sc</code></br>  

---

### 1. Program Thread Sederhana (Compile Thread 3)

#### **Kode Program**
```c
#include <pthread.h>
#include <stdio.h>

pthread_cond_t cond1 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond2 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond3 = PTHREAD_COND_INITIALIZER;

pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

int done = 1;

void *foo(void *n) {
  while (1) {

    pthread_mutex_lock(&lock);

    if (done != (int)*(int *)n) {

      if ((int)*(int *)n == 1) {
        pthread_cond_wait(&cond1, &lock);
      } else if ((int)*(int *)n == 2) {
        pthread_cond_wait(&cond2, &lock);
      } else {
        pthread_cond_wait(&cond3, &lock);
      }
    }
    printf("%d ", *(int *)n);

    if (done == 3) {
      done = 1;
      pthread_cond_signal(&cond1);
    } else if (done == 1) {
      done = 2;
      pthread_cond_signal(&cond2);
    } else if (done == 2) {
      done = 3;
      pthread_cond_signal(&cond3);
    }

    pthread_mutex_unlock(&lock);
  }

  return NULL;
}

int main() {
  pthread_t tid1, tid2, tid3;
  int n1 = 1, n2 = 2, n3 = 3;

  pthread_create(&tid1, NULL, foo, (void *)&n1);
  pthread_create(&tid2, NULL, foo, (void *)&n2);
  pthread_create(&tid3, NULL, foo, (void *)&n3);

  while (1);

  return 0;
}
```

#### **Output**
![image](https://github.com/user-attachments/assets/20b4d5e4-37a5-4861-b569-44481e572fe4)

(Program berjalan terus hingga ditekan `CTRL+C`.)

#### **Apa yang terjadi ?**
Program ini menggunakan 3 thread untuk mencetak angka 1, 2, dan 3 secara bergiliran terus-menerus. Sinkronisasi antar thread dilakukan dengan:

- `pthread_mutex_t`: Mengatur akses bersama ke variabel done

- `pthread_cond_t`: Masing-masing thread memiliki condition variable untuk menunggu gilirannya

Thread hanya mencetak angka jika nilai done sesuai dengan ID-nya. Setelah mencetak, thread akan memberi sinyal ke thread berikutnya untuk melanjutkan.

Tujuan: Menjaga urutan output agar selalu:
1 2 3 1 2 3 ...

---

### 2. Program Forking

#### **Kode Program**
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork gagal");
        return 1;
    } else if (pid == 0) {
        printf("Proses anak (PID: %d)\n", getpid());
    } else {
        printf("Proses induk (PID: %d) dengan anak PID: %d\n", getpid(), pid);
        wait(NULL);
        printf("Proses anak telah selesai\n");
    }
    return 0;
}

```

#### **Output**
![image](https://github.com/user-attachments/assets/0b4f2ca2-346b-49a3-917b-0d363cacc107)

(Urutan output bisa berbeda tergantung eksekusi proses.)

#### **Apa yang terjadi ?**
Program ini menunjukkan bagaimana cara membuat proses baru menggunakan fungsi <code>fork()</code> dalam bahasa C, serta bagaimana proses induk dapat menunggu proses anak selesai menggunakan fungsi <code>wait()</code>.

Saat program dijalankan, proses utama (induk) akan memanggil <code>fork()</code>. Fungsi ini akan membuat salinan dari proses tersebut, sehingga menghasilkan dua proses yang berjalan secara paralel: proses induk dan proses anak.

Jika <code>fork()</code> berhasil, maka:

- Proses anak akan mencetak pesan yang menunjukkan bahwa ia adalah proses anak, serta mencetak ID prosesnya sendiri (<code>getpid()</code>).

- Proses induk akan mencetak pesan yang menunjukkan bahwa ia adalah proses induk, mencetak ID-nya, dan juga mencetak ID dari proses anak (yang dikembalikan oleh <code>fork()</code>).

- Setelah itu, proses induk akan memanggil <code>wait(NULL)</code>, yang berfungsi untuk menunggu proses anak selesai sebelum melanjutkan. Setelah proses anak selesai, proses induk akan mencetak pesan tambahan bahwa proses anak telah selesai. </br></br>

---


### Soal Nomor 3  
**Pertanyaan:**  
*Apakah keuntungan menggunakan time quantum size di level yang berbeda dari sebuah antrian sistem multilevel?*

#### **Jawaban**
1. **Menyesuaikan kebutuhan proses berdasarkan prioritas**  
   - **Proses prioritas tinggi (misal interaktif)** butuh respons cepat, jadi diberi time quantum kecil agar sering dijadwalkan → responsif.
   - **Proses prioritas rendah (misal background task)** bisa diberi time quantum besar → mengurangi overhead karena lebih jarang preempted.

2. **Meningkatkan efisiensi CPU**  
   - Proses yang cepat selesai (short jobs) dapat diselesaikan lebih cepat di level atas.
   - Proses panjang turun ke level bawah, sehingga tidak mengganggu proses cepat.

3. **Mengurangi context switch yang tidak perlu**  
   - Di level bawah, time quantum yang besar → proses berjalan lebih lama → lebih sedikit context switching, artinya overhead sistem lebih kecil.  

4. **Mendukung strategi aging dan keadilan**  
   - Proses lambat yang sering turun level tetap bisa dapat waktu CPU, dan seiring waktu bisa dipromosikan kembali (dalam MLFQ).  
   - Sistem lebih fair untuk berbagai jenis proses: CPU-bound, I/O-bound, interaktif, dsb.

---

### Soal Nomor 4  
*Gambarkan 4 diagram Chart yang mengilustrasikan eksekusi dari proses-proses tersebut
menggunakan FCFS, SJF, prioritas nonpreemptive dan round robin.*    
**Tabel Proses:**  
| Proses | Burst Time | Prioritas |  
|--------|------------|-----------|  
| P1     | 10         | 3         |  
| P2     | 1          | 1         |  
| P3     | 2          | 3         |  
| P4     | 1          | 4         |  
| P5     | 5          | 2         |  

#### **Gantt Chart**  
**1. FCFS (First Come First Serve)**  
Urutan: P1 → P2 → P3 → P4 → P5  
![1](https://github.com/user-attachments/assets/957c43c9-eb14-475e-a5ef-ceeebbbfe849)  

Penjelasan Rinci:

1. P1 dimulai pada waktu 0 dan berjalan selama 10 unit waktu (0-10).

2. P2 dimulai setelah P1 selesai pada waktu 10 dan berjalan selama 1 unit waktu (10-11).

3. P3 dimulai pada waktu 11 dan berjalan selama 2 unit waktu (11-13).

4. P4 dimulai pada waktu 13 dan berjalan selama 1 unit waktu (13-14).

5. P5 dimulai pada waktu 14 dan berjalan selama 5 unit waktu (14-19).

**2. SJF Nonpreemptive**  
Urutan: P2 (1) → P4 (1) → P3 (2) → P5 (5) → P1 (10)  
![2](https://github.com/user-attachments/assets/59acc0fc-d9df-41e6-b6ad-53ee50b0f314)

Penjelasan Rinci:

1. P2 (waktu eksekusi 1) dimulai pada waktu 0 dan selesai pada waktu 1.

2. P4 (waktu eksekusi 1) dimulai pada waktu 1 dan selesai pada waktu 2.

3. P3 (waktu eksekusi 2) dimulai pada waktu 2 dan selesai pada waktu 4.

4. P5 (waktu eksekusi 5) dimulai pada waktu 4 dan selesai pada waktu 9.

5. P1 (waktu eksekusi 10) dimulai pada waktu 9 dan selesai pada waktu 19.

**3. Prioritas Nonpreemptive**  
Urutan: P2 (1) → P5 (2) → P1 (3) → P3 (3) → P4 (4)  
![3](https://github.com/user-attachments/assets/06d50e98-59ee-48d8-8c15-debb25b3a2a8)

Penjelasan Rinci:

1. P2 (prioritas tertinggi) dimulai pada waktu 0 dan selesai pada waktu 1.

2. P5 (prioritas tinggi) dimulai pada waktu 1 dan selesai pada waktu 3.

3. P1 (prioritas sedang) dimulai pada waktu 6 dan selesai pada waktu 9.

4. P3 (prioritas lebih rendah) dimulai pada waktu 9 dan selesai pada waktu 12.

5. P4 (prioritas terendah) dimulai pada waktu 12 dan selesai pada waktu 19.

**4. Round Robin (Quantum = 2)**  
Urutan :  P1 → P2 → P3 → P4 → P5 → P1 → P5 → P1 → P5 → P1 → P1
![4](https://github.com/user-attachments/assets/05d1b633-80e8-414b-970a-3d7e36422328)
  
Penjelasan Rinci:

1. P1 dimulai pada waktu 0, berjalan selama 2 unit waktu (0-2), kemudian P2 dijalankan.

2. P2 berjalan selama 2 unit waktu (2-4), kemudian proses berlanjut ke P3.

3. P3 berjalan selama 2 unit waktu (4-6), dan seterusnya.

4. Setelah setiap time quantum habis, proses dipindahkan ke antrian dan giliran eksekusi berlanjut ke proses berikutnya.

5.  Setelah semua proses berjalan, P1 dan P5 kembali ke antrian dan terus berjalan sesuai time quantum hingga selesai.
---

**Penjelasan Singkat Algoritma:**  
- **FCFS**: Proses dieksekusi sesuai urutan kedatangan.  
- **SJF**: Proses dengan burst time terkecil didahulukan.  
- **Prioritas**: Proses dengan prioritas terkecil (tertinggi) dieksekusi pertama.  
- **Round Robin**: Setiap proses mendapat jatah waktu 2 unit, lalu di-switch ke proses berikutnya.

