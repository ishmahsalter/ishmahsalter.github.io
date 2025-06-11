---
title: "01 - Memahami Activity Selection Problem"
date: 2025-05-06 08:00:00 +0800
categories: [DAA, Greedy Algorithm]
tags: [Greedy, Penjadwalan, C++, Optimization]
---

# ⏰ Activity Selection Problem

![Desktop View](/assets/ASP.png)

## 👥 Kelompok 1
- Ahmad Hidayat  
- Julio Rema Palotongan  
- Suci Sri Aulia  
- Naailah Mazaya  
- Yud Bryawan  

---

## 📖 Apa Itu Activity Selection Problem?

Masalah ini muncul saat kita ingin **memilih aktivitas sebanyak mungkin tanpa saling bertabrakan waktu**. Setiap aktivitas punya waktu mulai (**start**) dan waktu selesai (**finish**). Aktivitas dianggap **kompatibel** jika satu aktivitas selesai sebelum yang lain mulai.

Sederhananya: kita ingin **menjadwalkan kegiatan sebanyak mungkin tanpa tumpang tindih**.

---

## 💡 Inti Algoritma

- Termasuk algoritma **Greedy** (alias serakah 😄)
- Fokus pada aktivitas yang **selesai paling awal**
- Sangat cocok untuk kasus penjadwalan, seperti ruangan, kelas, atau CPU

---

## 🔍 Kenapa Pakai Greedy?

Algoritma **Greedy** memilih solusi "terbaik saat itu juga", tanpa melihat konsekuensi jangka panjang.  
Untuk masalah ini, strategi terbaik adalah:
> **Pilih aktivitas yang selesai paling awal**, lalu lanjutkan ke aktivitas berikutnya yang tidak bentrok.

---

## 🧾 Format Masukan & Keluaran

### Input:
- Dua array atau list:  
  `start[]` → waktu mulai  
  `finish[]` → waktu selesai  

### Output:
- Indeks atau daftar aktivitas yang bisa dijalankan tanpa konflik waktu

---

## 🔧 Langkah-Langkah Penyelesaian

1. **Urutkan** semua aktivitas berdasarkan waktu selesai (paling cepat selesai = prioritas tinggi)
2. **Ambil aktivitas pertama** (karena dia selesai paling awal)
3. Untuk aktivitas berikutnya, hanya pilih jika waktu mulai ≥ waktu selesai aktivitas terakhir yang dipilih

---

## 🧪 Contoh Kasus

| Aktivitas | Mulai | Selesai |
| --------- | ----- | ------- |
| A1        | 1     | 4       |
| A2        | 3     | 5       |
| A3        | 0     | 6       |
| A4        | 5     | 7       |
| A5        | 8     | 9       |
| A6        | 5     | 9       |

### Solusi:
1. **Urut berdasarkan selesai** → A1, A2, A3, A4, A5, A6  
2. Pilih A1 → selesai jam 4  
3. A2 dan A3 bentrok → lewati  
4. A4 mulai jam 5 → aman  
5. A5 mulai jam 8 → aman  
6. A6 bentrok

✅ Jawaban optimal: **{A1, A4, A5}**

---

## 🧑‍💻 Kode Implementasi (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int start, finish, index;
};

bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

void activitySelection(vector<Activity>& activities) {
    sort(activities.begin(), activities.end(), compare);

    cout << "Aktivitas terpilih (index): ";
    int lastFinish = -1;

    for (auto act : activities) {
        if (act.start >= lastFinish) {
            cout << act.index << " ";
            lastFinish = act.finish;
        }
    }
    cout << endl;
}

int main() {
    vector<Activity> activities = {
        {1, 2, 0}, {3, 4, 1}, {0, 6, 2},
        {5, 7, 3}, {8, 9, 4}, {5, 9, 5}
    };

    activitySelection(activities);
    return 0;
}
