# Klasifikasi Gambar Food-41

## Ringkasan
Proyek ini mengimplementasikan model klasifikasi gambar menggunakan dataset Food-41 (41 kategori makanan, >10.000 gambar).
Pipeline meliputi:
1.  **Download** dataset via sumber Kaggle ([kmader/food41](https://www.kaggle.com/kmader/food41)), kemungkinan menggunakan KaggleHub atau library terkait, ke dalam direktori `datasets/`.
2.  **Split** data menjadi train/validation/test (70/15/15) ke dalam direktori `food41_split/`.
3.  **Preprocessing** & **augmentasi** dengan `ImageDataGenerator`.
4.  **Transfer learning**: MobileNetV2 + top layers custom.
5.  **Callbacks**: EarlyStopping, ReduceLROnPlateau, ModelCheckpoint, TensorBoard (log di `logs/fit/`), custom threshold 85%.
6.  **Training** & **fine-tuning** (target total ≥ 45 epoch).
7.  **Evaluasi**: plot loss/accuracy, confusion matrix, classification report.
8.  **Export** model ke SavedModel, TFLite, dan TensorFlow.js ke dalam direktori `model_output/`.
9.  **Inference** dengan Keras, TFLite, dan (opsional) TFJS.

## Dataset
-   **Sumber**: [kmader/food41](https://www.kaggle.com/kmader/food41)
-   **Lokasi Unduh**: `datasets/food-41/`
-   **Lokasi Split**: `food41_split/` (train/validation/test)
-   **Total gambar**: >10.000
-   **Kategori**: 41 jenis makanan
-   **Format**: .jpg/.png

## Arsitektur Model
-   **Base**: `MobileNetV2` pretrained ImageNet (tanpa top).
-   **Head**:
    -   GlobalAveragePooling2D
    -   Dense(512, relu) + BatchNorm + Dropout(0.5)
    -   Dense(256, relu) + BatchNorm + Dropout(0.3)
    -   Dense(41, softmax)

## Performa (Target/Hasil)
-   **Train Accuracy**: >95%
-   **Val Accuracy**: >90%
-   **Test Accuracy**: >90%

## File & Direktori Penting
-   `datasets/`
    -   `food-41/`: Folder berisi dataset asli yang diunduh.
-   `food41_split/`: Folder berisi data train/validation/test hasil pemisahan.
    -   `train/`
    -   `validation/`
    -   `test/`
-   `logs/`
    -   `fit/`: Direktori berisi log TensorBoard dari proses training.
-   `model_output/`: Direktori berisi semua hasil ekspor model dan label.
    -   `saved_model/`: Model dalam format TensorFlow SavedModel.
    -   `tfjs_model/`: Model dalam format TensorFlow.js (`model.json` + shard `.bin`).
    -   `model.tflite`: File model dalam format TensorFlow Lite.
    -   `labels.txt`: File teks berisi daftar nama kelas/kategori makanan.
-   `Proyek_Klasifikasi_Gambar_V3.ipynb`: Jupyter notebook utama proyek.
-   `README.md`: File ini.
-   `requirements.txt`: Daftar dependensi Python.

## Cara Pakai

1.  **Clone Repositori:**
    ```bash
    git clone [https://github.com/KillerKing93/Dicoding-Proyek-Klasifikasi-Gambar/](https://github.com/KillerKing93/Dicoding-Proyek-Klasifikasi-Gambar/)
    cd Dicoding-Proyek-Klasifikasi-Gambar
    ```

2.  **Buat dan Aktifkan Lingkungan Virtual (Sangat Direkomendasikan):**
    Menggunakan lingkungan virtual (`venv`) membantu mengisolasi paket Python yang dibutuhkan oleh proyek ini dari paket lain di sistem Anda. Ini mencegah potensi konflik versi.

    * **Buat Lingkungan Virtual:**
        Buka terminal atau command prompt di direktori proyek Anda, lalu jalankan:
        ```bash
        python -m venv venv
        ```
        *(Jika Anda memiliki Python 2 dan 3 terinstal, Anda mungkin perlu menggunakan `python3 -m venv venv`)*
        Ini akan membuat folder baru bernama `venv` di dalam direktori proyek Anda.

    * **Aktifkan Lingkungan Virtual:**
        Cara mengaktifkannya berbeda tergantung pada sistem operasi dan shell yang Anda gunakan:
        * **Linux / macOS (Bash/Zsh):**
            ```bash
            source venv/bin/activate
            ```
        * **Windows (Command Prompt):**
            ```cmd
            .env\Scriptsctivate.bat
            ```
            *(Terkadang cukup dengan: `venv\Scriptsctivate`)*
        * **Windows (PowerShell):**
            ```powershell
            .env\Scripts\Activate.ps1
            ```
            *(Catatan: Jika Anda mendapatkan error terkait eksekusi skrip, Anda mungkin perlu menjalankan PowerShell sebagai Administrator dan mengetik: `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`, lalu tekan 'Y' untuk konfirmasi.)*
        * **Windows (Git Bash):**
            ```bash
            source venv/Scripts/activate
            ```
        Setelah aktivasi berhasil, Anda akan melihat `(venv)` muncul di awal baris prompt terminal Anda, menandakan bahwa lingkungan virtual sedang aktif.

3.  **Instal Dependensi:**
    Pastikan lingkungan virtual Anda sudah aktif (lihat langkah 2). Kemudian, instal semua paket yang diperlukan dari file `requirements.txt`:
    ```bash
    pip install -r requirements.txt
    ```
    *(Pastikan file `requirements.txt` ada di direktori proyek dan berisi daftar library yang benar seperti tensorflow, numpy, matplotlib, scikit-learn, dll.)*

4.  **Jalankan Notebook:**
    Sekarang Anda siap menjalankan notebook. Buka dan jalankan file `Proyek_Klasifikasi_Gambar_V3.ipynb` dari sel paling atas hingga akhir menggunakan Jupyter Notebook, Jupyter Lab, Google Colab (jika diunggah), atau VS Code. Proses ini akan:
    * Mengunduh data ke `datasets/food-41/`.
    * Membuat split data di `food41_split/`.
    * Melatih model (log disimpan di `logs/fit/`).
    * Mengevaluasi model.
    * Menyimpan semua output model dan label ke dalam direktori `model_output/`.

5.  **Hasil Model:**
    Semua artefak model (`saved_model/`, `tfjs_model/`, `model.tflite`) dan `labels.txt` akan ditemukan di dalam direktori `model_output/`.

6.  **Inference:**
    Untuk melakukan prediksi pada gambar baru, lihat implementasi fungsi `prediksi_gambar` (untuk model Keras/SavedModel) dan `uji_model_tflite` (untuk model TFLite) yang ada di dalam notebook. Sesuaikan path ke model jika diperlukan untuk menunjuk ke dalam `model_output/`.

7.  **Nonaktifkan Lingkungan Virtual (Opsional):**
    Jika Anda sudah selesai bekerja dengan proyek ini, Anda dapat menonaktifkan lingkungan virtual dengan mengetik perintah berikut di terminal Anda:
    ```bash
    deactivate
    ```
    Prompt terminal Anda akan kembali normal.

## Penulis
-   **Nama:** Alif Nurhidayat (KillerKing93)
-   **Email:** alifnurhidayatwork@gmail.com
-   **GitHub:** [https://github.com/KillerKing93](https://github.com/KillerKing93)

---
*README ini diperbarui berdasarkan struktur direktori aktual proyek dan menyertakan instruksi venv yang lebih detail.*
