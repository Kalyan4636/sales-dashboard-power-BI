# 📊 Sales Performance Dashboard — Power BI Backgrounds & Generator Scripts

A 4-page **Power BI dashboard background kit** generated programmatically from a real
sales dataset (2,707 orders · 20 SKUs · 5 categories · European retail, FY 2026),
plus the Python scripts used to build it and a narrated video reel for social sharing.

Each page is a self-contained **1280×720 PNG** designed to be dropped in as a Power BI
report page background — the card outlines line up with real KPI cards, charts and
lists so you can lay your own live visuals directly on top.

---

## 🖼️ Pages

| Page | Focus | File |
|---|---|---|
| **1 — Sales Performance Dashboard** | Revenue trend, top products, region mix, channel breakdown, headline KPIs | [`dashboard/page1_sales_performance.png`](dashboard/page1_sales_performance.png) |
| **2 — Product & Customer Insights** | Top countries, category performance, customer-type mix, quarterly revenue | [`dashboard/page2_product_customer_insights.png`](dashboard/page2_product_customer_insights.png) |
| **3 — Channel & Order Economics** | Average order value, revenue vs. cost, discount rate, order share by channel | [`dashboard/page3_channel_order_economics.png`](dashboard/page3_channel_order_economics.png) |
| **4 — Product Deep Dive** | Best/worst sellers, price vs. cost markup, revenue concentration, units by category | [`dashboard/page4_product_deep_dive.png`](dashboard/page4_product_deep_dive.png) |

An interactive preview of all four pages is in [`docs/dashboard_preview.html`](docs/dashboard_preview.html)
(open it directly in a browser).

## 🎥 Video

[`video/sales_dashboard_reel.mp4`](video/sales_dashboard_reel.mp4) — a ~61-second
Ken Burns–style walkthrough of all four pages with an original, royalty-free
synthesized background score. Built for LinkedIn / social posting.

## 📁 Data

[`data/Interactive_Excel_Sales_Dashboard.xlsx`](data/Interactive_Excel_Sales_Dashboard.xlsx)
is the source dataset: order-level sales records with region, country, channel,
customer type, product category, revenue, COGS, gross profit and margin.

## ⚙️ Scripts

All images and audio/video assets are generated from these scripts — nothing is
hand-drawn. Run them in order to reproduce (or tweak) the whole kit:

| Script | Produces |
|---|---|
| `scripts/01_build_page1_background.py` | Page 1 PNG |
| `scripts/02_build_page2_background.py` | Page 2 PNG |
| `scripts/03_build_page3_background.py` | Page 3 PNG |
| `scripts/04_build_page4_background.py` | Page 4 PNG |
| `scripts/05_generate_background_music.py` | Synthesized ambient background score (`.wav`) |
| `scripts/06_build_video_clips.py` | Per-page Ken Burns video clips (combine with `ffmpeg xfade` — see below) |

### Requirements

```bash
pip install -r requirements.txt
```

Video assembly also requires [ffmpeg](https://ffmpeg.org/) to be installed and on your `PATH`.

### Regenerating the dashboard images

```bash
python scripts/01_build_page1_background.py
python scripts/02_build_page2_background.py
python scripts/03_build_page3_background.py
python scripts/04_build_page4_background.py
```

### Regenerating the video

```bash
python scripts/05_generate_background_music.py
python scripts/06_build_video_clips.py

# then crossfade the 4 clips together with background music:
ffmpeg -y \
  -i clips/clip0.mp4 -i clips/clip1.mp4 -i clips/clip2.mp4 -i clips/clip3.mp4 \
  -i bgmusic.wav \
  -filter_complex "[0:v][1:v]xfade=transition=fade:duration=0.75:offset=15.0167[v01];[v01][2:v]xfade=transition=fade:duration=0.75:offset=30.0333[v012];[v012][3:v]xfade=transition=fade:duration=0.75:offset=45.05[vout];[4:a]atrim=0:60.8:0,afade=t=in:st=0:d=1.5,afade=t=out:st=59.2:d=1.6[aout]" \
  -map "[vout]" -map "[aout]" \
  -c:v libx264 -crf 18 -preset medium -pix_fmt yuv420p -c:a aac -b:a 192k \
  video/sales_dashboard_reel.mp4
```

## 🧩 Using the backgrounds in Power BI

1. In Power BI Desktop, go to **View → Page background**.
2. Insert the PNG for the page you're building.
3. Set **Image fit** to `Fit` and transparency to `0%`.
4. Add your live visuals on top of the matching white card outlines (the layout
   already reserves space for a chart, a KPI pair, a ranked list, and a donut/pie
   per page).

## 🛠️ Built with

- Python (Pillow, NumPy, SciPy, openpyxl)
- ffmpeg (video assembly, Ken Burns zoompan, crossfades, audio mix)

## 📄 License

MIT — see [LICENSE](LICENSE). The sample dataset is illustrative/synthetic and free
to reuse.
