# Telco_Churn_Analysis

Ini adalah proyek modul ke tiga sebagai prasyarat untuk dapat mengikuti final project Purwadhika Data Science and Machine Learning

# Apa yang Terjadi Saat Ini?
Sebuah perusahaan yang bergerak di bidang Telekomunikasi ingin mencari tahu seberapa besar prediksi customer churn atau tingkat pelanggan berhenti berlangganan pada layanan telekomunikasi perusahaan tersebut. Hal ini diperlukan untuk dapat membuat strategi bagaimana mempertahankan customer secara efektif dan efisien berdasarkan data-data yang sudah ada. 

Target :

No (0) : Tidak berhenti berlangganan

Yes (1) : Berhenti berlangganan/Churn

Goal yang diharapkan adalah mencari nilai Recall yang tinggi untuk mendeteksi nilai False Negative. Sehingga perusahaan dapat memprediksi jumlah False Negative dan melakukan strategi bisnis dengan memberikan offer dan layanan khusus sehingga customer tidak jadi melakukan churn. Hal ini tentu akan bermanfaat bagi perusahaan karena biaya yang diperlukan lebih rendah dibandingkan mendapatkan customer baru.

# Step of Work

**1. Data Preprocessing and Explanatory Data Analysis (EDA)**

Proses ini dimulai dengan melakukan import dataset dengan dataset Telco Churn Prediction yang dapat didapatkan di: https://drive.google.com/file/d/1oB5uKtJr7dWcWr9Ld5ZFAuVkpV005ceH/view 
Saya melakukan cleaning dengan mencari missing value, mengubah nilai Yes/No menjadi biner 1 dan 0, Melakukan One Hot Encoding, dan juga melakukan Scaling. Sejauh ini tidak ada data outlier dan nilai Nan/missing value tidak ditemukan. 

**2. Feature Engineering**

Karena nilai data pada kolom Tenure dan MonthlyCharges bervariasi, maka dilakukan Scaling, Selain itu data juga imbalance. Nilai yang tidak melakukan Churn (0) sebesar 73% sedangkan nilai Churn sebesar 26,7%. Oleh karena itu akan dilakukan proses penyeimbangan data menggunakan Random Oversampling dan SMOTE. 

**3. Modeling**

Saya melakukan modeling menggunakan data yang sudah bersih dan terstandarisasi. Selain itu saya mengaplikasikan Logistic Regression, Decision Tree, KNN. Random Forest, Adaboost, dan Extreme Gradient Boosting. 

**4. Evaluation**

Saya melakukan evaluasi menggunakan 5-fold-cross validation, Mean Score, Standard Deviasi Score dan Recall Score. Hasil evaluasi menggunakan Random Oversampler terlihat seperti berikut ini.
![image](https://user-images.githubusercontent.com/46861252/170717003-f77e6899-c89a-4a16-8918-46b7acaffe06.png)

Terlihat bahwa setelah dilakukan oversampling, mean scorenya meningkat tajam mencapai 97% dengan cross validation yang seimbang. Namun demikian memiliki recall yang rendah. Oleh karena metric evaluasi yang difokuskan adalah recall maka saya akan memilih menggunakan Adaboost Classifier karena memiliki mean score 81% dan recall 84%. Sehingga cukup ideal untuk dilakukan hypertuning. 

**5.HyperTuning Parameter**

Parameter untuk melakukan hypertuning adalah
```
grid['n_estimators'] = [10, 50, 100, 500]
grid['learning_rate'] = [0.0001, 0.001, 0.01, 0.1, 1.0]
```

Lalu melakukan GridsearchCV dengan metrik akurasi dan recall. Hasilnya seperti berikut:

![image](https://user-images.githubusercontent.com/46861252/170718248-2bd5ee2c-fe7c-4d2e-9e98-d9449ec91da7.png)

Terlihat dari classification report, meskipun setelah dituning recall menjadi 97%, namun akurasinya turun hingga 48%. Sedangkan setelah dituning melalui akurasi, nilainya turun 1%. Oleh karena itu kita akan menggunakan model default sebagai model akhir kita dan layak untuk diterima.

**6.Feature Importance**

![image](https://user-images.githubusercontent.com/46861252/170718704-e694bd9b-cd37-4d2d-b63f-63d45f566cc7.png)

Tampak dari grafik feature importance diketahui bahwa Biaya Bulanan atau Monthly Charges sangat mempengaruhi produksi hasil klasifikasi kita. Selain itu jumlah bulan berlangganan atau tenure juga mempengaruhi apakah customer akan churn atau tidak. Sedangkan Paperless Billing dan Dependents tidak terlalu mempengaruhi klasifikasi. Sehingga kedepannya mungkin dapat dihapus.

**7.Kesimpulan dan Rekomendasi**

![image](https://user-images.githubusercontent.com/46861252/170719208-ec007962-01c5-4c28-8b44-0f332f852d73.png)


Berdasarkan hasil classification report, kita dapat menyimpulkan bahwa bila seandainya nanti kita menggunakan model kita untuk menyaring customer yang akan melakukan churn atau tidak, maka model kita dapat mengurangi 71% customer yang tidak melakukan churn, dan model kita dapat mendapatkan 84% customer yang akan melakukan churn. (semua ini berdasarkan recallnya)

Model kita pun memiliki presisi untuk customer yang tidak melakukan churn sebesar 92%. Dan akurasi keseluruhan 75%. Namun memiliki presisi yang kurang baik untuk customer yang melakukan churn, yakni hanya sebesar 52%.

*Recommendation*

Hal yang bisa dilakukan untuk mengembangkan proyek lebih baik lagi adalah:

  - Menambahkan parameter pada hypertuning parameter sehingga akan didapatkan model lebih baik lagi.
  - Melakukan feature selection untuk mengurangi atau melakukan feature engineering untuk menambah fitur.
  - Membuat kebijakan baru pada tagihan bulanan apabila telah melebihi tenure setahun atau dua tahun akan mendapatkan penawaran khusus sehingga konsumen akan bertahan untuk berlangganan pada produk perusahaan.
