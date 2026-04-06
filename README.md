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
Proses &c selesai dalam 66 detik
Proses &c selesai dalam 67 detik
Proses &c selesai dalam 65 detik
```

Penjelasan :

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
            printf("%d", memory [k]);
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
Memori setelah halaman 1 dimuat: [-11-1]
Memori setelah halaman 2 dimuat: [21-1]
Memori setelah halaman 3 dimuat: [31-1]
Memori setelah halaman 4 dimuat: [34-1]
Memori setelah halaman 5 dimuat: [35-1]
Memori setelah halaman 2 dimuat: [352]
Memori setelah halaman 1 dimuat: [351]
Memori setelah halaman 3 dimuat: [351]
Memori setelah halaman 7 dimuat: [371]
Memori setelah halaman 5 dimuat: [375]
```

Penjelasan :
