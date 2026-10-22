<div align="center">

# 🛰️ Classification of Multispectral & Thermal UAV Images with DeepLab v3

### Talk at the ArcGIS User Conference Greece – Cyprus 2026 · Athens

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![reveal.js](https://img.shields.io/badge/reveal.js-5.x-F2A93B)](https://revealjs.com/)
[![KaTeX](https://img.shields.io/badge/KaTeX-offline-329932)](https://katex.org/)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live%20Demo-181717?logo=github)](https://nickkoro21.github.io/arcgis-uc2026-uav-deeplab/)
[![Bilingual EN / ΕΛ](https://img.shields.io/badge/Bilingual-EN%20%2F%20%CE%95%CE%9B-2E86AB)](#controls)
[![ArcGIS User Conference 2026](https://img.shields.io/badge/ArcGIS%20User%20Conference-Greece%20%E2%80%93%20Cyprus%202026-2C7AC3)](#)

[**▶ Open the presentation**](https://nickkoro21.github.io/arcgis-uc2026-uav-deeplab/) ·
[**📊 Source code**](https://github.com/Nickkoro21/arcgis-uc2026-uav-deeplab) ·
[**📄 Slides (PDF, ΕΛ / EN)**](https://github.com/Nickkoro21/arcgis-uc2026-uav-deeplab/releases/tag/uc2026-slides) ·
[**🎓 University of the Aegean**](https://www.aegean.gr/)

</div>

---

**Classification of Multispectral & Thermal UAV Images with DeepLab v3**: an offline, self-contained [reveal.js](https://revealjs.com/) presentation for the **ArcGIS User Conference Greece – Cyprus 2026** (Συνάντηση Χρηστών ArcGIS Ελλάδας – Κύπρου 2026), Athens, 22 October 2026. It presents the work of the MSc thesis *Development of a GIS Application for the Automatic Classification of Multispectral & Thermal UAV Images Using Convolutional Neural Networks* (*Geography & Applied Geoinformatics*, **University of the Aegean**). Fully bilingual 🇬🇷 **ΕΛ** (default) / 🇬🇧 **EN**, switchable in place on any slide.

---

## About

The study trains the **same** segmentation network (DeepLabV3 + PointRend / ResNet-101) twice over UAV imagery of **Pamfila, Lesvos** (GSD 4.52 cm), and tests — rigorously — whether extra channels change the map:

- **7-Band composite** — Blue · Green · Red · RedEdge · NIR · **nDSM** (geometry) · **Thermal** (LWIR)
- **RGB baseline** — colour only

### Key finding

| Metric (M4, 2,100 paired points) | 7-Band | RGB | Δ |
|---|---|---|---|
| Overall Accuracy | **93.90%** | 83.29% | +10.61 |
| Cohen's κ | **92.89%** | 82.09% | +10.80 |
| Macro-F1 | **93.99%** | 83.71% | +10.28 |
| mIoU | **88.87%** | 73.78% | +15.09 |

The advantage is **structural** (McNemar *p* ≈ 8.6×10⁻⁴⁰; all bootstrap 95% CIs > 0) and is driven by **geometry (nDSM)** and **heat (LWIR)** — not by spectrum. After Holm correction, only **Building, Vehicle and Shadow-Noise** gain significantly, so the *multispectral* gain is **not** the vegetation bands — **H3 overturned**.

## Controls

| Action | Key |
|---|---|
| Next / previous | `→` `←` / `Space` |
| Switch language (EN ⇄ ΕΛ) | `l` (or the **EN / ΕΛ** button, top-right) |
| Slide menu / jump | `m` (or the **☰** button) |
| Overview | `Esc` |

The deck also has live, interactive widgets (animated McNemar table, paired-bootstrap simulation, a 3-tab confusion-matrix heat-map, and links to companion web apps).

## Built with

- **reveal.js 5.x** — vendored, no CDN, runs fully offline
- **KaTeX** — offline math rendering
- Vanilla JS for the language toggle, slide menu and animations (`deck/js/deck.js`)
- Headless **Puppeteer** tooling for deterministic rendering & an overflow/overlap audit (`deck/shoot.js`, `deck/audit.js`)

## Tools & methodology

The full toolchain behind the study and this presentation:

| Stage | Tools |
|---|---|
| **Data acquisition** | DJI **Matrice 350 RTK** UAV · **MicaSense Altum-PT** sensor (5 spectral bands + panchromatic + LWIR thermal) · RTK-fixed georeferencing (no GCPs) |
| **Photogrammetry** | **ArcGIS Drone2Map** — three independent SfM projects (multispectral · panchromatic · thermal) → orthomosaics, DSM, DTM |
| **GIS & data prep** | **ArcGIS Pro 3.6.2** — nDSM = DSM − DTM, 7-band composite stacking, polygon digitisation + topology, *Export Training Data* (Classified Tiles) |
| **Deep learning** | **arcgis.learn** / **PyTorch** — DeepLabV3 + **ResNet-101** backbone + **PointRend**, trained twice (7-band vs RGB), Focal + Dice loss with mixup |
| **Statistical evaluation** | Stratified paired sampling · **McNemar's** exact test · **Holm–Bonferroni** correction · **paired bootstrap** (10,000 resamples) · Cohen's κ — in Python |
| **Post-processing** | **PostProcessing Toolbox** (ArcGIS Python toolbox, GPLv3) — raster → structured vectors + per-class analysis · **Jeffries–Matusita** separability toolbox |
| **Delivery** | **GitHub Pages** · **Hugging Face Spaces** / **Gradio** · **Aegean SDI Portal** (`.dlpk` models) |
| **Presentation** | **reveal.js 5.x** (offline) · **KaTeX** · vanilla JS · headless **Puppeteer** (render/audit) · built & iteratively refined with **Claude Code** (Anthropic) |

## Run locally

```bash
cd deck
npm install        # only needed for the render/audit tooling
node server.js     # serves the deck at http://localhost:8000
```

Or just open `deck/index.html` in a browser — everything is self-contained.

## Author

**Nikolaos Koroniadis** — MSc *Geography & Applied Geoinformatics*, University of the Aegean · Mytilene, 2026

### Companion deliverables

- [ArcGIS StoryMap: Multi-spectral and Thermal Mapping with MicaSense Altum-PT](https://storymaps.arcgis.com/stories/0d764febde08486cad4290a65b4ecfcd)
- [Geospatial AI pipeline: walkthrough](https://nickkoro21.github.io/geospatial-ai-pipeline-walkthrough/)
- [Interactive methodology map](https://nickkoro21.github.io/lesvos-altum-segmentation/)
- [7-Band vs RGB dashboard](https://nickkoro21.github.io/thesis-7band-vs-rgb/)
- [PostProcessing Toolbox (ArcGIS, GPLv3)](https://github.com/Nickkoro21/PostProcessing-Toolbox)
- [Spectral Separability Explorer (JM, sensor-agnostic)](https://github.com/Nickkoro21/jm-separability-toolbox)
- [Spectral 3D Explorer](https://huggingface.co/spaces/NickKoro21/spectral-3d-explorer)

## Citation

If you refer to this work, please cite the MSc thesis. A machine-readable [`CITATION.cff`](CITATION.cff) is included, so you can use the **“Cite this repository”** button on GitHub.

> Koroniadis, N. (2026). *Development of a GIS Application for the Automatic Classification of Multispectral & Thermal UAV Images Using Convolutional Neural Networks* (MSc thesis). University of the Aegean, Department of Geography, Mytilene.

## License

Released under the [GNU General Public License v3.0](LICENSE) — © 2026 Nikolaos Koroniadis.
Vendored libraries ([reveal.js](https://revealjs.com/), [KaTeX](https://katex.org/)) retain their own MIT licences.
