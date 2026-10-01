# Penugasan-Tutorial-Computer-Vision


# Evaluasi YOLOv8 pada Deteksi Objek Kecil di VisDrone melalui Peningkatan Resolusi Input

Repositori ini berisi implementasi eksperimen Computer Vision untuk mengevaluasi pengaruh peningkatan resolusi input terhadap kemampuan YOLOv8 dalam mendeteksi objek kecil pada citra udara.

## Tujuan

Eksperimen ini menguji pertanyaan:

> Apakah keterbatasan YOLO dalam mendeteksi objek kecil dan padat masih terlihat pada YOLOv8 modern, dan apakah peningkatan resolusi input dapat membantu mengatasinya?

Eksperimen membandingkan dua konfigurasi:

* **Baseline:** YOLOv8n dengan input 640×640.
* **Intervensi:** YOLOv8n dengan input 1280×1280.

Intervensi yang diuji adalah peningkatan resolusi input tanpa melakukan perubahan arsitektur YOLOv8.

## Dataset

Dataset yang digunakan adalah **VisDrone2019-DET**, yaitu dataset deteksi objek pada citra yang diperoleh menggunakan platform drone. Dataset memiliki variasi ukuran objek, occlusion, dan kondisi latar yang kompleks.

Dataset yang digunakan dalam eksperimen terdiri dari 10 kelas objek:

* pedestrian
* people
* bicycle
* car
* van
* truck
* tricycle
* awning-tricycle
* bus
* motor

Notebook menggunakan konfigurasi `VisDrone.yaml` dari Ultralytics sehingga dataset dapat diunduh dan dipersiapkan secara otomatis ketika notebook dijalankan.

### Referensi Dataset

Du, D., Zhu, P., Wen, L., Bian, X., Ling, H., Hu, Q., et al., “VisDrone-DET2019: The Vision Meets Drone Object Detection in Image Challenge Results,” *Proceedings of the IEEE/CVF International Conference on Computer Vision Workshops (ICCVW)*, 2019, pp. 213–226, doi: 10.1109/ICCVW.2019.00030.

## Eksperimen

### Konfigurasi

| Konfigurasi     | Model   |  Resolusi | Epoch | Batch Size |
| --------------- | ------- | --------: | ----: | ---------: |
| Baseline        | YOLOv8n |   640×640 |    10 |         16 |
| High-resolution | YOLOv8n | 1280×1280 |    10 |          4 |

Konfigurasi umum:

* **Random seed:** 42
* **Pretrained:** True
* **Workers:** 2
* **Deterministic training:** True
* **Evaluation:** validation set

Sepuluh epoch digunakan agar eksperimen dapat dijalankan dalam waktu yang sesuai dengan lingkungan Google Colab. Oleh karena itu, eksperimen ini tidak dimaksudkan sebagai replikasi langsung terhadap konfigurasi training pada paper referensi.

## Metrik Evaluasi

Metrik utama yang digunakan:

* **mAP@0.5**
* **mAP@0.5:0.95**

Metrik tambahan:

* Precision
* Recall

Analisis tambahan dilakukan berdasarkan ukuran objek:

* Small
* Medium
* Large

## Hasil Eksperimen

### Hasil Keseluruhan

| Model                   |     Input |  mAP@0.5 | mAP@0.5:0.95 | Precision |   Recall |
| ----------------------- | --------: | -------: | -----------: | --------: | -------: |
| YOLOv8n Baseline        |   640×640 | 0.234137 |     0.131171 |  0.351433 | 0.272471 |
| YOLOv8n High-resolution | 1280×1280 | 0.380451 |     0.227675 |  0.489474 | 0.396570 |

Perubahan dari baseline ke high-resolution:

* **mAP@0.5:** +0.146314
* **mAP@0.5:0.95:** +0.096503

### Hasil Berdasarkan Ukuran Objek

| Ukuran Objek | Baseline mAP@0.5:0.95 | High-resolution mAP@0.5:0.95 |
| ------------ | --------------------: | ---------------------------: |
| Small        |                0.0478 |                       0.1241 |
| Medium       |                0.1887 |                       0.3252 |
| Large        |                0.4259 |                       0.4171 |

Hasil menunjukkan bahwa peningkatan resolusi memberikan peningkatan kinerja pada objek **small** dan **medium**, sedangkan kinerja pada objek **large** relatif serupa.

## Struktur Notebook

Notebook eksperimen terdiri dari beberapa tahapan:

1. Instalasi dependensi
2. Pemeriksaan GPU
3. Pengunduhan dan persiapan dataset VisDrone
4. Pemeriksaan dataset dan statistik
5. Visualisasi sampel citra
6. Training YOLOv8n baseline 640×640
7. Evaluasi baseline
8. Training YOLOv8n 1280×1280
9. Evaluasi high-resolution
10. Analisis performa berdasarkan ukuran objek
11. Evaluasi menggunakan COCOeval
12. Visualisasi hasil deteksi
13. Analisis failure cases
14. Penyimpanan hasil eksperimen
15. Ringkasan hasil


## Visualisasi

Laporan menggunakan tiga visualisasi utama:

1. **Contoh citra VisDrone2019-DET** untuk menunjukkan karakteristik objek kecil dan padat.
2. **Perbandingan failure case** antara baseline 640×640 dan high-resolution 1280×1280.

## Paper Referensi Utama

Eksperimen ini menggunakan paper berikut sebagai referensi utama mengenai deteksi objek kecil pada YOLO:

> Hu et al., “A Universal Structure of YOLO Series Small Object Detection Models,” *ACCV*, 2024.

Paper tersebut membahas keterbatasan YOLO pada deteksi objek kecil dan padat serta mengusulkan perubahan struktur YOLO untuk meningkatkan kemampuan deteksi objek kecil.

Pada eksperimen ini, intervensi yang diuji adalah **peningkatan resolusi input**, bukan implementasi modul arsitektur DGB dan FRM dari paper tersebut.

## Keterbatasan

* Eksperimen menggunakan **YOLOv8n**, bukan YOLOv8-L seperti pada eksperimen paper referensi.
* Training dibatasi menjadi **10 epoch** untuk menyesuaikan keterbatasan waktu komputasi.
* Intervensi yang diuji hanya berupa peningkatan resolusi input.
* Evaluasi dilakukan pada validation set.
* Eksperimen tidak dimaksudkan sebagai replikasi langsung terhadap hasil numerik paper referensi.

