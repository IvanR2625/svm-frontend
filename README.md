# SVM for MDD and Gut

A static website for a **CYSF 2025–2026 science fair project** that uses a Support Vector Machine (SVM) to predict **Major Depressive Disorder (MDD)** from gut-microbiome composition.

The site explains the research (methods, results, limitations) and has an interactive **Try it Now** page. On that page you enter bacterial abundance values, and a hosted backend model returns an *MDD* or *Control* prediction.

**Authors:** Ivan Raizada, Matthew Sun

> ⚠️ This tool is for educational and research demonstration purposes only. It is not intended for clinical use or medical diagnosis.

---

## Pages

| Page | File | Contents |
| --- | --- | --- |
| Home | `index.html` | Project overview, feature highlights, and links to the other pages |
| Try it Now | `svm_try.html` | Interactive prediction form that calls the backend API |
| Methodology | `svm_methods.html` | The 8-phase pipeline, from SRA download to deployment, with Chart.js charts |
| Results | `svm_results.html` | Sequencing QC, alpha/beta diversity, PERMANOVA, and differentially abundant genera |
| Limitations | `svm_limits.html` | Study limitations, key considerations, and mitigation strategies |

---

## Repository structure

```
svm-frontend/
├── index.html          # Home page
├── svm_try.html        # Interactive model demo (calls the backend)
├── svm_methods.html    # Methodology
├── svm_results.html    # Results
├── svm_limits.html     # Limitations
├── styles.css          # Shared stylesheet for all pages
├── images/             # Backgrounds, icons, placeholder images
└── info/               # Design and content notes (CSS, colour gradients, data references)
```

The site is plain HTML, CSS, and vanilla JavaScript, with no build step or framework. It loads these from CDNs:

- [Google Fonts](https://fonts.google.com/): Lato and Nunito
- [Chart.js](https://www.chartjs.org/), on the Methodology page only

---

## Research summary

As described on the site:

- **Data:** 205 paired-end 16S rRNA stool samples (106 healthy controls, 99 MDD), downloaded from NCBI SRA with the SRA Toolkit.
- **Processing:** QIIME 2 (v2025.7). DADA2 denoising and chimera removal kept about 13.05M reads, with a median of 63,356 reads per sample.
- **Taxonomy:** a SILVA 138 Naive Bayes classifier assigned reads to 344 genera.
- **Statistics:**
  - Shannon alpha diversity, Kruskal–Wallis test: H = 30.343, p = 3.62 × 10⁻⁸
  - Bray–Curtis beta diversity, PERMANOVA: pseudo-F = 17.36, p = 0.001
  - 30+ genera differed significantly (Mann–Whitney U with Benjamini–Hochberg correction), including *Bacteroides*, *Faecalibacterium*, *Parabacteroides*, *Blautia*, and *Alistipes*.
- **Model:**
  - RBF-kernel SVM on CLR-transformed, standardized genus abundances
  - Hyperparameters chosen by grid search over C and γ
  - SMOTE used to balance the classes
  - Evaluated with 5-fold stratified cross-validation and a 20% hold-out test set

---

## Running locally

The site is static, so you can open `index.html` directly in a browser. If you want a local web server instead, run one of these from the repository root:

```bash
# Python
python -m http.server 8000

# or Node
npx serve .
```

Then open <http://localhost:8000>.

The **Try it Now** page needs an internet connection, because it sends requests to the hosted backend.

---

## Using the "Try it Now" page

1. In **Read Counts**, enter relative abundance values separated by semicolons, for example `0.4; 0.32; 0.04`.
2. Under **Select Genera**, tick one genus for each value. The search box filters the list.
3. Click **Get Prediction**.

You must select exactly as many genera as you entered values, or the page shows a mismatch error. **Clear Form** resets all inputs.

### Backend API

The page sends a `POST` request to the backend URL set at the top of the `<script>` block in `svm_try.html`:

```js
const backendURL = "https://svm-backend-eu5s.onrender.com/predict";
```

**Request** (`Content-Type: application/json`):

```json
{
  "reads": [0.4, 0.32, 0.04],
  "genera": ["Blautia", "Bacteroides", "Faecalibacterium"]
}
```

**Expected response** (example values):

```json
{
  "prediction_label": 1,
  "prediction_probability": 0.87
}
```

- `prediction_label`: a truthy value means **MDD**; a falsy value means **Control**.
- `prediction_probability`: shown on the page as a percentage.

The backend is hosted on Render, so the first request after a period of inactivity may be slow while the service starts up.

To use a different backend, such as a local copy, change `backendURL` in `svm_try.html`.

---

## Deployment

The site is static, so any static host works, for example GitHub Pages, Netlify, Vercel, or Render Static Sites. Deploy the repository root as-is; there is no build command.

If the backend runs on a different domain from the frontend, it must allow cross-origin (CORS) requests from the frontend's domain.

---

## Known issues

- The Try it Now page says there are **341** selectable genera, and the methodology says **344** were identified. The `bacteriaList` array in `svm_try.html` actually contains **269** entries.
- Several pages still use `images/placeholder.jpg` instead of real figures.

---

## Disclaimer

This project is a student science fair project. Its predictions are not medically validated and must not be used to diagnose or treat any condition. If you have concerns about your mental health, talk to a qualified healthcare professional.
