# Lofi-version-converter
A Python-based Lofi Version Converter that transforms original audio into smooth, chill Lofi beats. Uses Librosa, Pydub, and Scipy.
## Lofi Version Converter Script
# -*- coding: utf-8 -*-
"""Lofi version converter.ipynb

Automatically generated by Colab.

Original file is located at
    https://colab.research.google.com/drive/1IuzGGdADuGLtw8yQtwLNKwBw0pVxaV9g
"""

!pip install librosa pydub ffmpeg numpy scipy

from google.colab import files

uploaded = files.upload()

!pip install soundfile

import librosa
import numpy as np
from scipy.signal import butter, lfilter
from pydub import AudioSegment

def load_audio(file_path):
    y, sr = librosa.load(file_path, sr=None)
    return y, sr

def apply_lofi_effects(y, sr):
    # Normalize audio
    y = librosa.util.normalize(y)

    def butter_lowpass_filter(data, cutoff, fs, order=4):
        nyquist = 0.5 * fs
        normal_cutoff = cutoff / nyquist
        b, a = butter(order, normal_cutoff, btype='low', analog=False)
        return lfilter(b, a, data)

    # Beat track detection
    tempo, _ = librosa.beat.beat_track(y=y, sr=sr)

    # Debug tempo and type-checking
    print("Detected Tempo:", tempo)

    # Handle invalid tempo by setting a safe default
    if not isinstance(tempo, (int, float)) or tempo <= 0:
        print("Invalid tempo detected. Skipping time stretching.")
        y_slow = y
    else:
        # Ensure valid stretching rate
        target_tempo = max(tempo * 0.8, 30)
        stretch_rate = max(tempo / target_tempo, 0.1)

        # Perform time stretch
        y_slow = librosa.effects.time_stretch(y.astype(np.float32), rate=stretch_rate)

    # Apply low-pass filter
    y_filtered = butter_lowpass_filter(y_slow, cutoff=4000, fs=sr)
    return y_filtered

def export_audio(y_filtered, sr, output_path):
    y_16bit = np.int16(y_filtered * 32767)
    audio_segment = AudioSegment(
        y_16bit.tobytes(), frame_rate=sr, sample_width=2, channels=1
    )
    audio_segment.export(output_path, format="mp3")
    print("Lofi version exported successfully!")

# Main code
input_path = "uploaded_audio.mp3"  # Replace with the actual filename after upload
y, sr = load_audio(input_path)
y_lofi = apply_lofi_effects(y, sr)
output_path = "lofi_version.mp3"
export_audio(y_lofi, sr, output_path)

from google.colab import files
files.download("lofi_version.mp3")
