# tugas 1
# Simulasi Round Robin dengan Program C
Kode :
```c
#include <stdio.h>

typedef struct{
    char name;
    int burst_time;
} Process;

void round_robin (Process processes[], int n, int quantum){
    int remaining_time[n];
    for(int i=0; i<n; i++)
        remaining_time[i] = processes[i].burst_time;

    int time = 0;
    int done;

    do{
        done=1;
            for(int i=0; i<n; i++){
                if(remaining_time[i]> 0){
                    done = 0;
                    if(remaining_time[i] > quantum){
                        time += quantum;
                        remaining_time[i] -= quantum;
                        printf("Proses %c berjalan selama %d detik\n", processes[i].name, quantum);
                    } else {
                        time += remaining_time[i];
                        printf("Proses &c selesai dalam %d detik\n", processes[i].name, remaining_time[i]);
                        remaining_time[i] = 0;
                    }

                }

            }
    }while(!done);
}

int main(){
    Process processes[] = {{'A', 10}, {'B', 5}, {'C', 8}};
    int quantum = 4;
    int n = sizeof(processes)/sizeof(processes[0]);

    printf("Simulasi Round Robin Scheduling:\n");
    round_robin(processes, n, quantum);

    return 0;
}
```

Output :
```c
Simulasi Round Robin Scheduling:
Proses A berjalan selama 4 detik
Proses B berjalan selama 4 detik
Proses C berjalan selama 4 detik
Proses A berjalan selama 4 detik
Proses B selesai dalam 1 detik
Proses C selesai dalam 4 detik
Proses A selesai dalam 2 detik
```

Penjelasan :  
Kode ini mensimulasikan algoritma Round Robin, yaitu : CPU dibagi ke tiap proses secara adil (bergiliran) dengan jatah waktu tertentu (quantum). Cara kerja kode :
1. Ada struct Process yang menyimpan :
- name (nama proses)
- burst_time (lama proses butuh CPU)
2. Array :
  ```c
  Process processes[] = {{'A', 10}, {'B', 5}, {'C', 8}};
  ```
  Artinya :
  - A butuh 10 detik
  - B butuh 5 detik
  - C butuh 8 detik
3. Quantum :
```c
int quantum = 4;
```
Setiap proses maksimal jalan 4 detik sekali giliran.  

ALUR EKSEKUSI  
Program pakai
```c
do { ... } while(!done);
```
Loop sampai semua proses selesai.  
Di dalam :
```c
if(remaining_time[i] > quantum)
```
- Kalau waktu masih lebih besar dari quantum, maka proses masih akan dieksekusi selama jatah waktu quantum yaitu 4 detik.
- Kalau sisa waktu sudah lebih kecil dari quantum, maka proses akan selesai dalam ... waktu (<4 detik)

PENJELASAN OUTPUT  
Sistem muter ke semua proses, tidak menyelesaikan 1 proses langsung. Contoh :  
- A: 10 -> 6 -> 2 -> selesai. Maka dari itu outputnya begini  
  Proses A berjalan selama 4 detik  
  Proses A berjalan selama 4 detik  
  Proses A selesai dalam 2 detik    

KELEBIHAN  
1. Adil.  
2. Tidak ada proses starvation.  
3. Cocok untuk sistem interaktif (misal OS).  

KEKURANGAN  
1. Banyak context switching (boros CPU).   
2. Kalau quantum salah (Terlalu kecil, jadi lambat. Terlalu besar, jadi mirip FCFS).


# Simulasi Paging dengan Program C
Kode :
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define FRAMES 3
#define PAGE_COUNT 10

void paging_simulation(int pages[], int page_count) {
    int memory [FRAMES];
    for(int i=0; i<FRAMES; i++) 
        memory[i] = -1;

    srand(time(NULL));

    for(int i=0; i<page_count; i++){
        int found = 0;
        for(int j=0; j<FRAMES; j++){
            if(memory[j] == pages[i]){
                found = 1;
                break;
            }
        }

        if(!found) {
            int idx = rand() % FRAMES;
            memory[idx] = pages[i];
        }

        printf("Memori setelah halaman %d dimuat: [", pages[i]);
        for(int k=0; k<FRAMES; k++) {
            printf("%d ", memory [k]);
        }
        printf("]\n");
    }
}

