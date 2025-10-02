# 🌦️ Climate and Atmospheric Conditions Data Quality Report

## 📌 Deskripsi Dataset
Dataset ini berisi data iklim dan kondisi atmosfer per jam sepanjang tahun 2012, dengan total **8.784 entri** dan **8 kolom**:

- **Date/Time** → Waktu pengukuran  
- **Temp_C** → Suhu udara (°C)  
- **Dew Point Temp_C** → Suhu titik embun (°C)  
- **Rel Hum_%** → Kelembaban relatif (%)  
- **Wind Speed_km/h** → Kecepatan angin (km/jam)  
- **Visibility_km** → Jarak pandang (km)  
- **Press_kPa** → Tekanan udara (kPa)  
- **Weather** → Kondisi cuaca  

---

## 🔹 Analisis Data Quality

### 1. Missing Values
- Tidak ditemukan missing values pada semua kolom.  
✅ **Data lengkap.**

### 2. Duplikat
- Jumlah duplikat: **0**  
✅ **Tidak ada duplikasi.**

### 3. Statistik Deskriptif
| Kolom             | Min   | Max   | Mean    | Std Dev  |
|-------------------|-------|-------|---------|----------|
| Temp_C            | -23.3 | 33.0  | 8.79    | 11.68    |
| Dew Point Temp_C  | -28.5 | 24.4  | 2.55    | 10.88    |
| Rel Hum_%         | 18    | 100   | 67.4    | 16.9     |
| Wind Speed_km/h   | 0     | 83    | 14.9    | 8.6      |
| Visibility_km     | 0.2   | 48.3  | 27.6    | 12.6     |
| Press_kPa         | 97.5  | 103.6 | 101.0   | 0.84     |

📌 Catatan: Suhu dan titik embun masih masuk akal untuk data iklim Kanada.  

### 4. Konsistensi: Temp vs. Dew Point
- **Aturan:** Titik embun seharusnya ≤ suhu.  
- Ditemukan **anomali** di mana `Dew Point Temp_C > Temp_C`.  
👉 Jumlah kasus: **beberapa entri terdeteksi** (cek dataframe `invalid_dew`).  

### 5. Distribusi Kategori Weather
- Total kategori unik: **50 jenis cuaca**  
- Kondisi dominan:
  - Mainly Clear (2106 kali)  
  - Mostly Cloudy (2069 kali)  
  - Cloudy (1728 kali)  
  - Clear (1326 kali)  
- Kondisi jarang: Hanya muncul sekali (misalnya: *Thunderstorms,Heavy Rain Showers*).  

### 6. Range Validasi Numerik
- Suhu: -23.3°C hingga 33°C → masuk akal  
- Kelembaban: 18% – 100% → valid  
- Angin: 0 – 83 km/jam → kecepatan 83 km/jam cukup tinggi, **perlu dicek apakah ekstrem atau outlier**  
- Tekanan: 97.5 – 103.6 kPa → normal  

### 7. Outlier Detection (IQR)
- Terindikasi ada **outlier** pada beberapa kolom, terutama:  
  - `Wind Speed_km/h` (nilai ekstrem > 60 km/jam)  
  - `Visibility_km` (nilai sangat rendah 0.2 km → kabut tebal)  

---

## 📊 Kesimpulan
1. **Data relatif bersih** → tidak ada missing values dan duplikat.  
2. **Ada anomali kecil** → beberapa titik embun lebih tinggi dari suhu.  
3. **Outlier wajar** → bisa dijelaskan fenomena cuaca ekstrem (kabut, badai, tekanan rendah).  
4. **Kondisi dominan**: Mainly Clear, Mostly Cloudy, Cloudy, dan Clear.  

---

## 🚀 Rekomendasi
- **Validasi ulang** entri dengan `Dew Point > Temp`.  
- **Tandai outlier** (misalnya `Wind Speed > 60 km/jam`) untuk analisis khusus.  
- **Kelompokkan kategori cuaca** (misalnya: Clear, Cloudy, Rain, Snow, Fog) agar analisis lebih sederhana.  
- Buat **visualisasi distribusi** (boxplot, bar chart) untuk memperkuat hasil analisis data quality.  
