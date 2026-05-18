# Project-EVDAT-AnalisisKorelasi
## Kelompok Es Teh Manis


# Gene Expression Correlation Analysis of Murine Norovirus-Induced Hepatitis

Analisis eksplorasi dan visualisasi data ekspresi gen RNA-seq pada jaringan liver tikus model hepatitis akibat Murine Norovirus (MNV).

Dataset yang digunakan berasal dari GEO accession:

- GSE326527
- "Expression in myeloid cells restrains murine norovirus-induced hepatitis and fibrosis"

---

## Tujuan Analisis

Project ini bertujuan untuk:

- Mengeksplorasi distribusi data ekspresi gen
- Melakukan transformasi data RNA-seq
- Mengukur korelasi antar sampel
- Mengidentifikasi pola hubungan antar gen
- Membuat visualisasi network gen berbasis korelasi

---

## Tools dan Library

Project menggunakan bahasa R dengan package:

```r
library(readr)
library(dplyr)
library(tidyr)
library(tibble)
library(ggplot2)
library(corrplot)
library(pheatmap)
library(RColorBrewer)
```

---

## Struktur Analisis

### 1. Data Loading

Dataset RNA-seq dibaca menggunakan `read_csv()` kemudian dipilih hanya sampel jaringan liver.

```r
data_liver <- data[, grep("^liver_", colnames(data))]
```

---

## 2. Exploratory Data Analysis (EDA)

### Statistik Deskriptif

Parameter yang dihitung:

- Total counts
- Mean expression
- Median expression
- Variance
- Genes detected
- Detection rate

### Interpretasi

- Detection rate tinggi menunjukkan sebagian besar gen berhasil terdeteksi.
- Median expression membantu melihat distribusi data tanpa terlalu dipengaruhi outlier.
- Varians antar sampel membantu mengidentifikasi heterogenitas biologis.

---

## 3. Distribusi Data Sebelum dan Sesudah Transformasi

### Sebelum Transformasi

Visualisasi:
- Boxplot
- Histogram

### Interpretasi

Data RNA-seq mentah biasanya:

- Sangat skewed
- Banyak nilai nol
- Memiliki outlier ekstrem

Distribusi ini menyebabkan analisis statistik menjadi kurang stabil.

---

### Sesudah Log2 Transformation

Transformasi:

```r
log2(data_liver + 1)
```

### Interpretasi

Transformasi log2 bertujuan untuk:

- Mengurangi skewness
- Menstabilkan varians
- Membuat distribusi lebih mendekati normal
- Mempermudah analisis korelasi

Setelah transformasi:
- Boxplot menjadi lebih seimbang
- Distribusi histogram lebih homogen antar sampel

---

## 4. Analisis Korelasi Sampel

### Pearson Correlation

Digunakan untuk mengukur hubungan linear antar sampel.

```r
cor(data_filtered, method = "pearson")
```

### Interpretasi Pearson

- Nilai mendekati 1 menunjukkan pola ekspresi sangat mirip.
- Nilai rendah menunjukkan adanya perbedaan biologis atau teknis.
- Korelasi tinggi antar replikasi menunjukkan kualitas data baik.

---

### Spearman Correlation + Clustering

Digunakan untuk mengukur hubungan berbasis ranking.

```r
cor(data_filtered, method = "spearman")
```

### Interpretasi Spearman

- Lebih robust terhadap outlier.
- Clustering membantu melihat grup sampel yang memiliki pola ekspresi serupa.
- Sampel yang mengelompok kemungkinan memiliki kondisi biologis yang sama.

---

## 5. Filtering Gen

Gen dengan ekspresi rendah dihapus:

```r
gene_filter <- rowSums(data_liver > 10) >= 3
```

### Interpretasi

Filtering dilakukan untuk:

- Mengurangi noise
- Menghilangkan gen yang jarang diekspresikan
- Meningkatkan kualitas analisis korelasi

---

## 6. Gene Correlation Network

### Tahapan

1. Transformasi log2
2. Menghitung varians gen
3. Memilih top 15 gen dengan varians tertinggi
4. Menghitung korelasi antar gen
5. Membentuk network berdasarkan threshold korelasi

### Threshold

```r
threshold <- 0.75
```

Hanya korelasi kuat yang dipertahankan.

---

## Interpretasi Network

### Node

Merepresentasikan gen.

### Edge

Merepresentasikan hubungan korelasi antar gen.

- Biru → korelasi positif
- Merah → korelasi negatif

### Makna Biologis

- Gen yang terhubung kuat kemungkinan terlibat dalam jalur biologis yang sama.
- Korelasi positif menunjukkan gen cenderung aktif bersama.
- Korelasi negatif dapat menunjukkan mekanisme regulasi berlawanan.

### Top Variance Genes

Gen dengan varians tertinggi dipilih karena:

- Lebih informatif secara biologis
- Lebih mampu membedakan kondisi sampel
- Sering menjadi kandidat biomarker

---

## Output Visualisasi

Project menghasilkan:

- Boxplot distribusi ekspresi
- Histogram ekspresi gen
- Heatmap korelasi Pearson
- Heatmap clustering Spearman
- Network korelasi gen

---

## Dataset

GEO Accession:
GSE326527

Source:
NCBI GEO

---

## Potensi Pengembangan

Project ini dapat dikembangkan menjadi:

- Differential Expression Analysis (DESeq2)
- Gene Ontology Enrichment
- Pathway Analysis
- Weighted Gene Co-expression Network Analysis (WGCNA)
- Machine Learning untuk klasifikasi penyakit

---

## Author

Mario dan Raka