int main(){
    int pages [PAGE_COUNT] = {1, 2, 3, 4, 5, 2, 1, 3, 7, 5};
    
    printf("Simulasi paging:\n");
    paging_simulation (pages, PAGE_COUNT);
    
    return 0;
}
```

Output :
```c
Simulasi paging:
Simulasi paging:
Memori setelah halaman 1 dimuat: [-1 1 -1 ]
Memori setelah halaman 2 dimuat: [2 1 -1 ]
Memori setelah halaman 3 dimuat: [2 1 3 ]
Memori setelah halaman 4 dimuat: [2 1 4 ]
Memori setelah halaman 5 dimuat: [2 1 5 ]
Memori setelah halaman 2 dimuat: [2 1 5 ]
Memori setelah halaman 1 dimuat: [2 1 5 ]
Memori setelah halaman 3 dimuat: [2 3 5 ]
Memori setelah halaman 7 dimuat: [2 3 7 ]
Memori setelah halaman 5 dimuat: [2 5 7 ]
```

Penjelasan :  
Cara OS mengatur memori dengan membagi ke halaman (pages) & frame. Cara kerja kode :
1. Konstanta :
```c
#define FRAMES 3
#define PAGE_COUNT 10
```
Memori cuma punya 3 slot (frame).  
2. Data halaman :
  ```c
  int pages [PAGE_COUNT] = {1, 2, 3, 4, 5, 2, 1, 3, 7, 5};
  ```
  Ini urutan halaman yang diminta. 

PROSES UTAMA   
1. Cek apakah halaman sudah ada
```c
if(memory[j] == pages[i])
```
Kalau ada, tidak perlu masukin lagi. Kalau tidak ada, masukin ke memori.  
2. Kalau penuh :
```c
int idx = rand() % FRAMES;
```
Pilih frame secara random untuk diganti. Bagian inilah yang bikin random dengan memilih frame secara acak, tidak pakai aturan apapun. Tapi justru biasanya pakai algoritma tertentu biar lebih efisien, di dunia nyata (OS asli) biasanya pakai algoritma :
1. FIFO
```c
[1 2 3] -> masuk 4 -> jadi [4 2 3]
(1 dibuang)
```
2. LRU (Least Recently Used)
   = yang paling lama tidak dipakai akan dibuang.
3. Optimal
   = Buang yang tidak akan dipakai lagi dalam waktu lama (paling efisien tapi susah untuk penerapannya).  

PENJELASAN OUTPUT  
Karena program akan mengganti frame secara acak setiap kali halaman tidak ditemukan (page fault), outputnya begini + penjelasan :
```c
[-1 1 -1 ] //secara acak 1 ditaruh di tengah
[2 1 -1 ] //secara acak 2 menggantikan frame kosong (-1)
[2 1 3 ] //secara acak 3 ditaruh untuk melengkapi frame kosong yang tersisa
[2 1 4 ] //page 4 mau masuk tapi belum ada, maka harus ada yang diganti salah satu. Yang keganti 3 (random)
[2 1 5 ] //begitu seterusnya . . .
[2 1 5 ]
[2 1 5 ]
[2 3 5 ]
[2 3 7 ]
[2 5 7 ]
```

KELEBIHAN  
1. Sederhana karena program mengganti frame secara acak, sehingga mudah diimplementasikan.
2. Menghindari fragmentasi eksternal.
3. Fleksibel dalam manajemen memori.  

KEKURANGAN  
1. Random replacement tidak efisien.
2. Bisa sering page fault.
3. Tidak optimal dibanding FIFO, LRU, optimal.  

KESIMPULAN
1. Round Robin fokus CPU Scheduling,  
   Paging fokus memory management.
2. Round Robin bertujuan membagi waktu CPU,  
   Paging bertujuan mengatur memori.  
3. Round Robin ngatur proses,  
   Paging ngatur halaman(page).  
4. Round Robin pakai sistem time sharing,  
   Paging pakai sistem virtual memory.  
5. Round Robin bekerja dgn cara bergiliran,  
   Paging bekerja dgn cara load & replace page.
