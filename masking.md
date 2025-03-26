# Masking

* Masking is a way to remove or hide unwanted pixels in an image so that they are not considered in further analysis. 
* What Does "Masked" Mean?
  * A masked image in Google Earth Engine (GEE) is an image where some pixels are transparent (ignored) and others remain visible.

In GEE, masking is done using binary values:
* **1 (valid pixels)** → These pixels remain visible and will be used in calculations.
* **0 (masked pixels)** → These pixels become transparent and are ignored in further processing.

# Purpose of Masking in Your Code
1. Focusing on Relevant Areas:
- The mask ensures that only pixels with B4 ≥ 0.3 are included in the area calculation.
- This could be used, for example, to identify areas with vegetation, water, or specific land cover types.

2. Accurate Area Calculation:
- Without masking, all pixels (including irrelevant ones) would be included in the area computation.
- The mask ensures that only pixels meeting the B4 ≥ 0.3 condition contribute to the total area.

3. Efficient Processing:
- Masked pixels are ignored in calculations, reducing computational load and improving efficiency.

# Example: How Masking Works Visually

Let's say we have an image where `B4` values look like this:

| Pixel Position | B4 Value | Mask Applied (B4 ≥ 0.3) |
|---------------|----------|-------------------------|
| (1,1)        | 0.5      | ✅ (Keep)               |
| (1,2)        | 0.2      | ❌ (Mask)               |
| (2,1)        | 0.4      | ✅ (Keep)               |
| (2,2)        | 0.1      | ❌ (Mask)               |

After applying the mask, pixels with `B4 < 0.3` will be **ignored (transparent)**, while others remain for further analysis.

```markdown
# Klasifikasi Tutupan Lahan Berdasarkan NDVI

Kode ini digunakan untuk **masking** atau pemfilteran area berdasarkan nilai **NDVI (Normalized Difference Vegetation Index)** guna mengklasifikasikan tutupan lahan berdasarkan vegetasi.

## 1. Masking Area Sabana (NDVI antara 0.2 - 0.5)
```python
sabana = ndvi.gt(0.2).And(ndvi.lt(0.5)).selfMask()

- `ndvi.gt(0.2)`: Memilih piksel dengan **NDVI lebih besar dari 0.2** (vegetasi ringan).
- `ndvi.lt(0.5)`: Memilih piksel dengan **NDVI kurang dari 0.5** (bukan hutan lebat).
- `.And(...)`: Menggabungkan kedua kondisi sehingga hanya piksel dengan **NDVI antara 0.2 dan 0.5** yang dipilih.
- `.selfMask()`: Menghapus semua piksel yang **tidak memenuhi kriteria**, sehingga hanya area sabana yang tersisa.

## 2. Masking Area Hutan (NDVI > 0.5)
```python
hutan = ndvi.gte(0.5).selfMask()

- `ndvi.gte(0.5)`: Memilih semua piksel dengan **NDVI lebih besar atau sama dengan 0.5**.
- `.selfMask()`: Menghapus semua piksel yang **tidak memenuhi kriteria**, sehingga hanya area dengan NDVI tinggi yang tersisa.

## 3. Masking Lahan Terbuka (NDVI < 0.2)
```python
lahan_terbuka = ndvi.lt(0.2).selfMask()

- `ndvi.lt(0.2)`: Memilih semua piksel dengan **NDVI kurang dari 0.2**, yang biasanya merupakan lahan tanpa vegetasi.
- `.selfMask()`: Menghapus semua piksel yang **tidak memenuhi kriteria**, sehingga hanya lahan terbuka yang tersisa.
