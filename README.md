# Radar Beam Blockage — Preliminary Geometry Assessment

Analisis geometri awal potensi beam blockage oleh bangunan tinggi terhadap radar cuaca C-Band Tangerang (EEC DWSR-2501C).

## Ringkasan

Notebook ini menghitung sudut halangan (obstruction angle) bangunan terhadap radar, membandingkannya dengan amplop beam pada 9 elevasi scan VCP-style, dan mengklasifikasikan potensi blockage menjadi **Clear**, **Partial**, atau **Severe**.

**Bukan** DEM-based partial beam blockage, bukan validasi operasional.

## Struktur Proyek

```
├── preliminary_geometry_blockage_notebook.ipynb   # Notebook utama
├── data/
│   └── Radar_Building_Distance_Measurement_Layout.png
└── outputs/                                       
    ├── figures/
    │   ├── fig1_measurement_layout.png
    │   ├── fig2_side_view_obstruction.png
    │   ├── fig3_classification_heatmap.png
    │   ├── fig4_obstruction_angle_vs_distance.png
    │   └── fig5_polar_sector.png
    └── tables/
        └── blockage_classification.csv
```

## Cara Menjalankan

```bash
# 1. Buat virtual environment
python -m venv .venv

# 2. Aktivasi
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # Linux/Mac

# 3. Install dependencies
pip install numpy pandas matplotlib jupyter

# 4. Jalankan notebook
jupyter notebook preliminary_geometry_blockage_notebook.ipynb
```

## Parameter Kunci

| Parameter | Nilai | Sumber |
|-----------|-------|--------|
| Tinggi radar (radome top) | 21 m | Operator |
| Tinggi bangunan | 39 m | WIKA Gedung |
| Beamwidth | 0.95° | EEC datasheet |
| Jarak terdekat (D_min) | 44.6 m | Google Earth Pro |
| Jarak representatif (D_center) | 96.5 m | Google Earth Pro |
| Jarak terjauh (D_max) | 154.0 m | Google Earth Pro |

## Output

- **5 figure PNG** (300 DPI) — siap untuk paper IEEE-style
- **1 CSV** — tabel klasifikasi blockage per skenario jarak × elevasi

## Metode

1. **Obstruction angle:** α = arctan((H_building − H_radar) / distance)
2. **Beam envelope:** θ ± β/2
3. **Klasifikasi:**
   - α < θ_lower → Clear
   - θ_lower ≤ α ≤ θ_upper → Partial
   - α > θ_upper → Severe

## Batasan

- Bukan fraksi partial beam blockage (wradlib-style)
- Tidak menggunakan DEM/DSM
- Tinggi radar adalah top-of-radome, bukan antenna phase center
- Elevasi scan adalah referensi VCP 21-style, bukan konfirmasi operasional Tangerang