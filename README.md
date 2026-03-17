![jujutsu kaisen character](https://github.com/user-attachments/assets/75a9ba6a-df54-4608-a41b-047adaa210d8)

# Image Classification - Jujutsu Kaisen Character
Proyek ini mengimplementasi **klasifikasi gambar karakter anime** dari serial Jujutsu Kaisen menggunakan metode Deep Learning. Model yang digunakan sebagai berikut

- *Convolutional Neural Network* (CNN)
- Transfer Learning menggunakan VGG16

Dengan tujuan utama untuk mengklasifikasikan gambar ke dalam **17 kelas karakter** dengan akurasi yang tinggi dan stabil.

## Problem Statement
Dalam computer vision, klasifikasi karakter anime memiliki tantangan tersendiri seperti variasi pose dan ekspresi, perbedaan cahaya, hingga kemiripan visual antar karakter. Dengan dataset yang besar (> 10.000 gambar), model ini diharapkan dapat mengklasifikasikan karakter secara akurat, mengurangi overfitting melalui augmentasi. 

## Executive Summary
Dua pendekatan utama yang digunakan dalam proyek ini, memiliki perbedaan:

1. CNN

   - Dibangun diawal, dengan custom lapisan konvolusi dan pooling
   - Lebih ringan
   - Performa mencapai sebesar 90%
     
2. *Transfer Learning* (VGG16)

   - Menggunakan *pretrained* dari *ImageNet*
   - Akurasi yang dihasilkan lebih tinggi
   - Training lebih cepat konvergen

Hasil evaluasi dan analisis kedua pendekatan tersebut, menunjukkan bahwa *transfer learning* memberikan peningkatan akurasi, data augmentasi berperan penting dalam generalisasi model, dan overfitting tipis dapat ditekan dengan penambahan dropout dan callback. 

## Project Approch
📁 Dataset

Dataset yang digunakan bersalah dari [Kaggle](https://www.kaggle.com/datasets/angelirodriguez/jujutsu-kaisen-character-dataset)

Dataset gambar asli terdiri dari folder train dan folder val yang didalamnya berisikan lagi data berupa folder sesuai nama karakter, total sebanyak 21 folder. Namun, pada proyek ini dataset yang digunakan hanya terdiri dari **17 kelas karakter** dari serial anime Jujutsu Kaisen, dengan total gambar awal sebanyak 3.557 gambar. 

Dengan distribusi data awal **tidak seimbang**, di mana beberapa kelas memiliki jumlah data sangat sedikit, sementara kelas lain memiliki hingga 956 gambar. Dengan resolusi tiap gambar yang **tidak seragam** pula. Dengan struktur folder sebagai berikut

      clean_jjk_data/
      │
      ├── dataset/
         ├── Aoi Todo/
         ├── Junpei Yoshino/
         ├── Kasumi Miwa/
         ├── Kento Nanami/
         ├── Kiyotaka Ijichi/
         ├── Mahito/
         ├── Mai Zenin/
         ├── Maki Zenin/
         ├── Megumi Fushiguro/
         ├── Momo Nishimiya/
         ├── Nobara Kugisaki/
         ├── Noritosho Kamo/
         ├── Panda/
         ├── Satoru Gojo/
         ├── Sukuna/
         ├── Toge Inumaki/
         └── Yuji Itadori/

🧹*Image Preparation*

Untuk mengatasi permasalahan dataset yang imbalance dan juga tidak seragamnya resolusi gambar, dilakukan beberapa proses 

- Seluruh gambar melalui proses standarisasi ukuran menjadi **224x224** piksel untuk memastikan konsistensi input model.
- Mengatasi *class imbalance*, dengan proses data augmentation hingga setiap kelas (kecuali yang sudah besar) memiliki sekitar 650 gambar. Sehingga total dataset gambar sebanyak **11.356 gambar.** Dan distribusi menjadi lebih seimbang antar kelas. 

🔄️*Image Augmentation*

Dilakukan beberapa transformasi pada gambar untuk meningkatkan variasi data. Beberapa teknik yang digunakan adalah

- Rotasi gambar : Memutar gambar secara acak dalam rentang sudut tertentu
- Brigthness : Untuk mensimulasikan kondisi pencahayaan yang berbeda
- Blur : Untuk memberikan efek kabur pada gambar sehingga model lebih tahan terhadap noise visual
- Shear : Untuk memberikan efek pergeseran atau kemiringan pada gambar
- Horizontal Flip : Untuk membalikkan gambar secara horizontal sehingga gambar dapat dikenali dari arah yang berbeda
  
## Modelling
Pada proyek ini digunakan dua pendekatan

1. CNN

   - Model CNN dibangun dengan beberapa layer konvolusi Con2VD - BatchNormalization - MaxPooling
   - GlobalAveragePooling untuk mereduksi fitur
   - Dropout untuk mengurangi overfitting
   - Ouput layer Softmax (17 kelas)
     
2. VGG16

   - Layer awal di *freeze* (feature extractor)
   - Layer akhir di *fine tune*
   - Dengan GlobalAverage, BatchNormalization, Dense, dan Dropout
  
Penggunaan parameter dan callback seperti

   - Optimizer: RMSprop
   - Epoch: 25-30
   - EarlyStopping
   - ModelCheckpoint
   - ReduceLROnPlateau
     
## Model Evaluation
Perbandingan performa kedua model

| Model      | Accuracy |
| ---------- | -------- |
| CNN        | 91%      |
| VGG16      | 98%      |

Hasil akurasi tersebut menunjukkan bahwa CNN cukup baik untuk baseline, kemudian VGG16 memberikan peningkatan yang signifikan karena memanfaatkan pretrained feature ini efektif untuk dataset gambar seperti karakter. 
