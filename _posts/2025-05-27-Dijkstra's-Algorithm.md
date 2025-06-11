---
title: "10 - Dijkstra's Algorithm"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Dijkstra's Algorithm]
tags: [Dijkstra's Algorithm, C++, Shortest Path]
---

# Dijkstra's Algorithm
![Shortest Path](/assets/Dijkstra.png)

## 🌐 Materi 10 – Dijkstra’s Algorithm  
**Kelompok 10**  
- Akmal  
- Muhammad Arlis  
- Muhammad Hairi  
- Zahra Aulia Putri  

---

## 📌 Deskripsi Singkat  

**Dijkstra's Algorithm** adalah algoritma pencarian jalur terpendek dari satu simpul ke simpul lainnya dalam graf berbobot non-negatif. Algoritma ini bekerja dengan pendekatan **greedy** dan memastikan bahwa simpul dengan jarak minimum dari sumber diproses lebih dulu, menggunakan **priority queue (min-heap)** sebagai struktur data pendukung.

---

## 🧠 Konsep Utama

1. Inisialisasi semua jarak ke simpul menjadi ∞ kecuali simpul sumber yang bernilai 0.
2. Gunakan priority queue untuk memilih simpul dengan jarak minimum.
3. Untuk setiap tetangga dari simpul yang sedang diproses:
   - Jika jalur baru lebih pendek, perbarui jaraknya.
   - Simpan parent (simpul sebelumnya) untuk pelacakan jalur.
4. Ulangi sampai semua simpul selesai diproses.

---

## 📥 Input

- Graf berbobot positif, direpresentasikan dengan adjacency list  
- Jumlah simpul (`V`) dan sisi (`E`)  
- Simpul awal (source)

---

## 📤 Output

- Jarak minimum dari simpul awal ke semua simpul lainnya  
- Jalur terpendek (opsional, dengan backtracking dari parent node)

---

## 💻 Contoh Kode Lengkap (C++) – Cocok untuk VS Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <algorithm>
using namespace std;

typedef pair<int, int> pii; // (jarak, simpul)

void dijkstra(int V, vector<vector<pii>>& adj, int source) {
    vector<int> dist(V, INT_MAX);
    vector<int> parent(V, -1);
    priority_queue<pii, vector<pii>, greater<pii>> pq;

    dist[source] = 0;
    pq.push({0, source});

    while (!pq.empty()) {
        int d = pq.top().first;
        int u = pq.top().second;
        pq.pop();

        if (d > dist[u]) continue;

        for (auto edge : adj[u]) {
            int v = edge.first;
            int w = edge.second;

            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                parent[v] = u;
                pq.push({dist[v], v});
            }
        }
    }

    cout << "Jarak minimum dari simpul " << char('A' + source) << ":\n";
    for (int i = 0; i < V; ++i) {
        cout << char('A' + i) << ": " << dist[i] << endl;
    }

    int target = 3; // contoh: simpul D
    cout << "Jalur dari " << char('A' + source) << " ke " << char('A' + target) << ": ";
    vector<int> path;
    for (int at = target; at != -1; at = parent[at])
        path.push_back(at);
    reverse(path.begin(), path.end());
    for (int node : path)
        cout << char('A' + node) << " ";
    cout << endl;
}

int main() {
    int V = 4;
    vector<vector<pii>> adj(V);

    // A=0, B=1, C=2, D=3
    adj[0].push_back({1, 1});
    adj[0].push_back({2, 4});
    adj[1].push_back({2, 2});
    adj[1].push_back({3, 6});
    adj[2].push_back({3, 3});

    dijkstra(V, adj, 0);
    return 0;
}
