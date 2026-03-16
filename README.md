# DIY-MOD: Personalized Content Moderation System

![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![Node.js](https://img.shields.io/badge/node-16+-green.svg)
![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC_BY--NC--SA_4.0-lightgrey.svg)
**Tags**: `content-moderation`, `llm`, `hci`, `personalized-moderation`, `browser-extension`

Copyright © 2026 The Regents of the University of Michigan
Social Computing Lab
Innovation Partnerships

This repository contains the official implementation for the paper:

**What If Moderation Didn't Mean Suppression? A Case for Personalized Content Transformation**  
*Rayhan Rashed and Farnaz Jahanbakhsh*  
📄 [Read the Paper](https://arxiv.org/abs/2509.22861) | 🌐 [Project Website](https://rayhan.io/diymod/)

## Overview

**DIY-MOD** is an end-to-end system that enables users to personalize their online content experience without platform-side censorship. Instead of simply blocking unwanted content, DIY-MOD transforms it using Large Language Models (LLMs)—applying interventions such as blurring, warning labels, or content rewriting based on user-defined natural language preferences.

## System Architecture

The system consists of two primary components:

1.  **Core DIY-MOD System**:
    *   **Backend**: A Python/FastAPI server that handles content processing, LLM interaction, and caching.
    *   **Browser Extension**: A Chrome extension that intercepts web content and applies real-time transformations.

2.  **Research Tools**:
    *   **Dual-Feed System** (Section 6): A controlled simulation used in our user studies. [Live Demo](https://diy-mod.vercel.app/) | [Readme](reddit-clone/README.md)

## Prerequisites

*   **Python**: 3.8+
*   **Node.js**: 16+
*   **Redis**: Required for task queue and caching.
*   **API Keys**: OpenAI (GPT-4o/GPT-4o-mini) and Google Gemini (for image processing).

## Quick Start

### 1. Backend Setup

The backend handles the heavy lifting of content analysis and transformation.

```bash
# 1. Clone the repository and navigate to Backend
cd Backend

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure Environment Variables
cp .env.template .env
# Open .env and add your OpenAI and Google API keys
```

### 2. Start Services

You will need to run the Redis server, Celery workers, and the API server simultaneously.

**Terminal 1: Redis**
```bash
redis-server
```

**Terminal 2: Celery Worker**
```bash
cd Backend
source venv/bin/activate
# Using gevent for concurrent task handling
celery -A celery_gevent_worker worker --loglevel=info -P gevent -c 1000
```

**Terminal 3: API Server**
```bash
cd Backend
source venv/bin/activate
python app.py
# Server will start at http://localhost:8001
```

### 3. Browser Extension Setup

```bash
cd BrowserExtension
npm install
npm run build
```

**To Load in Chrome:**
1.  Navigate to `chrome://extensions/`.
2.  Enable **Developer mode** (toggle in top-right).
3.  Click **Load unpacked**.
4.  Select the `BrowserExtension/dist` directory.

## Configuration

*   **Backend Config**: Main settings are in `Backend/config.yaml`.
*   **Environment Variables**: API keys and secrets are managed in `Backend/.env`.
*   **Extension Settings**: Configurable via the extension's popup interface.

## Research & User Study Tools [Study 2]

To replicate our study environment or test with the Reddit Clone:

### Reddit Clone Interface

This custom frontend interacts with the DIY-MOD backend to display transformed feeds side-by-side with original content.

```bash
cd reddit-clone

# Install dependencies
npm install

# Setup Environment
# Create .env.local if not present (see .env.production for reference keys)

# Start Development Server
npm run dev
# Access at http://localhost:3000
```

### Creating Custom Feeds

We provide scripts to generate and process custom feeds for testing:

```bash
cd Backend
# Create example feed data
python process_json_custom_feed.py --create-example

# Process the feed using the DIY-MOD pipeline
python process_json_custom_feed.py custom_feed_example.json --save --user demo-user --title "Test Feed"
```

## Community & Support

* **Community Page**: Join the conversation on our [GitHub Discussions](https://github.com/UMichHCI/diymod/discussions) page.
* **Contributing**: We welcome contributions! Please review our [Contributor Guidelines](CONTRIBUTING.md) and [Code of Conduct](CODE_OF_CONDUCT.md).
* **Support**: For UMich open source ecosystem support, please email [UMichOpenSourceSoftware@umich.edu](mailto:UMichOpenSourceSoftware@umich.edu).
* **Research Lab Context**: DIY-MOD was developed by researchers at the **Social Computing Lab** at the University of Michigan. 

## Citation

If you use this software or our research, please cite our CHI '26 paper and/or the repository. 

**Official Publication (CHI '26):**
```bibtex
@inproceedings{rashed2026diymod,
  author = {Rashed, Rayhan and Jahanbakhsh, Farnaz},
  title = {What If Moderation Didn't Mean Suppression? A Case for Personalized Content Transformation},
  year = {2026},
  isbn = {979-8-4007-2278-3/2026/04},
  publisher = {Association for Computing Machinery},
  url = {https://doi.org/10.1145/3772318.3790495},
  doi = {10.1145/3772318.3790495},
  booktitle = {Proceedings of the 2026 CHI Conference on Human Factors in Computing Systems},
  location = {Barcelona, Spain},
  series = {CHI '26}
}
```

**arXiv Preprint:**
```bibtex
@article{rashed2026diymod,
  title={What If Moderation Didn't Mean Suppression? A Case for Personalized Content Transformation},
  author={Rashed, Rayhan and Jahanbakhsh, Farnaz},
  journal={arXiv preprint arXiv:2509.22861},
  year={2026}
}
```

To cite the repository directly, use the Zenodo DOI:
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19042745.svg)](https://doi.org/10.5281/zenodo.19042745)

## License

Copyright © 2026 The Regents of the University of Michigan
Innovation Partnerships

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International license, CC BY-NC-SA 4.0. To view a copy of this license, visit https://creativecommons.org/licenses/by-nc-sa/4.0/ or see the [LICENSE](LICENSE) file.
