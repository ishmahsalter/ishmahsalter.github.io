---
title: "03 - Huffman Coding"
date: 2025-05-20 08:00:00 +0800
categories: [DAA, Huffman Coding]
tags: [Huffman Coding, algoritma greedy, kompresi data, C++]
---

# Huffman Coding
![Desktop View](/assets/HC.png)
## 🧾 Materi 3 
**Huffman Coding**  
**Kelompok 3**
- Dalvyn Suhada
- Sarham San
- Hilmy Affayyad Akbar
- Ivan Ramadhan
- Gyerend Nydle Linta Mangaluk

## 📌 Deskripsi Singkat
**Huffman Coding** adalah algoritma **kompresi data lossless** yang dikembangkan oleh **David A. Huffman** pada tahun 1952. Tujuan utamanya adalah **mengurangi ukuran data** dengan cara mengganti simbol-simbol yang sering muncul dengan **kode biner yang lebih pendek**, dan simbol yang jarang muncul dengan kode lebih panjang. Algoritma ini banyak digunakan dalam format kompresi populer seperti **ZIP, JPEG, MP3**, dan lainnya.

## 🧠 Konsep Utama
- Berdasarkan **frekuensi kemunculan** karakter dalam suatu data.
- Karakter dengan **frekuensi tinggi** akan memiliki **kode lebih pendek**.
- Karakter dengan **frekuensi rendah** memiliki **kode lebih panjang**.
- Representasi data dilakukan melalui **struktur pohon biner** (Huffman Tree).

## 🧑‍💻 Proses Pembuatan Kode Huffman
1. Hitung frekuensi setiap karakter dalam data.
2. Buat simpul (node) untuk setiap karakter dan masukkan ke min-heap berdasarkan frekuensinya.
3. Gabungkan dua node dengan frekuensi terkecil menjadi simpul baru.
4. Ulangi proses hingga terbentuk satu **pohon Huffman**.
5. Tetapkan kode biner untuk tiap karakter: **kiri = 0, kanan = 1**.

## 📥 Input
Terdiri dari:
- Daftar karakter beserta frekuensinya  
  Contoh:  
  Karakter: [a, b, c, d, e, f]  
  Frekuensi: [5, 9, 12, 13, 16, 45]

- Bisa juga berupa string mentah  
  Contoh: `"beep boop beer!"`

## 📤 Output
- Daftar kode Huffman untuk setiap karakter.
- String hasil kompresi (encoded message).
- (Opsional) Pohon Huffman untuk visualisasi.

## 🧮 Langkah Penyelesaian
1. Buat node untuk setiap karakter dan tempatkan dalam **min-heap**.
2. Bangun pohon Huffman dengan menggabungkan node-node kecil.
3. Tetapkan kode biner berdasarkan traversal pohon.
4. Enkode string menggunakan kode Huffman yang telah dibuat.
5. (Opsional) Deskode string kembali menggunakan pohon.

## 🧩 Contoh Masalah dan Solusi
**Input**: String = `"ABBCCCDDDD"`  
**Frekuensi**:
- A: 1
- B: 2
- C: 3
- D: 4

**Hasil kode Huffman (salah satu kemungkinan)**:
- D = 0  
- C = 10  
- B = 110  
- A = 111

**String terenkripsi (contoh)**: `1111101101010100000`  
Ukuran lebih kecil dibandingkan representasi asli.

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
using namespace std;

struct Node {
    char ch;
    int freq;
    Node *left, *right;

    Node(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};

struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq; // min-heap
    }
};

void generateCodes(Node* root, string code, unordered_map<char, string>& huffmanCode) {
    if (!root) return;

    if (!root->left && !root->right) {
        huffmanCode[root->ch] = code;
    }

    generateCodes(root->left, code + "0", huffmanCode);
    generateCodes(root->right, code + "1", huffmanCode);
}

void huffmanCoding(const unordered_map<char, int>& freqMap) {
    priority_queue<Node*, vector<Node*>, Compare> minHeap;

    for (auto pair : freqMap) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    while (minHeap.size() > 1) {
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();

        Node* merged = new Node('\0', left->freq + right->freq);
        merged->left = left;
        merged->right = right;

        minHeap.push(merged);
    }

    Node* root = minHeap.top();

    unordered_map<char, string> huffmanCode;
    generateCodes(root, "", huffmanCode);

    cout << "Kode Huffman untuk tiap karakter:\n";
    for (auto pair : huffmanCode) {
        cout << pair.first << " : " << pair.second << endl;
    }
}

int main() {
    unordered_map<char, int> freqMap = {
        {'a', 5}, {'b', 9}, {'c', 12},
        {'d', 13}, {'e', 16}, {'f', 45}
    };

    huffmanCoding(freqMap);

    return 0;
}
