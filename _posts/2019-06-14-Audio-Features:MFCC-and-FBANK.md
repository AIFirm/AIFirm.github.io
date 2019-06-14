---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.1.3
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

# Tips

### 1. 机器学习第一步是特征提取，语音领域也不例外。目前使用最多的是FBank（Filter banks）和MFCC，两者整体相似，MFCC多了一步DCT（离散余弦变换）。

### 2. 目前，用的多得是Fbank，因为fbank的信息比MFCC丰富（因为DCT只提取了频谱的包络信息，而损失了大量的声音细节），MFCC多了一步DCT，某种程度上是对语音信息的损变，而且因为多了一步，计算量更大。

### 3. FBank 适用于深度学习。因为，深度神经网络强大的特征提取能力使得我们只需要将信息更加丰富的Mel谱（或功率谱）信息直接送入神经网络进行训练，让神经网络提取更加鲁棒的特征。

### 4. MFCC 配合 GMM+HMM 声学模型比较合适。


# 本文是下面前2个具体实现，第3个实现见链接
1. Librosa 实现
2. python_speech_features 实现
3. [speech_process.ipynb](https://github.com/jamess010/AIOpen/blob/master/data/tools/librosa/mfcc-fbank/speech_process.ipynb)

```python
import numpy as np
import librosa
import random
import matplotlib.pyplot as plt
```

```python
def plot_spectrogram(spec, note):
    fig = plt.figure(figsize=(20, 5))
    heatmap = plt.pcolor(spec)
    fig.colorbar(mappable=heatmap)
    plt.xlabel('Time(s)')
    plt.ylabel(note)
    plt.tight_layout()

```

```python
def extract_fbank(y, sr, size=3, n_fft = 512, hop_length=.01, n_mels=40):
    """
    extract log mel spectrogram feature
    :param y: the input signal (audio time series)
    :param sr: sample rate of 'y'
    :param size: the length (seconds) of random crop from original audio, default as 3 seconds
    :param n_fft:
    :param hop_length: hop time in second, default 10ms
    :param n_nmels: 
    :return: log-mel spectrogram feature
    """
    # normalization
    y = y.astype(np.float32)

    # Pre-Emphasis
    pre_emphasis = 0.97
    emphasized_signal = np.append(y[0], y[1:] - pre_emphasis * y[:-1])
    
    signal_size = librosa.samples_to_time(len(emphasized_signal), sr)
    
    if(size < signal_size): 
        # random crop
        start = random.randint(0, len(y) - size * sr)
        #y = y[start: start + size * sr]
        emphasized_signal = emphasized_signal[0:int(size*sr)]

    # extract log mel spectrogram #####
    melspectrogram = librosa.feature.melspectrogram(y=emphasized_signal, sr=sr, n_fft=n_fft, hop_length=int(hop_length*sr), n_mels=n_mels)
    logmelspec = librosa.power_to_db(melspectrogram)

    return logmelspec
```

```python
def extract_mfcc(y, sr, size=3, n_fft = 512, hop_length=.01, n_mels=40, n_mfcc=20):
    """
    extract MFCC feature
    :param y: np.ndarray [shape=(n,)], real-valued the input signal (audio time series)
    :param sr: sample rate of 'y'
    :param size: the length (seconds) of random crop from original audio, default as 3 seconds
    :param size: the length (seconds) of random crop from original audio, default as 3 seconds
    :param n_fft:
    :param hop_length: hop time in second, default 10ms
    :param n_nmels:
    :param n_mfcc:   
    :return: MFCC feature
    """
    # normalization
    y = y.astype(np.float32)

    # Pre-Emphasis
    pre_emphasis = 0.97
    emphasized_signal = np.append(y[0], y[1:] - pre_emphasis * y[:-1])
    
    signal_size = librosa.samples_to_time(len(emphasized_signal), sr)
    
    if(size < signal_size): 
        # random crop
        start = random.randint(0, len(y) - size * sr)
        #y = y[start: start + size * sr]
        emphasized_signal = emphasized_signal[0:int(size*sr)]


    # extract log mel spectrogram #####
    melspectrogram = librosa.feature.melspectrogram(y=emphasized_signal, sr=sr, n_fft=n_fft, hop_length=int(hop_length*sr), n_mels=n_mels)
    mfcc = librosa.feature.mfcc(S=librosa.power_to_db(melspectrogram), n_mfcc=n_mfcc)
    mfcc_delta = librosa.feature.delta(mfcc)
    mfcc_delta_delta = librosa.feature.delta(mfcc_delta)
    mfcc_comb = np.concatenate([mfcc, mfcc_delta, mfcc_delta_delta], axis=0)

    return mfcc_comb
```

```python
audio_file = './OSR_us_000_0010_8k.wav'
```

```python
from IPython.display import Audio
Audio(audio_file)
```

```python
y, sr = librosa.load(audio_file, sr = None)
```

# 1. Librosa 实现

```python
y1 = y[:int(sr*1.0)]
fbank = extract_fbank(y=y1, sr=sr, size=3.5, n_fft=512, hop_length=.01, n_mels=40)
fbank.shape
```

```python
import librosa.display
fig = plt.figure(figsize=(20, 5))
librosa.display.specshow(fbank,y_axis='mel', x_axis='time', sr=sr,hop_length=80)
plt.colorbar()

```

```python
plot_spectrogram(fbank,'logmel')
```

```python
mfcc = extract_mfcc(y=y, sr=sr, size=3.5, n_fft=512, hop_length=.01, n_mels=40, n_mfcc=20)
mfcc.shape
```

```python
plot_spectrogram(mfcc,'logmel')
```

```python
y1 = y[:int(3.5*sr)]
mfccs = librosa.feature.mfcc(y=y1, sr=sr, n_mfcc=20, hop_length=80, n_fft=512, n_mels=40) 
mfccs.shape
```

```python
plot_spectrogram(mfccs,'logmel')
```

# 2. python_speech_features 实现

```python
from python_speech_features import base

y1 = y[:int(1.0*sr)]

```

```python
fbank = base.logfbank(signal=y1, samplerate=sr,nfilt=64,winfunc=np.hamming, winlen=.024, winstep=.01)
fbank.shape
```

```python
plot_spectrogram(fbank.T,'logfbank')
```

```python
mfcc = base.mfcc(signal=y1, samplerate=sr,nfilt=64,winfunc=np.hamming,numcep=20)
mfcc.shape
```

```python
plot_spectrogram(mfcc.T,'mfcc')
```

### Reference


1. 语谱图，滤波器组（Filter banks、MFCC）：
https://www.jianshu.com/p/b416d5617b0c

2. 声音识别教程，包括数据分析，特征提取，模型构建，模型训练和模型测试：
https://github.com/JasonZhang156/Sound-Recognition-Tutorial

3. ASR中常用的语音特征之FBank和MFCC（原理 + Python实现）：https://blog.csdn.net/Magical_Bubble/article/details/90295814#FBankFilter_Banks_228

4. 语音识别的前世今生：GMM+HMM & 深度学习：
http://makaidong.com/lyu0709/44493_689370_4.htm

5. python_speech_features：
https://github.com/jameslyons/python_speech_features
