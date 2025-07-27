# 🎧 Lofi Version Converter

A Python-based audio converter that transforms regular audio files into smooth, chill Lofi beats.  
Built using **Librosa**, **PyDub**, and **Scipy**.

---

## 📌 Features

- Applies low-pass filter to smoothen audio.
- Reduces tempo slightly for that Lofi vibe.
- Normalizes volume levels.
- Converts and exports audio to `.mp3` format.

---

##  Run via Google Colab

You can also try it directly in your browser using this Google Colab Notebook:  
[🔗 Open in Colab](https://colab.research.google.com/drive/1IuarGsGAdDuGtx8yCthtNXwBoDpvvaVg)

---

##  Installation

First, clone the repository and install the dependencies:

```bash
git clone https://github.com/Gurukiran20/Lofi-version-converter.git
cd Lofi-version-converter
pip install -r requirements.txt
```bash
---

## 🚀 Usage
Replace input_audio.mp3 with your actual file:
python lofi_converter.py --input input_audio.mp3 --output lofi_version.mp3

 Example:
python lofi_converter.py --input song.mp3 --output song_lofi.mp3

---

## Requirements:
Create a requirements.txt file:
Make sure these Python packages are installed:

librosa
pydub
scipy
numpy


