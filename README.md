# Shake, Rattle, and Roll  
**Embedded Challenge Spring 2025 - Term Project**

## 🧠 Objective
This project aims to detect and classify abnormal hand movements in Parkinson’s patients using data collected from the onboard accelerometer/gyroscope. The primary goals are:
- Detect **tremors** (3–5 Hz): rhythmic oscillations due to low dopamine (“Off” state)
- Detect **dyskinesia** (5–7 Hz): excessive movements caused by excess dopamine (“Too On” state)
- Quantify movement **intensity**
- Indicate conditions using **on-board indicators** (e.g., LEDs, buzzer)

## 📦 Project Features
- 📊 **3-second sampling window** of motion data
- ⚡ **FFT-based frequency analysis** for condition detection
- 🔔 Condition indication using **peripherals only** (no serial output)
- 🔋 Runs on portable power (e.g., power bank)
- 🧩 Implemented entirely on dev board using **PlatformIO**
- 🚫 No external hardware or modules allowed

## 🧪 Detection Criteria
| Symptom      | Frequency Range | Description                         |
|--------------|------------------|-------------------------------------|
| Tremor       | 3–5 Hz           | Indicates “Off” state               |
| Dyskinesia   | 5–7 Hz           | Indicates excessive dopamine levels |

## ⚙️ How It Works

### 1. **Data Collection**
- The onboard accelerometer samples motion data at a fixed rate of **104 Hz**.
- A total of **256 samples** are collected, representing approximately **2.46 seconds** of motion data.
- For each sample, the **magnitude of acceleration** is calculated as `sqrt(x^2 + y^2 + z^2)` to combine the X, Y, and Z axis data.

### 2. **Frequency Analysis**
- The collected data is processed using a **Fast Fourier Transform (FFT)** to convert the time-domain signal into the frequency domain.
- Only the **first half** of the FFT output is analyzed, as the second half is symmetric for real-valued input signals.
- The **frequency resolution** is calculated as `frequency_resolution = sampling_rate / sample_size`, allowing each FFT bin to correspond to a specific frequency.

### 3. **Motion Detection**
- The program identifies two types of motion abnormalities based on frequency and amplitude thresholds:
  - **Tremor**: Detected in the **3–5 Hz** frequency range if the amplitude exceeds **14.0** and at least **2 bins** meet this condition.
  - **Dyskinesia**: Detected in the **5–7 Hz** frequency range if the amplitude exceeds **15.0** and at least **3 bins** meet this condition.
- Detection results are indicated using onboard LEDs:
  - **Tremor**: LED remains solid ON for 5 seconds.
  - **Dyskinesia**: LED blinks 25 times with a 200 ms interval.
  - **No abnormal motion**: LED remains OFF.

### 4. **Real-Time Operation**
- The detection process is repeated every **5 seconds**, providing real-time feedback on motion patterns.

## 🎯 Grading Criteria
| Category                          | Weight |
|----------------------------------|--------|
| Tremor detection                 | 25%    |
| Dyskinesia detection             | 25%    |
| Intensity quantification         | 10%    |
| Robustness (video demonstration) | 10%    |
| Ease of use                      | 10%    |
| Creativity of indication method  | 10%    |
| Code quality                     | 5%     |
| System complexity                | 5%     |
