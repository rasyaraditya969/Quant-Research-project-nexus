<div align="center">

# 🧭 NEXUS-QRM

### Quantitative Regime & Market Intelligence — mesin riset kuantitatif siap pakai di Colab

*Rezim makro • Rotasi sektor • Sinyal lintas aset • Walk-forward testing • Sentimen berita*

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/nexus-qrm/blob/main/notebooks/NEXUS_QRM.ipynb)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Google-Colab-F9AB00?logo=googlecolab&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue)

[🇬🇧 English](README.md) · **🇮🇩 Bahasa Indonesia**

</div>

---

## 📌 Gambaran Umum

**NEXUS-QRM** adalah kerangka riset dalam satu notebook yang mengubah data pasar mentah menjadi
gambaran terstruktur tentang kondisi makro, lalu menguji aturan trading yang transparan di atasnya.

Berjalan penuh di Google Colab **tanpa API key**: install, klik *Run all*, baca laporannya.

> ⚠️ **Ini adalah framework riset dan backtesting, bukan sistem trading dan bukan saran investasi.**

## ✨ Fitur

- 🌍 **Universe aset:** indeks (SPX, NDX, DJI, RUT), VIX, proksi suku bunga / USD, komoditas (Emas, Perak, Minyak, Tembaga), dan 11 ETF sektor SPDR
- 📈 **Feature engineering:** momentum 20D, volatilitas realisasi, z-score VIX, momentum lintas aset, breadth sektor
- 🧠 **Klasifikasi rezim:** *Growth Score* → `Expansion / Risk-On`, `Transition`, `Contraction / Risk-Off`
- 🔄 **Rotasi sektor:** peringkat komposit 1M / 3M / 6M
- 🔗 **Korelasi lintas aset:** matriks korelasi rolling 60 hari + heatmap
- ⚙️ **Signal engine:** aturan long/short yang mudah diaudit
- 💸 **Analisis biaya:** turnover dan biaya transaksi eksplisit (gross vs net)
- 🎛️ **Optimasi parameter** hanya pada data training
- 🚶 **Walk-forward** (train 756 hari / test 126 hari) dan 🧪 **holdout OOS 20%**
- 🎲 **Monte Carlo bootstrap** 2.000 jalur
- 📰 **News engine:** Google News RSS + sentimen VADER
- ⚡ **Event study:** profil return SPX di sekitar event (CPI, FOMC, NFP)
- 📝 **Laporan riset** otomatis ala Bloomberg dalam satu cell

## 🧮 Metodologi

```text
Growth_Score = z(SPX_Mom_20D) − z(DXY_Mom_20D) − z(SPX_Vol_20D) + z(Sector_Breadth)

  Growth_Score >  0.75  →  Expansion / Risk-On
  Growth_Score < -0.75  →  Contraction / Risk-Off
  selain itu            →  Transition
```

- **Long:** momentum 20D > 0, `Growth_Score` > 0, dan volatilitas di bawah median 252 hari
- **Short:** momentum 20D < 0 dan `Growth_Score` < 0
- Sinyal **digeser 1 hari** agar tidak memakai harga close hari ini untuk bertransaksi di hari yang sama
- Parameter dipilih di data training; 20% data terakhir tidak pernah disentuh

## 📊 Contoh Hasil

SPX, **2018-01-02 → 2026-09-25**, parameter default, sebelum biaya:

| Metrik | Strategi (full sample) | Holdout OOS |
|---|---:|---:|
| Total return | -15,60% | 0,78% |
| CAGR | -2,03% | 0,45% |
| Volatilitas tahunan | 16,40% | 12,58% |
| Sharpe | -0,04 | 0,10 |
| Max drawdown | -44,20% | -13,34% |

> 💡 Aturan contoh di notebook ini **tidak mengalahkan buy-and-hold**, dan memang itu tujuannya:
> notebook ini adalah *alat uji yang bersih dari kebocoran data*, untuk menunjukkan dengan cepat
> kapan sebuah ide **tidak** bekerja. Silakan ganti dengan model alpha Anda sendiri.

## 🚀 Cara Pakai

**Google Colab (disarankan):** klik badge **Open in Colab** → `Runtime → Run all`.

**Lokal:**

```bash
git clone https://github.com/YOUR_USERNAME/nexus-qrm.git
cd nexus-qrm
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/NEXUS_QRM.ipynb
```

## 🛣️ Roadmap

- [ ] Database event FOMC / CPI / NFP / PCE / ISM beserta surprise vs konsensus
- [ ] Simbol futures dan penanganan roll kontrak
- [ ] Model slippage dan komisi per instrumen
- [ ] Volatility targeting, position sizing, risk parity
- [ ] Validasi time-series purged / embargoed
- [ ] Atribusi faktor, VaR / CVaR portofolio

## ⚠️ Disclaimer

Proyek ini hanya untuk **tujuan edukasi dan riset**. Bukan saran investasi atau ajakan membeli / menjual
instrumen apa pun. Hasil backtest bersifat hipotetis dan tidak menjamin kinerja di masa depan.
Gunakan dengan risiko Anda sendiri.

## 📄 Lisensi

[MIT License](LICENSE)
