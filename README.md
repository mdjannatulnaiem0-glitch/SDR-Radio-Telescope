 SDR-Radio-Telescope by yagi antenna 
 
Building an open-source Radio Telescope with RTL-SDR and Python for cosmic signal observation.

SDR-Based DIY Radio Telescope

this is created IPC rules 

Hello! I am Naiem and an aspiring Electronic Engineering student. This is my open-source project for building a DIY Radio Telescope using an RTL-SDR receiver to observe cosmic radio signals (like Solar Transits or the 21cm Hydrogen Line).

## System Block Diagram

Below is the architectural flow of the radio telescope system:

##  Components Used

### Hardware:
1. **RTL-SDR Blog V3/V4** USB Dongle.
2. **2.4GHz WiFi Grid Dish** (Modified for 1.42 GHz) or a **Horn Antenna**.
3. **LNA (Low Noise Amplifier)** with a Bandpass Filter (e.g., Nooelec SAWbird+ H1).
4. **PC / Laptop** (Linux/Windows).

### Software:
- **Python 3.x**
- **pyrtlsdr** library (for interfacing SDR with Python)
- **matplotlib** & **numpy** (for data visualization)

and i created an circuit embedding custom for signal dBm high 

## 💻 Software Implementation

The python script connects to the RTL-SDR, captures raw RF IQ samples at the specified center frequency, calculates the Power Spectral Density (PSD) using Fast Fourier Transform (FFT), and plots the signal spectrum.

## 🚀 How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com
   ```
2. Install dependencies:
   ```bash
   pip install pyrtlsdr matplotlib numpy
   ```
3. Connect your RTL-SDR and run the Python script:
   ```bash
   python telescope_receiver.py


   import time
import numpy as np
import matplotlib.pyplot as plt
from rtlsdr import RtlSdr

# 1. SDR Configuration
sdr = RtlSdr()
sdr.sample_rate = 2.4e6       # 2.4 MHz sample rate
sdr.center_freq = 1420.4e6    # 1420.4 MHz (Hydrogen Line Frequency)
sdr.gain = 'auto'             # Automatic gain setup

print("📡 Radio Telescope Initialized...")
print(f"Listening at Center Frequency: {sdr.center_freq / 1e6} MHz")

try:
    # Interactive mode for real-time plotting
    plt.ion()
    fig, ax = plt.subplots()
    
    # Loop to read and plot data 50 times
    for i in range(50):
        # Read 1 Million IQ samples from SDR
        samples = sdr.read_samples(1024*1024)
        
        # Clear previous plot and calculate Power Spectral Density (PSD)
        ax.clear()
        ax.psd(samples, NFFT=1024, Fs=sdr.sample_rate/1e6, Fc=sdr.center_freq/1e6)
        
        # Set graph labels and title
        ax.set_title("Cosmic Signal Power Spectrum")
        ax.set_xlabel("Frequency (MHz)")
        ax.set_ylabel("Relative Power (dB)")
        
        plt.draw()
        plt.pause(0.1)
        time.sleep(0.1)

finally:
    # Safely close the SDR connection after finishing
    sdr.close()
    print("🔌 SDR Connection Closed Safely.")
    
