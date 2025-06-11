---
title: "06 - Rat in a Maze"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Rat in a Maze]
tags: [Rat in a Maze, Backtracking, C++]
---

# Rat in a Maze
![Desktop View](/assets/RAM.png)
## 🐀 Materi 6 – Rat in a Maze  
**Kelompok 6**
- Rizky Nur Fariid  
- Muh. Anugrah Ashary P.  
- Azizah Nurul Izzah  
- Nur Atika Binti Ardi  
- Shabrina Zahrah R.

---

## 📌 Deskripsi Singkat  
**Rat in a Maze** adalah salah satu problem klasik yang melibatkan pencarian jalur di dalam labirin. Seekor tikus harus bergerak dari titik awal ke tujuan akhir dalam sebuah grid (matriks), hanya melalui jalur yang bisa dilalui (ditandai dengan 1) dan menghindari tembok (ditandai dengan 0). Solusi umum untuk permasalahan ini menggunakan pendekatan **Backtracking**.

---

## 🧠 Konsep Utama

1. Tikus memulai dari titik awal `(0, 0)`.
2. Hanya bisa bergerak ke arah yang valid: kanan, bawah (atau semua arah jika eksplorasi penuh).
3. Langkah valid: tidak keluar dari labirin dan bukan sel tembok.
4. Gunakan backtracking jika menemui jalan buntu.

---

## 🧑‍💻 Apa Itu Backtracking?

Backtracking adalah metode eksplorasi solusi dengan pendekatan coba dan mundur. Cocok untuk problem seperti ini karena:
- Menjelajah semua kemungkinan jalur.
- Mundur jika jalan tidak menuju solusi.

Setiap sel yang dikunjungi akan ditandai. Jika tidak bisa maju, program akan mundur dan mencoba jalur alternatif.

---

## 📥 Input

- Matriks 2D `maze[N][N]`  
  - `1` = jalur bisa dilalui  
  - `0` = dinding  
- Titik awal: `(0, 0)`  
- Titik akhir: `(N-1, N-1)`

---

## 📤 Output

- Matriks solusi: `1` jika jalur dilalui tikus, `0` jika tidak.
- Bisa juga berupa daftar koordinat jalur atau semua kemungkinan solusi (jika diminta).

---

## 🧮 Langkah Penyelesaian

1. Mulai dari `(0, 0)`
2. Cek apakah jalur valid (tidak keluar grid, bukan 0)
3. Tandai posisi sebagai bagian dari jalur
4. Rekursi ke arah bawah atau kanan
5. Jika jalan buntu, hapus tanda (backtrack)
6. Jika mencapai tujuan, cetak solusi

---

## 💻 Contoh Kode Lengkap (C++) – Cocok untuk VS Code

```cpp
#include <iostream>
using namespace std;

const int N = 4;

// Fungsi untuk mengecek apakah sel (x, y) valid untuk dilalui
bool isSafe(int maze[N][N], int x, int y) {
    return (x >= 0 && x < N && y >= 0 && y < N && maze[x][y] == 1);
}

// Fungsi rekursif untuk menyelesaikan maze
bool solveMazeUtil(int maze[N][N], int x, int y, int sol[N][N]) {
    // Jika sudah sampai tujuan
    if (x == N - 1 && y == N - 1 && maze[x][y] == 1) {
        sol[x][y] = 1;
        return true;
    }

    // Cek apakah maze[x][y] bisa dilewati
    if (isSafe(maze, x, y)) {
        // Tandai sebagai bagian dari solusi
        sol[x][y] = 1;

        // Coba ke bawah
        if (solveMazeUtil(maze, x + 1, y, sol))
            return true;

        // Coba ke kanan
        if (solveMazeUtil(maze, x, y + 1, sol))
            return true;

        // Backtrack jika tidak ada solusi
        sol[x][y] = 0;
        return false;
    }

    return false;
}

// Fungsi utama untuk menyelesaikan maze
void solveMaze(int maze[N][N]) {
    int sol[N][N] = { {0} };

    if (!solveMazeUtil(maze, 0, 0, sol)) {
        cout << "Tidak ada solusi!" << endl;
        return;
    }

    // Cetak solusi
    cout << "Solusi Maze:\n";
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++)
            cout << sol[i][j] << " ";
        cout << endl;
    }
}

// Fungsi main
int main() {
    int maze[N][N] = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {0, 1, 0, 0},
        {1, 1, 1, 1}
    };

    solveMaze(maze);
    return 0;
}
