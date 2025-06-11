---
title: "02 - Fractional Knapsack: Solusi Serakah yang Optimal"
date: 2025-05-06 08:00:00 +0800
categories: [DAA, Fractional Knapsack]
tags: [Knapsack, Greedy, C++]
---

# 🧭 Fractional Knapsack
![Desktop View](/assets/FK.png)
## 📘 Ringkasan Materi  
**Disusun oleh Kelompok 2:**
- Zalfa Syauqiyah Hamka  
- Davidzen  
- Maisyah Mahdiyyah  
- Muhammad Fadel Aryasatya Makkulau  
- Hezekiah Reynard Tikupadang  

## 🎯 Tujuan Utama  
Fractional Knapsack adalah persoalan optimasi di mana kita ingin **mengisi tas dengan barang-barang yang memiliki nilai maksimal** tanpa melebihi kapasitas tas tersebut. Berbeda dengan versi 0/1, versi fraksional ini **memungkinkan kita mengambil sebagian dari barang**.

## 🔍 Inti Permasalahan  
Diberikan:
- Kapasitas maksimum `W`
- Sekumpulan barang, masing-masing dengan `value` dan `weight`

Tugas:
- Pilih kombinasi barang (atau sebagian dari mereka) untuk **memaksimalkan total value** tanpa melebihi `W`.

## ⚙️ Konsep Greedy di Knapsack  
Greedy berarti **ambil keputusan terbaik secara lokal pada setiap langkah**, tanpa melihat keseluruhan akibat di masa depan. Dalam Fractional Knapsack, strategi greedy terbukti **selalu memberikan hasil optimal**.

### Strategi:
- Hitung rasio `value/weight` untuk setiap item  
- Ambil barang dengan rasio tertinggi terlebih dahulu  
- Jika barang tak muat sepenuhnya, ambil sebagian  

## 📥 Input  
Input terdiri dari:
- `n`: jumlah item  
- `W`: kapasitas maksimum  
- Daftar item berisi:
  - `value[i]`: nilai barang ke-i  
  - `weight[i]`: berat barang ke-i  

## 📤 Output  
- Total nilai maksimal yang bisa dibawa  
- Detail barang yang dipilih beserta fraksinya jika sebagian

## 🧮 Contoh Soal dan Penyelesaian  
**Data Barang:**

| Barang | Nilai | Berat |
| ------ | ----- | ----- |
| A      | 50    | 10    |
| B      | 60    | 20    |
| C      | 140   | 40    |
| D      | 60    | 30    |

**Kapasitas tas:** 50

### Langkah-langkah:
1. Hitung rasio:
   - A: 5.0
   - B: 3.0
   - C: 3.5
   - D: 2.0  
2. Urutkan: A (5.0), C (3.5), B (3.0), D (2.0)  
3. Ambil:
   - A sepenuhnya (10kg)
   - C sepenuhnya (40kg)
   - Total berat: 50kg → tas penuh

✅ **Total nilai: 50 + 140 = 190**

## 💻 Implementasi C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Item {
    string name;
    double value, weight;
    double ratio() const { return value / weight; }
};

bool cmp(Item a, Item b) {
    return a.ratio() > b.ratio();
}

int main() {
    double capacity = 50.0;
    vector<Item> items = {
        {"A", 50, 10}, {"B", 60, 20},
        {"C", 140, 40}, {"D", 60, 30}
    };

    sort(items.begin(), items.end(), cmp);

    double totalValue = 0.0;
    cout << "Barang yang dimasukkan ke tas:\n";

    for (const auto& item : items) {
        if (capacity == 0) break;

        if (item.weight <= capacity) {
            cout << "- " << item.name << " (penuh, " << item.weight << "kg)\n";
            totalValue += item.value;
            capacity -= item.weight;
        } else {
            double fraction = capacity / item.weight;
            cout << "- " << item.name << " (" << fraction * 100 << "%, " << capacity << "kg)\n";
            totalValue += item.value * fraction;
            capacity = 0;
        }
    }

    cout << "Total nilai maksimal: " << totalValue << endl;
    return 0;
}
