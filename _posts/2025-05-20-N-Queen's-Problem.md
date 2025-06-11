---
title: "N-Queens Problem — Backtracking dan Implementasi dalam C++"
date: 2025-05-20 08:00:00 +0800
categories: [DAA, Algoritma]
tags: [N-Queens, Backtracking, C++, CSP]
---

# 👑 N-Queens Problem
![Desktop View](/assets/NQP.png)
_Disusun oleh Kelompok 4:_
- Aditya Hisbul Bahri  
- Jonas Baka  
- Akhmad Hidayat  
- As'syiekril Fikryan Sarda  
- M. Fadhil Mulyadi  

---

## 📌 Apa Itu N-Queens Problem?

**N-Queens Problem** adalah tantangan untuk menempatkan `N` buah ratu catur pada papan catur berukuran `N × N`, sedemikian rupa sehingga **tidak ada dua ratu** yang dapat saling menyerang.

Ratu dalam catur dapat bergerak:
- Secara horizontal (baris),
- Vertikal (kolom),
- Diagonal (kedua arah).

Tujuan utamanya adalah mencari semua konfigurasi valid penempatan ratu, seperti pada versi klasik **8-Queens** di papan 8×8.

---

## 🧠 Konsep Kunci

- **Constraint Satisfaction Problem (CSP)**: Solusi harus memenuhi syarat tidak ada dua ratu yang menyerang.
- **Eksplorasi Solusi Sistematis**: Menggunakan algoritma _backtracking_ untuk menjelajahi semua kemungkinan.
- **Pohon Pencarian**: Setiap level dalam pohon menyimbolkan peletakan ratu per baris.
- **Pemangkasan (Pruning)**: Solusi tidak valid langsung dihentikan sejak dini.

---

## 🔁 Backtracking: Strategi Efisien

Backtracking adalah metode rekursif di mana kita mencoba membangun solusi langkah demi langkah dan **mundur (backtrack)** ketika menemukan jalan buntu.

### Alur Backtracking dalam N-Queens:

1. **Letakkan ratu per baris**, mulai dari baris ke-0.
2. Untuk setiap kolom di baris tersebut:
   - Periksa apakah posisi tersebut **aman**.
   - Jika ya, lanjutkan ke baris berikutnya.
   - Jika tidak ada posisi aman, **kembali ke baris sebelumnya** dan coba posisi lain.

---

## 📥 Input dan 📤 Output

### Input:
Sebuah bilangan bulat `N` sebagai ukuran papan dan jumlah ratu.

### Output:
- Jumlah total solusi valid
- Representasi visual tiap solusi berupa string 2D

---

## 💻 Implementasi N-Queens dalam C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

class NQueens {
private:
    int N;
    vector<vector<string>> solutions;

    bool isSafe(int row, int col, vector<string>& board) {
        for (int i = 0; i < row; i++)
            if (board[i][col] == 'Q') return false;

        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 'Q') return false;

        for (int i = row - 1, j = col + 1; i >= 0 && j < N; i--, j++)
            if (board[i][j] == 'Q') return false;

        return true;
    }

    void solve(int row, vector<string>& board) {
        if (row == N) {
            solutions.push_back(board);
            return;
        }

        for (int col = 0; col < N; col++) {
            if (isSafe(row, col, board)) {
                board[row][col] = 'Q';
                solve(row + 1, board);
                board[row][col] = '.'; // Backtrack
            }
        }
    }

public:
    void solveNQueens(int n) {
        N = n;
        solutions.clear();
        vector<string> board(N, string(N, '.'));

        solve(0, board);

        cout << "Jumlah solusi: " << solutions.size() << endl;
        for (const auto& sol : solutions) {
            for (const string& row : sol)
                cout << row << endl;
            cout << endl;
        }
    }
};

int main() {
    int n;
    cout << "Masukkan nilai N: ";
    cin >> n;

    NQueens solver;
    solver.solveNQueens(n);

    return 0;
}
