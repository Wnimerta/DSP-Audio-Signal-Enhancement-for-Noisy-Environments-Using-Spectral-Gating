# 🎧 Audio Signal Enhancement for Noisy Environments Using Spectral Gating

A Python-based digital signal processing (DSP) system to enhance speech clarity in noisy recordings using **spectral gating**. This project leverages **Librosa** and **scientific visualization tools** to denoise audio signals with practical insights into frequency-domain processing.

---

## 🎓 Project Info

* **University**: Sukkur IBA University
* **Department**: Computer Systems Engineering
* **Project Title**: Audio Signal Enhancement for Noisy Environments Using Spectral Gating
* **Submitted By**: Nimerta Wadhwani 
* **Supervisor**: Dr. Junaid Ahmed Bhatti

---

## 🌎 Abstract

This project implements a spectral gating algorithm to enhance audio clarity in recordings distorted by background noise (e.g., chatter, air conditioning, projector hum). It uses a 5-second noise profile, applies a frequency-dependent threshold, and reconstructs cleaner audio. Implemented in Python with **Librosa**, **NumPy**, **Matplotlib**, **Seaborn**, and **SciPy**, the system produces effective denoising while maintaining intelligibility.

---

## 🚀 Objectives

* Reduce stationary/semi-stationary background noise.
* Preserve the speaker's voice and intelligibility.
* Visualize signal enhancement stages.
* Explore frequency-domain DSP techniques.

---

## 🔧 Tools & Libraries

| Tool/Library       | Purpose                           |
| ------------------ | --------------------------------- |
| Python 3.9         | Programming language              |
| Librosa            | Audio processing and STFT/ISTFT   |
| NumPy              | Numerical computation             |
| SciPy              | Median filtering                  |
| Matplotlib/Seaborn | Signal and spectrum visualization |
| IPython Display    | Inline audio playback in notebook |

---

## 🔍 Methodology

### 1. Noise Characterization

* 5-second segment assumed to contain only noise.
* Analyzed via:

  * **Time-Domain Plot**
  * **FFT Spectrum** (up to 1 kHz)
  * **Spectrogram** (STFT visualization)

### 2. Algorithm Selection

Evaluated:

* Adaptive Filtering ❌ (needs reference)
* Wiener Filtering ❌ (requires precise PSD)
* **Spectral Gating** ✅ (chosen for simplicity & effectiveness)

### 3. Spectral Gating Steps

1. Compute STFT of signal & noise sample
2. Calculate frequency-dependent threshold:

   ```python
   threshold = mean + std_dev * 3
   ```
3. Apply binary mask: suppress bins below threshold
4. ISTFT to reconstruct audio
5. Median filtering to smooth result

---

## 🔢 Code Overview

```python
def spectral_gate(y, sr, noise_sample, n_std=3.0, n_fft=2048, hop=512):
    stft_noise = librosa.stft(noise_sample, n_fft=n_fft, hop_length=hop)
    stft_signal = librosa.stft(y, n_fft=n_fft, hop_length=hop)
    noise_mag = np.abs(stft_noise)
    threshold = np.mean(noise_mag, axis=1) + n_std * np.std(noise_mag, axis=1)
    mask = (np.abs(stft_signal) > threshold[:, None]).astype(float)
    y_clean = librosa.istft(stft_signal * mask, hop_length=hop)
    y_clean = medfilt(y_clean, kernel_size=3)
    return y_clean, mask, threshold
```

---

## 🔄 Performance Evaluation

### ✅ Visualizations

* Noise Profile (Time & Frequency)
* Frequency-Dependent Threshold Curve
* Binary Time-Frequency Mask
* Waveform Comparison
* Difference Spectrogram

### 🗣️ Subjective Listening

* Average rating: **4.5/5** from 5 listeners
* Minor high-frequency artifacts noted (\~0:21)

---

## 🔍 Results

* **Significant noise reduction** in low & mid-frequency bands
* **Preserved speech intelligibility**
* **Visual clarity** in waveform & spectrogram

---

## 🤔 Trade-offs & Limitations

| Trade-Off                  | Observation                             |
| -------------------------- | --------------------------------------- |
| Threshold vs Distortion    | Higher threshold = risk of speech loss  |
| FFT Size vs Computation    | 2048 = good resolution, heavier compute |
| Real-time Capability       | Not optimized for live processing       |
| Static Noise Profile       | Dynamic noises less effectively removed |
| No Objective Metric (PESQ) | Suggested for future version            |

---

## 🚀 Future Scope

* Adaptive Noise Estimation (non-stationary handling)
* Soft Thresholding (reduce artifacts)
* Real-Time Processing (with Numba or PyAudio)
* Integration of PESQ or MOS
* Hybrid Denoising (e.g., ML + DSP)

---

## 📆 Implementation Details

* Audio File: Mono MP3, sampled at 16 kHz
* Noise Sample: First 5 seconds
* FFT Size: 2048
* Hop Length: 512
* Window: Hanning

---

## 📖 References

1. Boll, S. F. (1979). *Spectral Subtraction for Noise Suppression*.
2. Loizou, P. C. (2013). *Speech Enhancement: Theory and Practice*.
3. Goodwin & Vetterli (2008). *Time-Frequency DSP for Noise Reduction*.
4. McFee et al. (2015). *Librosa Toolkit*.
5. Skodras et al. (2001). *JPEG2000 & Signal Processing*.

---

> ✨ *Enhancing audio one frequency bin at a time!*
