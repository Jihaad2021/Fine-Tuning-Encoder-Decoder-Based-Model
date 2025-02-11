# **Evaluasi Model Transformer untuk Klasifikasi Berita Indonesia**  

## 📌 1. Gambaran Proyek  
Penelitian ini bertujuan untuk mengevaluasi performa model berbasis **Transformer** dalam tugas **klasifikasi teks berita berbahasa Indonesia**. Secara umum, model **encoder-only** seperti BERT, RoBERTa, dan DistilBERT seringkali menghasilkan performa terbaik dalam tugas klasifikasi teks. Namun, tugas ini juga dapat diselesaikan menggunakan model **decoder-only** seperti GPT-2, maupun model **encoder-decoder** seperti T5.

Selain itu, dalam pemrosesan bahasa alami (NLP), performa klasifikasi teks biasanya dapat ditingkatkan dengan melakukan **tuning dari model multibahasa ke bahasa spesifik**, seperti bahasa Indonesia. Oleh karena itu, penelitian ini juga mengevaluasi pengaruh **pre-trained model berbahasa Inggris** dibandingkan dengan **pre-trained model yang telah dituning dalam bahasa Indonesia** untuk memahami sejauh mana tuning bahasa berpengaruh terhadap hasil klasifikasi.  

---

## 🎯 2. Tujuan  
- **Membandingkan performa model Transformer dengan arsitektur berbeda** (encoder-only, decoder-only, dan encoder-decoder) dalam tugas klasifikasi teks berita berbahasa Indonesia untuk menentukan model yang paling efektif dalam memahami dan mengklasifikasikan artikel berita.  

- **Menganalisis pengaruh tuning bahasa terhadap performa klasifikasi** dengan membandingkan hasil klasifikasi antara **pre-trained model multibahasa** dan **pre-trained model yang telah dituning dalam bahasa Indonesia**, guna memahami sejauh mana tuning berkontribusi dalam meningkatkan akurasi model.  

---

## 📚 3. Dataset  
Dataset yang digunakan adalah **Indonesia News Dataset**, yang berisi **32.127 artikel berita** dari sumber berita terkemuka di Indonesia:  

- **CNN Indonesia**: 3.491 artikel  
- **Jawa Pos**: 1.014 artikel  
- **Kumparan**: 8.414 artikel  
- **Okezone**: 7.066 artikel  
- **Suara**: 4.004 artikel  
- **Tempo**: 3.632 artikel  

Total ukuran dataset: **737.92 MB**.  
Dataset ini mencakup berbagai kategori berita, memungkinkan evaluasi model pada beragam topik.  

Sebelum digunakan untuk training, dataset mengalami **preprocessing** yaitu:  
- **Menghapus nama penerbit** seperti `"Tempo.com"`, `"CNBC Indonesia"`, `"Suara.com"` dalam teks artikel untuk memastikan bahwa model tidak belajar dari sumber berita tetapi dari isi artikel.  

---

## 🏗 4. Model Penelitian  
Penelitian ini menguji dan membandingkan tiga kategori model Transformer:  

### **1️⃣ Encoder-Only Models**  
- **BERT** (Bidirectional Encoder Representations from Transformers)  
- **RoBERTa** (Robustly Optimized BERT Approach)  
- **DistilBERT** (Distilled version of BERT)  

Encoder-only models berfokus pada pemahaman teks melalui representasi bidirectional.  

### **2️⃣ Decoder-Only Model**  
- **GPT-2** (Generative Pretrained Transformer 2)  

Model ini lebih umum digunakan untuk tugas **generasi teks**, namun dalam penelitian ini, GPT-2 diuji untuk tugas klasifikasi.  

### **3️⃣ Encoder-Decoder Model**  
- **T5** (Text-to-Text Transfer Transformer)  

T5 mampu menangani berbagai tugas NLP dengan pendekatan **text-to-text**. Dalam penelitian ini, model diuji dalam konteks klasifikasi berita.  

---

## 📊 5. Hasil  
Evaluasi dilakukan menggunakan metrik **Akurasi, F1-Score, Precision, dan Recall**. Berikut adalah hasil awal dari eksperimen:  

