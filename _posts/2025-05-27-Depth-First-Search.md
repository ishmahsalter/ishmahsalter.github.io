---
title: "08 - Depth-First Search (DFS)"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, DFS]
tags: [DFS, Maze, C++, Stack, Backtracking]
---

# Depth-First Search (DFS)
![Maze View](/assets/DFS.png)

## 🌐 Materi 8 – Depth-First Search (DFS)  
**Kelompok 8**  
- Bismillah Ghaniyyu Putra Darmin  
- Maynova Christin Gabryela Simamora  
- Nabila Salsabila  
- Moh. Ichwanul Muslimin Mayang  

---

## 📌 Deskripsi Singkat  

**DFS (Depth-First Search)** adalah algoritma penelusuran graf atau grid yang mengeksplorasi jalur sejauh mungkin sebelum mundur (backtrack). Cocok digunakan untuk masalah pencarian jalur, seperti menyusuri labirin, dan juga menyelesaikan teka-teki seperti Sudoku, N-Queens, dan Rat in a Maze.

---

## 🧠 Konsep Utama

1. DFS dapat menggunakan **rekursi** atau **stack** untuk menyimpan node yang dikunjungi.
2. Algoritma ini akan masuk lebih dalam hingga tidak bisa lanjut, lalu **backtrack**.
3. Cocok untuk **menjelajahi semua jalur** atau mencari solusi dengan kedalaman maksimal.
4. Pada grid atau maze, DFS bisa digunakan untuk mencari apakah ada jalur dari start ke goal.

---

## 📥 Input

- Matriks 2D `maze[N][N]`  
  - `1` = jalur bisa dilalui  
  - `0` = dinding  
- Titik awal: `(0, 0)`  
- Titik akhir: `(N-1, N-1)`

---

## 📤 Output

- Jalur dari start ke goal (jika ada), ditampilkan dalam urutan koordinat.

---

## 💻 Contoh Kode Lengkap (C++) – Cocok untuk VS Code

```cpp
#include <iostream>
#include <vector>
#include <cstring>

using namespace std;

const int N = 4;
int maze[N][N] = {
    {1, 1, 0, 1},
    {0, 1, 0, 1},
    {1, 1, 1, 1},
    {0, 0, 1, 1}
};

bool visited[N][N];
vector<pair<int, int>> path;

// Arah gerak: kanan, bawah, kiri, atas
int dx[] = {0, 1, 0, -1};
int dy[] = {1, 0, -1, 0};

// Cek apakah sel valid dan bisa dikunjungi
bool isSafe(int x, int y) {
    return (x >= 0 && x < N && y >= 0 && y < N &&
            maze[x][y] == 1 && !visited[x][y]);
}

// DFS rekursif
bool dfs(int x, int y) {
    visited[x][y] = true;
    path.push_back({x, y});

    if (x == N - 1 && y == N - 1)
        return true;

    for (int i = 0; i < 4; ++i) {
        int nx = x + dx[i];
        int ny = y + dy[i];

        if (isSafe(nx, ny)) {
            if (dfs(nx, ny))
                return true;
        }
    }

    path.pop_back(); // Backtrack
    return false;
}

int main() {
    memset(visited, false, sizeof(visited));

    if (dfs(0, 0)) {
        cout << "Jalur ditemukan (DFS):\n";
        for (auto [x, y] : path)
            cout << "(" << x << "," << y << ") ";
        cout << endl;
    } else {
        cout << "Tidak ada jalur dari start ke goal.\n";
    }

    return 0;
}
