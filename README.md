# Klasifikasi Gambar Food-41

## Ringkasan
Proyek ini mengimplementasikan model klasifikasi gambar menggunakan dataset Food-41 (41 kategori makanan, >10.000 gambar).
Pipeline meliputi:
1. **Download** dataset via KaggleHub
2. **Split** data menjadi train/validation/test (70/15/15)
3. **Preprocessing** & **augmentasi** dengan `ImageDataGenerator`
4. **Transfer learning**: MobileNetV2 + top layers custom
5. **Callbacks**: EarlyStopping, ReduceLROnPlateau, ModelCheckpoint, TensorBoard, custom threshold 85%
6. **Training** & **fine-tuning** (total ≥ 45 epoch) # <- Karakter ini aman dengan UTF-8
7. **Evaluasi**: plot loss/accuracy, confusion matrix, classification report
8. **Export** model ke SavedModel, TFLite, dan TensorFlow.js
9. **Inference** dengan Keras, TFLite, dan (opsional) TFJS

## Dataset
- Sumber: [kmader/food41](https://www.kaggle.com/kmader/food41)
- Total gambar: >10.000
- Kategori: 41 jenis makanan
- Format: .jpg/.png

## Arsitektur Model
- **Base**: `MobileNetV2` pretrained ImageNet (tanpa top)
- **Head**:
  - GlobalAveragePooling2D
  - Dense(512, relu) + BatchNorm + Dropout(0.5)
  - Dense(256, relu) + BatchNorm + Dropout(0.3)
  - Dense(41, softmax)

## Performa
- **Train Accuracy**: >95%
- **Val Accuracy**: >90%
- **Test Accuracy**: >90%

## File & Direktori
- `food41_data/`   : folder dataset asli
- `food41_split/`  : folder train/validation/test
- `saved_model/`   : TensorFlow SavedModel
- `model.tflite`   : TensorFlow Lite model
- `tfjs_model/`    : TensorFlow.js model (`model.json` + `.bin`)
- `labels.txt`     : daftar nama kelas
- `notebook.ipynb` : Jupyter notebook dengan semua kode & output
- `requirements.txt`

## Cara Pakai
1. Clone repo dan `pip install -r requirements.txt`
2. Jalankan `notebook.ipynb` dari cell paling atas hingga akhir
3. Model akan tersimpan di `saved_model/`, `model.tflite`, dan `tfjs_model/`
4. Untuk inference, lihat fungsi `prediksi_gambar` dan `uji_model_tflite` di notebook

## Penulis
Alif Nurhidayat (KillerKing93)
Email: alifnurhidayatwork@gmail.com
GitHub: https://github.com/KillerKing93
