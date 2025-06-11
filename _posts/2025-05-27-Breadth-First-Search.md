---
title: "07 - Breadth-First Search (BFS)"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, BFS]
tags: [BFS, Maze, C++, Queue]
---

# Breadth-First Search (BFS)
![Maze View](/assets/BFS.png)

## 🌐 Materi 7 – Breadth-First Search (BFS)  
**Kelompok 7**  
- Muh. Hanif Nurmahdin  
- Kevin Anugrah Somakila’  
- Moch. Syech Yusuf. M  
- Raihan Ramadhan  

---

## 📌 Deskripsi Singkat  

**BFS (Breadth-First Search)** adalah algoritma penelusuran graf atau pohon yang bekerja dengan mengeksplorasi simpul-simpul tetangga terlebih dahulu sebelum melangkah lebih jauh. BFS sangat efektif dalam mencari **jalur terpendek** di dalam labirin atau grid.

---

## 🧠 Konsep Utama

1. BFS menggunakan struktur data **antrian (queue)**.
2. Mulai dari titik awal `(0, 0)`, kita kunjungi semua tetangga yang valid (atas, bawah, kiri, kanan).
3. Setiap simpul dicatat apakah sudah pernah dikunjungi (visited).
4. Kita simpan **parent** setiap titik agar bisa merekonstruksi jalur terpendek dari tujuan ke asal.

---

## 📥 Input

- Matriks 2D `maze[N][N]`  
  - `1` = jalur bisa dilalui  
  - `0` = dinding  
- Titik awal: `(0, 0)`  
- Titik akhir: `(N-1, N-1)`

---

## 📤 Output

- Jalur terpendek dari start ke goal (jika ada), ditampilkan dalam urutan koordinat.

---

## 💻 Contoh Kode Lengkap (C++) – Cocok untuk VS Code

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>

using namespace std;

const int N = 4;
int dx[] = {0, 1, 0, -1}; // Kanan, Bawah, Kiri, Atas
int dy[] = {1, 0, -1, 0};

// Cek apakah posisi valid untuk dikunjungi
bool isValid(int x, int y, const vector<vector<int>>& maze, const vector<vector<bool>>& visited) {
    return x >= 0 && y >= 0 && x < N && y < N &&
           maze[x][y] == 1 && !visited[x][y];
}

// Fungsi BFS untuk mencari jalur terpendek
void bfsMaze(const vector<vector<int>>& maze, pair<int, int> start, pair<int, int> goal) {
    queue<pair<int, int>> q;
    vector<vector<bool>> visited(N, vector<bool>(N, false));
    vector<vector<pair<int, int>>> parent(N, vector<pair<int, int>>(N, {-1, -1}));

    q.push(start);
    visited[start.first][start.second] = true;

    while (!q.empty()) {
        auto [x, y] = q.front(); q.pop();

        if (make_pair(x, y) == goal) break;

        for (int i = 0; i < 4; ++i) {
            int nx = x + dx[i];
            int ny = y + dy[i];

            if (isValid(nx, ny, maze, visited)) {
                visited[nx][ny] = true;
                parent[nx][ny] = {x, y};
                q.push({nx, ny});
            }
        }
    }

    // Rekonstruksi jalur dari goal ke start
    vector<pair<int, int>> path;
    pair<int, int> current = goal;

    while (current != make_pair(-1, -1)) {
        path.push_back(current);
        current = parent[current.first][current.second];
    }

    // Jika tidak ada jalur
    if (path.back() != start) {
        cout << "Tidak ada jalur dari start ke goal.\n";
        return;
    }

    reverse(path.begin(), path.end());
    cout << "Jalur terpendek (BFS):\n";
    for (auto [x, y] : path) {
        cout << "(" << x << "," << y << ") ";
    }
    cout << endl;
}

int main() {
    vector<vector<int>> maze = {
        {1, 1, 0, 1},
        {0, 1, 0, 1},
        {1, 1, 1, 1},
        {0, 0, 1, 1}
    };

    pair<int, int> start = {0, 0};
    pair<int, int> goal = {3, 3};

    bfsMaze(maze, start, goal);

    return 0;
}
