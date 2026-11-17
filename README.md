# LLM Pipeline Explorer

An interactive, single-file browser application that demonstrates how raw text becomes discrete tokens and transforms into geometric vectors in a 3D semantic embedding space.

Live local development: [http://localhost:8000](http://localhost:8000)

---

## Overview

Modern Large Language Models (LLMs) operate through two foundational transformations:
1. **Discrete Tokenization**: Converting unstructured human text into discrete integer token IDs using a subword vocabulary.
2. **Dense Vector Embeddings & Geometry**: Mapping sequences into high-dimensional geometric spaces (e.g. 384 dimensions) where semantic similarity corresponds to spatial proximity (cosine similarity), and projecting those relationships to 3D for intuitive exploration.

This explorer lets you interact directly with both stages in real time—all within the browser without any server-side dependencies.

---

## Features

### 1. Interactive Tokenizer (Subword Vocabulary & Fragmentation)
- **Training Corpus**: Learns token frequencies from an editable set of sentences.
- **Punctuation & Contractions**: Keeps punctuation separate while sensibly preserving contractions like `"don't"`.
- **Dynamic Vocabulary Slider**: Adjust vocabulary size from 1 up to the total number of unique words in the corpus.
- **Greedy Longest-Match Subword Tokenization**: Words outside the active vocabulary are broken down into subword pieces (`##s`, `##ed`, `##ing`, etc.) and single-character pieces.
- **Live Token Expansion Metric**: Watch the token count grow in real time as the vocabulary shrinks (demonstrating why smaller vocabularies result in longer token sequences).
- **No `[UNK]` Failures**: Full alphabet fallback ensures standard text decomposes gracefully.

### 2. Embeddings & 3D Vector Geometry
- **In-Browser Sentence Embeddings**: Uses **[Transformers.js](https://huggingface.co/docs/transformers.js)** to run the quantized `Xenova/all-MiniLM-L6-v2` ONNX model directly in WebAssembly/WebGPU within your browser (384-dimensional dense vectors with mean pooling and L2 normalization).
- **Plain JavaScript PCA (Principal Component Analysis)**:
  - Centers the 384-dimensional embeddings.
  - Computes the sample covariance matrix and extracts the top 3 orthonormal principal components ($PC1, PC2, PC3$) via Gram-Schmidt orthogonalized power iteration.
- **Interactive 3D Scatter Plot (Plotly.js)**:
  - Rotate, zoom, pan, and hover over points to inspect sentences and their spatial coordinates.
  - Automatically color-codes semantic clusters using $k$-means in vector space (e.g. feline sentences, Python programming, and financial crash sentences cluster naturally).
  - Displays the Top 5 most semantically similar sentence pairs and their cosine similarities.
- **"Try Your Own Word or Sentence" Playground**:
  - Tokenizes your input using the active tokenizer vocabulary.
  - Computes its 384-dimensional MiniLM embedding.
  - **Projects onto the existing PCA basis without refitting**, adding your input as a distinct crimson diamond (`★`) marker on the 3D plot.
  - Identifies the top 3 closest sentences in the corpus with exact cosine similarity scores.
- **Offline Fallback**: Includes a built-in semantic character $n$-gram embedding fallback if network or CDN access is unavailable.

---

## Design System

Designed with a warm, human-crafted research studio aesthetic:
- **Crème Canvas (`#f7f4ed` / `#fffdf9`)**: Soft paper tones with subtle stone borders.
- **Botanical Sage Green (`#526855`)**: Primary buttons, badges, slider accents, and typography.
- **Terracotta Crimson Red (`#b92b27`)**: Call-to-action buttons, subword continuation marks, custom 3D points, and highlights.

---

## Getting Started

Because the application is completely self-contained in `index.html` with external libraries loaded via CDN, there is **zero build step** and **no package manager** required.

### Option A: Local Python Server (Recommended)
```bash
python3 -m http.server 8000
```
Then visit [http://localhost:8000](http://localhost:8000) in your browser.

### Option B: Open Directly
Double-click `index.html` to open it directly in Chrome, Safari, Firefox, or Edge.

---

## Technical Stack

- **HTML5 / CSS3 / Vanilla JavaScript**: Single self-contained file (`index.html`)
- **[Plotly.js](https://plot.ly/javascript/)**: 3D scatter visualization via CDN
- **[Transformers.js](https://github.com/xenova/transformers.js)**: In-browser inference for `Xenova/all-MiniLM-L6-v2`
- **Linear Algebra / PCA**: Pure JavaScript implementation of sample covariance and orthogonalized power iteration

---

## License

MIT License. Feel free to explore, learn, and adapt!
