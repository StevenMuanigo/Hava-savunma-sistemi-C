# Hava-savunma-sistemi-C

ÖNEMLİ NOT:Bu proje, tamamen bireysel olarak geliştirilmiştir Kodlama sürecinde herhangi bir yapay zeka aracı (ChatGPT, Copilot, vb.) kullanılmadığını gösterebilmek için kodlanmıştır  eğitim, demo ve yazılım mühendisliği pratiği için paylaşılmıştır herhangi bi sorumluluk kabul edilmiyor

C ile Hava Savunma Simülasyonu – Teknik Özeti

 Yapı Özellikleri
enum TargetType → Tehdit türleri: DRONE, JET, MISSILE

struct ThreatObject → Her hedefin azimut, yükseklik, menzil bilgisi

calculateCoordinates() → Rastgele koordinat değişimi simülasyonu

trackTarget() → Her hedefi ayrı thread ile izleme

pthread_create() → Gerçek zamanlı çoklu hedef takibi

sleep(1) → Radar ekranı gibi saniyelik güncelleme

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
#include <math.h>

#define NUM_TARGETS 3

// Enumeration for different types of targets
typedef enum { 
    DRONE, 
    JET, 
    MISSILE 
} TargetType;

// Structure to represent a target
typedef struct {
    TargetType type;
    double azimuth;
    double elevation;
    double range;
} ThreatObject;

// Function to calculate the coordinates of the target
void calculateCoordinates(ThreatObject *target) {
    // Simulate random coordinate changes
    target->azimuth += ((rand() % 10) - 5) * 0.1;
    target->elevation += ((rand() % 10) - 5) * 0.1;
    target->range += ((rand() % 10) - 5) * 0.1;

    if (target->azimuth < 0) target->azimuth = 0;
    if (target->elevation < 0) target->elevation = 0;
    if (target->range < 0) target->range = 0;
}

// Function to simulate tracking of a target
void* trackTarget(void *arg) {
    ThreatObject *target = (ThreatObject *)arg;
    while (1) {
        calculateCoordinates(target);

        // Simulate radar screen output
        printf("Tracking Target: %s | Azimuth: %0.2f | Elevation: %0.2f | Range: %0.2f\n",
               (target->type == DRONE ? "Drone" : target->type == JET ? "Jet" : "Missile"),
               target->azimuth, target->elevation, target->range);

        sleep(1);
    }
    return NULL;
}

int main() {
    pthread_t threads[NUM_TARGETS];
    ThreatObject targets[NUM_TARGETS] = {
        {DRONE, 45.0, 5.0, 100.0},
        {JET, 90.0, 10.0, 200.0},
        {MISSILE, 120.0, 15.0, 300.0}
    };
    
    srand(time(NULL)); // Initialize random generator

    // Create a thread for each target
    for (int i = 0; i < NUM_TARGETS; ++i) {
        pthread_create(&threads[i], NULL, trackTarget, (void *)&targets[i]);
    }

    // Join threads
    for (int i = 0; i < NUM_TARGETS; ++i) {
        pthread_join(threads[i], NULL);
    }

    return 0;
}
```
