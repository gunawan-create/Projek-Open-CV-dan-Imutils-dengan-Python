# Projek-Open-CV-dan-Imutils-dengan-Python
Nama : Ali Gunawan | Kelas : I241C | NIM : 312410400

## LAPORAN PROJECT DETEKSI PEJALAN KAKI MENGGUNAKAN OPENCV-PYTHON
### 1. Pendahuluan
Perkembangan teknologi Computer Vision saat ini semakin pesat dan banyak diterapkan dalam berbagai bidang seperti keamanan, kendaraan otonom, monitoring lalu lintas, dan sistem pengawasan otomatis. Salah satu implementasi Computer Vision adalah deteksi pejalan kaki (pedestrian detection). Pada project ini dilakukan pembuatan sistem deteksi pejalan kaki menggunakan bahasa pemrograman Python dengan bantuan library OpenCV dan Imutils. Sistem ini mampu mendeteksi keberadaan manusia pada gambar maupun video dengan memberikan kotak pembatas (bounding box) pada objek yang terdeteksi. Metode yang digunakan adalah Histogram of Oriented Gradients (HOG) dan Support Vector Machine (SVM) yang telah tersedia secara bawaan pada OpenCV.

### 2. Tujuan Project
Tujuan dari project ini adalah:

- Memahami konsep dasar Computer Vision.
- Mempelajari penggunaan library OpenCV pada Python.
- Membuat sistem deteksi manusia pada gambar dan video.
- Mengimplementasikan metode HOG Descriptor untuk mendeteksi pejalan kaki.
- Menampilkan hasil deteksi objek secara real-time.

### 3. Tools dan Library
Adapun tools dan library yang digunakan pada project ini yaitu:

- Python 3
- OpenCV
- Imutils
- Visual Studio Code
- Video atau gambar sebagai media pengujian
- 
Instalasi library dilakukan menggunakan perintah berikut:

- pip install opencv-python
- pip install imutils
- 
### 4. Penjelasan Sistem
Sistem bekerja dengan membaca gambar atau video menggunakan OpenCV. Setelah itu program akan menggunakan metode HOG Descriptor untuk mengenali bentuk manusia berdasarkan pola gradien pada citra.

Langkah kerja sistem:

- Membaca gambar atau video.
- Mengubah ukuran frame agar proses lebih ringan.
- Mendeteksi manusia menggunakan HOG + SVM.
- Memberikan kotak merah pada objek yang terdeteksi.
- Menampilkan hasil deteksi pada layar.

### 5. Source Code dan Penjelasan Program

## A. Program Deteksi Pejalan Kaki pada Gambar

### Source Code

```python id="r89x6v"
import cv2
import imutils

# Inisialisasi HOG Descriptor
hog = cv2.HOGDescriptor()
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())

# Membaca gambar
image = cv2.imread('img.png')

# Resize gambar
image = imutils.resize(image,
                       width=min(400, image.shape[1]))

# Deteksi manusia
(regions, _) = hog.detectMultiScale(
    image,
    winStride=(4, 4),
    padding=(4, 4),
    scale=1.05)

# Membuat kotak deteksi
for (x, y, w, h) in regions:
    cv2.rectangle(image,
                  (x, y),
                  (x + w, y + h),
                  (0, 0, 255), 2)

# Menampilkan hasil
cv2.imshow("Deteksi Gambar", image)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Penjelasan Program

1. Library `cv2` digunakan untuk pengolahan citra menggunakan OpenCV.
2. Library `imutils` digunakan untuk mempermudah proses resize gambar.
3. Program menggunakan `HOGDescriptor()` untuk mendeteksi bentuk manusia.
4. Fungsi `detectMultiScale()` digunakan untuk mencari objek manusia pada gambar.
5. Jika manusia terdeteksi maka program membuat kotak merah menggunakan `cv2.rectangle()`.
6. Hasil akhir ditampilkan pada window OpenCV.

### Hasil Output

<img width="301" height="200" alt="Hasil Tampilan" src="https://github.com/user-attachments/assets/41db3f05-30f5-4c8d-97bc-3fe0def71f7b" />

Pada output program, gambar akan muncul pada layar dan manusia yang terdeteksi akan diberi kotak berwarna merah. Sistem mampu mendeteksi satu atau lebih orang pada gambar sesuai kualitas citra yang digunakan.

---

## B. Program Deteksi Pejalan Kaki pada Video

### Source Code

```python id="9if2nl"
import cv2
import imutils

# Inisialisasi HOG Descriptor
hog = cv2.HOGDescriptor()
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())

# Membaca video
cap = cv2.VideoCapture('vid.mp4')

while cap.isOpened():

    # Membaca frame video
    ret, image = cap.read()

    if ret:

        # Resize frame
        image = imutils.resize(
            image,
            width=min(400, image.shape[1]))

        # Deteksi manusia
        (regions, _) = hog.detectMultiScale(
            image,
            winStride=(4, 4),
            padding=(4, 4),
            scale=1.05)

        # Membuat kotak deteksi
        for (x, y, w, h) in regions:
            cv2.rectangle(
                image,
                (x, y),
                (x + w, y + h),
                (0, 0, 255), 2)

        # Menampilkan hasil
        cv2.imshow("Deteksi Video", image)

        # Tombol keluar
        if cv2.waitKey(25) & 0xFF == ord('q'):
            break

    else:
        break

cap.release()
cv2.destroyAllWindows()
```

### Penjelasan Program

1. Program membaca video menggunakan `VideoCapture()`.
2. Video diproses frame per frame secara real-time.
3. Setiap frame akan di-resize agar proses lebih ringan.
4. Metode HOG + SVM digunakan untuk mendeteksi manusia pada setiap frame video.
5. Jika objek manusia ditemukan maka program membuat kotak merah pada tubuh manusia.
6. Hasil deteksi ditampilkan secara terus menerus hingga video selesai atau tombol `q` ditekan.

### Hasil Output

Output program berupa tampilan video yang berjalan secara real-time. Setiap manusia yang terdeteksi akan diberi kotak merah pada tubuhnya. Sistem dapat mendeteksi beberapa orang sekaligus apabila terlihat jelas pada video.

Hasil deteksi dipengaruhi oleh:

* kualitas video,
* pencahayaan,
* posisi tubuh manusia,
* dan jumlah objek pada frame.

6. Hasil Pengujian

Berdasarkan hasil pengujian, sistem berhasil mendeteksi manusia pada video dengan memberikan kotak berwarna merah di sekitar tubuh manusia.

Deteksi bekerja dengan baik pada kondisi:

- pencahayaan cukup terang,
- tubuh manusia terlihat jelas,
- dan objek tidak terlalu kecil.

Namun terdapat beberapa kekurangan seperti:

- deteksi kurang akurat pada kondisi gelap,
- terkadang terjadi false detection,
- dan performa menurun jika video terlalu ramai.

### 7. Kesimpulan
Berdasarkan project yang telah dilakukan dapat disimpulkan bahwa OpenCV dapat digunakan untuk membangun sistem deteksi pejalan kaki menggunakan metode HOG Descriptor dan SVM. Sistem mampu mendeteksi manusia pada gambar maupun video secara cukup baik dan dapat diterapkan sebagai dasar pengembangan sistem keamanan, monitoring, dan kendaraan cerdas. Project ini juga memberikan pemahaman mengenai implementasi Computer Vision menggunakan Python secara langsung.
