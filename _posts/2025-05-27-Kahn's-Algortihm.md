---
title: "09 - Kahn's Algorithm"
date: 2025-05-27 08:00:00 +0800
categories: [DAA, Khan's Algorithm]
tags: [Khan's Algorithm, C++, Topological Sort, DAG]
---

# Kahn's Algorithm
![Topological Sort](/assets/Kahn.png)

## 🌐 Materi 9 – Kahn’s Algorithm  
**Kelompok 9**  
- Nurul Marisa Clara Waldi  
- Agung Allo Karaeng  
- Ahmad Rizky Amis  
- Mahesa Putri Lukman  

---

## 📌 Deskripsi Singkat  

**Kahn’s Algorithm** digunakan untuk menyusun urutan simpul dalam **Directed Acyclic Graph (DAG)** berdasarkan ketergantungan antar node. Ini adalah algoritma **topological sort** berbasis **Breadth-First Search (BFS)**. Algoritma ini memastikan bahwa jika ada sisi `u → v`, maka `u` muncul sebelum `v` dalam urutan akhir.

---

## 🧠 Konsep Utama

1. Hitung **in-degree** (jumlah sisi masuk) untuk setiap simpul.
2. Masukkan semua simpul dengan in-degree 0 ke dalam **queue**.
3. Proses satu per satu:
   - Ambil dari queue dan tambahkan ke hasil urutan.
   - Kurangi in-degree semua tetangga.
   - Jika in-degree tetangga menjadi 0, masukkan ke queue.
4. Jika semua simpul diproses → tidak ada siklus.  
   Jika tidak → terdapat **siklus**.

---

## 📥 Input

- Jumlah simpul (`V`) dan sisi (`E`)
- Setiap sisi `A B` menunjukkan bahwa A → B (A sebelum B)

---

## 📤 Output

- Urutan topologis simpul jika tidak ada siklus  
- Pesan error jika terdapat siklus

---

## 💻 Contoh Kode Lengkap (C++) – Cocok untuk VS Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int V, E;
    cout << "Masukkan jumlah simpul dan sisi: ";
    cin >> V >> E;

    vector<vector<int>> adj(V);
    vector<int> in_degree(V, 0);

    cout << "Masukkan sisi dalam format 'u v' artinya u → v:\n";
    for (int i = 0; i < E; ++i) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        in_degree[v]++;
    }

    queue<int> q;
    for (int i = 0; i < V; ++i) {
        if (in_degree[i] == 0)
            q.push(i);
    }

    vector<int> topo_order;

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        topo_order.push_back(u);

        for (int v : adj[u]) {
            in_degree[v]--;
            if (in_degree[v] == 0)
                q.push(v);
        }
    }

    if (topo_order.size() == V) {
        cout << "Urutan topologis:\n";
        for (int v : topo_order)
            cout << v << " ";
        cout << endl;
    } else {
        cout << "Graf mengandung siklus, tidak bisa dilakukan topological sort.\n";
    }

    return 0;
}
