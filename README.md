# Ex: 03 - Pulse-Code-Modulation and Delta Modulation
## AIM:
Write a simple Python program for the modulation and demodulation of PCM, and DM.

## TOOLS REQUIRED:
Python IDE Numpy and Scipy.

## PROGRAM:

#### PULSE CODE MODULATION:

```
#PCM

import numpy as np
import matplotlib.pyplot as plt

# Parameters
fs, fm, T, L = 5000, 50, 0.1, 16
t = np.arange(0, T, 1/fs)

# Message signal
m = np.sin(2*np.pi*fm*t)

# Quantization (PCM)
step = (m.max() - m.min()) / L
q = np.round(m / step) * step

# PCM encoding (digital levels)
pcm = ((q - q.min()) / step).astype(int)

# Plot
plt.figure(figsize=(10,9))
plt.suptitle("NAME : RAKSHAYA R\nREG NO : 212224060204",
             fontsize=12, fontweight='bold')

plt.subplot(4,1,1)
plt.plot(t, m)
plt.title("Message Signal (Analog)")
plt.grid(True)

plt.subplot(4,1,2)
plt.step(t, q, where='mid')
plt.title("Quantized Signal")
plt.grid(True)

plt.subplot(4,1,3)
plt.stem(t[:50], pcm[:50], basefmt=" ")
plt.title("PCM Encoded Signal (Digital Levels)")
plt.grid(True)

plt.subplot(4,1,4)
plt.plot(t, q, 'r--')
plt.title("PCM Demodulated Signal")
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()
```
#### DELTA MODULATION:
```
#Delta Modulation

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

# Parameters
fs, fm, T, delta = 10000, 10, 1, 0.1
t = np.arange(0, T, 1/fs)

# Message signal
m = np.sin(2*np.pi*fm*t)

# Delta Modulation (Encoder)
dm = np.zeros_like(m)
bits = np.zeros_like(m)
for i in range(1, len(m)):
    if m[i] > dm[i-1]:
        bits[i] = 1
        dm[i] = dm[i-1] + delta
    else:
        dm[i] = dm[i-1] - delta

# Delta Demodulation
rec = np.cumsum((2*bits - 1) * delta)

# Low-pass filter
b, a = butter(4, 20/(0.5*fs), 'low')
rec_filt = filtfilt(b, a, rec)

# Plot
plt.figure(figsize=(10,8))
plt.suptitle("NAME : KRITHI.V\nREG NO : 212224060128",
             fontsize=12, fontweight='bold')

plt.subplot(3,1,1)
plt.plot(t, m)
plt.title("Original Signal")
plt.grid(True)

plt.subplot(3,1,2)
plt.step(t, dm, where='mid')
plt.title("Delta Modulated Signal")
plt.grid(True)

plt.subplot(3,1,3)
plt.plot(t, rec_filt, 'r--')
plt.title("Demodulated Signal")
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

```
## OUTPUT WAVEFORM:
#### PULSE CODE MODULATION:
<img width="976" height="887" alt="ep 3 dc" src="https://github.com/user-attachments/assets/ef125b9e-a54a-4cf3-8566-49fa8db2bd8e" />


#### DELTA MODULATION:
<img width="989" height="789" alt="ep 3 dc 2" src="https://github.com/user-attachments/assets/12946ca6-7286-4032-9abe-bad1a24e4dad" />


## RESULT:
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles.
