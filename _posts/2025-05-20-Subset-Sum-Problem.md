---
title: "05 - Pengenalan Subset Sum Problem"
date: 2025-05-20 08:00:00 +0800
categories: [DAA, Subset Sum]
tags: [Subset Sum, Backtracking, C++]
---

# Subset Sum Problem
![Desktop View](/assets/SSP.png)

## 🎯 Materi Kelompok 5  
Topik: **Subset Sum Problem**  
Anggota:
- Ishmah Nurwasilah  
- Trivania Buli Karoma  
- Diani Annisah  
- Diesty Mendila Tappo  
- Muhammad Fatir Syabhan

## 🔍 Ringkasan Topik  
**Subset Sum Problem (SSP)** adalah persoalan yang tergolong dalam kategori NP-Complete, di mana kita diminta untuk menentukan apakah mungkin memilih satu subset dari sekumpulan bilangan bulat sehingga jumlah elemen di dalam subset tersebut sama dengan nilai target tertentu. Ini adalah bentuk **decision problem**, karena jawabannya hanya “ya” atau “tidak”.

## 🔄 Varian Masalah  
Beberapa variasi dari Subset Sum Problem meliputi:
- **Unbounded SSP**: elemen bisa dipilih lebih dari sekali  
- **Multi-target SSP**: mencari lebih dari satu target sum  
- **Approximate SSP**: hasil tidak harus persis, cukup mendekati target

## ⚙️ Metode Penyelesaian  

### 1. Rekursif Murni  
Menjelajahi seluruh kemungkinan subset:  
- Setiap elemen: ❌ tidak dipilih atau ✅ dipilih  
- Basis:
  - `sum == 0` → hasilnya `true`  
  - `n == 0` dan `sum != 0` → hasilnya `false`  
- Kompleksitas waktu: O(2ⁿ), sangat tidak efisien untuk input besar

### 2. Dynamic Programming  

#### a. **Memoization (Top-Down)**  
Menggabungkan rekursi dengan penyimpanan hasil submasalah:
- Gunakan array 2D `dp[n][sum]`
- Cegah perhitungan ulang pada kombinasi yang sama
- Waktu & memori: **O(n × sum)**

#### b. **Tabulasi (Bottom-Up)**  
Membangun solusi dari kasus paling dasar:
- `dp[i][j]` = true jika dari `arr[0..i-1]` bisa dapat jumlah `j`
- Inisialisasi:
  - `dp[i][0] = true`  
  - `dp[0][j] = false` untuk `j > 0`

## 📥 Bentuk Input  
Biasanya berupa:  
- Array `A` berisi bilangan bulat positif  
- Target `S`, yaitu jumlah yang ingin dicapai

## 📤 Bentuk Output  
Output bergantung pada tujuan program:
- ✅ Apakah subset dengan jumlah `S` ada?  
- 📦 Apa saja subset-nya?  
- 🔢 Berapa banyak subset yang memenuhi?

## 🔧 Strategi Penyelesaian  
1. Putuskan apakah akan memilih suatu elemen  
2. Periksa batasan saat ini  
3. Rekursi untuk sisa elemen  
4. Lakukan backtracking bila perlu  
5. Tangani kasus dasar dengan tepat

## 🧩 Contoh Masalah  
**Carilah subset dari [3, 4, 5] yang totalnya 9**  

**Langkah-langkahnya**:  
- Pilih 3 → total: 3  
  - Tambah 4 → total: 7  
    - Tambah 5 → total: 12 ❌  
    - Lewati 5 → total: 7 ❌  
  - Lewati 4 → tambah 5 → total: 8 ❌  
- Lewati 3 → pilih 4 dan 5 → total: 9 ✅  
- Lewati semua kecuali 5 → total: 5 ❌  
**Jawaban akhir: [4, 5]**

## 💻 Contoh Program (C++)

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool subsetSumDP(const vector<int>& arr, int sum) {
    int n = arr.size();
    vector<vector<bool>> dp(n+1, vector<bool>(sum+1, false));

    for (int i = 0; i <= n; ++i)
        dp[i][0] = true;
iyyee 
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= sum; ++j) {
            if (arr[i-1] > j)
                dp[i][j] = dp[i-1][j];
            else
                dp[i][j] = dp[i-1][j] || dp[i-1][j - arr[i-1]];
        }
    }
    return dp[n][sum];
}

int main() {
    int n, target;
    cout << "Masukkan jumlah elemen: ";
    cin >> n;

    vector<int> arr(n);
    cout << "Masukkan elemen array:\n";
    for (int i = 0; i < n; ++i)
        cin >> arr[i];

    cout << "Masukkan target: ";
    cin >> target;

    if (subsetSumDP(arr, target))
        cout << "Subset ditemukan dengan total " << target << ".\n";
    else
        cout << "Tidak ditemukan subset yang cocok.\n";

    return 0;
}