### 1. **Menggunakan pre-trained model berbahasa Indonesia**  
| Model       | Tipe Arsitektur   | Pre-trained Model | Akurasi | F1-Score | Precision | Recall  |
|------------|------------------|------------------|---------|----------|-----------|---------|
| BERT       | Encoder-Only     |cahya/bert-base-indonesian-522M| 94%   | 92%    | 92%     | 92%   |
| RoBERTa    | Encoder-Only     |cahya/roberta-base-indonesian-522M| 95%   | 92%    | 92%     | 92%   |
| DistilBERT | Encoder-Only     |cahya/distilbert-base-indonesian| 95%   | 93%    | 94%     | 93%   |
| GPT-2      | Decoder-Only     |cahya/gpt2-small-indonesi an-522M| 96%   | 93%    | 93%     | 93%   |
| T5         | Encoder-Decoder  |LazarusNLP/indo-t5-base| 93%   | 89%    | 89%     | 88%   |

### 2. **Menggunakan pre-trained model berbahasa Inggris (belum dituning dalam bahasa Indonesia)**    
| Model       | Tipe Arsitektur   | Pre-trained Model | Akurasi | F1-Score | Precision | Recall  |
|------------|------------------|------------------|---------|----------|-----------|---------|
| BERT       | Encoder-Only     |google-bert/bert-base-uncased| 92%   | 87%    | 89%     | 90%   |
| RoBERTa    | Encoder-Only     |FacebookAI/roberta-large-mnli| 93%   | 89%    | 89%     | 90%   |
| DistilBERT | Encoder-Only     |distilbert/distilbert-base-uncased-finetuned-sst-2-english| 92%   | 87%    | 86%     | 87%   |
| GPT-2      | Decoder-Only     |open-ai/gpt2| 93%   | 89%    | 89%     | 88%   |
| T5         | Encoder-Decoder  |google/mt5-base| 77%   | 73%    | 77%     | 71%   |

## 📌 Kesimpulan

### 🔍 Perbandingan Arsitektur Model  
Model **Encoder-Only** (BERT, RoBERTa, DistilBERT) dan **Decoder-Only** (GPT-2) menunjukkan performa yang kompetitif dengan akurasi **94%-96%** saat menggunakan pre-trained model bahasa Indonesia.  

- **Encoder-Only** lebih unggul dalam memahami hubungan antar kata dalam satu teks, yang sangat penting dalam klasifikasi.  
- **Decoder-Only (GPT-2)** meskipun dirancang untuk tugas generatif, tetap mampu menangkap pola distribusi kata dengan baik sehingga dapat digunakan untuk klasifikasi.  

Model **Encoder-Decoder (T5)** memiliki performa lebih rendah dibandingkan dua arsitektur lainnya, terutama dalam kondisi tanpa tuning bahasa.  
- Hal ini disebabkan karena arsitektur Encoder-Decoder lebih kompleks dan dirancang untuk tugas sekuensial seperti terjemahan atau ringkasan teks.  
- Dalam tugas klasifikasi, model ini memerlukan lebih banyak penyesuaian agar dapat memahami dan merepresentasikan data secara efektif.  

### 🌍 Pengaruh Tuning Bahasa  
Model yang telah **dituning dalam bahasa Indonesia** menunjukkan akurasi lebih tinggi dibandingkan model pre-trained bahasa Inggris tanpa tuning.  

- Model yang telah dituning lebih memahami **struktur sintaksis, morfologi, dan kosakata bahasa Indonesia**, yang berbeda dari bahasa Inggris.  
- Perbedaan paling signifikan terlihat pada **T5 (Encoder-Decoder)**, di mana model pre-trained bahasa Inggris hanya mencapai **77% akurasi**, sementara model yang telah dituning dalam bahasa Indonesia mencapai **93% akurasi**.  
  - Ini menunjukkan bahwa model Encoder-Decoder lebih bergantung pada pemahaman konteks bahasa yang baik, dan tanpa tuning, model ini kesulitan menangkap pola bahasa Indonesia secara optimal.  
- Model **Encoder-Only dan Decoder-Only** mengalami **penurunan performa sekitar 1-3%** tanpa tuning, tetapi tetap menunjukkan hasil yang cukup baik.  
  - Hal ini karena model seperti **BERT dan RoBERTa** telah dilatih dengan struktur Transformer yang efektif dalam memahami teks lintas bahasa, sehingga masih dapat menangani klasifikasi bahasa Indonesia meskipun tidak optimal.  

---
