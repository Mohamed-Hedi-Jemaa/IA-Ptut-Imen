# Système de Positionnement RTT avec ESP32

# Indoor Positioning System (IPS) Using RSSI and Neural Networks

## Project Overview
This project implements an indoor positioning system that can determine a person or device's location inside a room using WiFi signal strengths (RSSI) from two ESP32 devices. Think of it like "indoor GPS" that uses WiFi signals instead of satellites.

## How It Works

### 1. Basic Concept
- Two ESP32 devices are placed at fixed positions in the room
- These ESP32s act like "indoor satellites"
- When someone moves around the room, we measure:
  * How strong the WiFi signal is from ESP32_1 (RSSI_1)
  * How strong the WiFi signal is from ESP32_2 (RSSI_2)
- Using these two signal strengths, we can predict where the person is
-------------------------------------
Example at one grid point (1, 1):
Code
CopyInsert
Scan 1: ESP32_1: -65dBm, ESP32_2: -70dBm
Scan 2: ESP32_1: -64dBm, ESP32_2: -71dBm
Scan 3: ESP32_1: -66dBm, ESP32_2: -69dBm
...
Scan 10: ESP32_1: -65dBm, ESP32_2: -70dBm
Why 10 measurements?
RSSI values fluctuate due to:
Environmental interference
People moving
Signal reflections
Taking multiple readings helps get a more stable average
We use the average value for better accuracy
In data_collector.py:
python
CopyInsert
# Pseudocode of what happens
for x_position in grid_points_x:
    for y_position in grid_points_y:
        # Move to this position physically
        print(f"Please move to position ({x_position}, {y_position})")
        
        # Collect 10 readings
        readings = []
        for i in range(10):
            rssi_esp1 = scan_wifi_from_esp32_1()  # Get RSSI from first ESP32
            rssi_esp2 = scan_wifi_from_esp32_2()  # Get RSSI from second ESP32
            readings.append({
                'x': x_position,
                'y': y_position,
                'esp1_rssi': rssi_esp1,
                'esp2_rssi': rssi_esp2
            })
            time.sleep(1)  # Wait a bit between readings
So while you only have 2 ESP32s, you're taking multiple measurements from each one at every position to account for signal variations and get more reliable data. This is similar to how your phone might show different WiFi signal strengths even when you're not moving.

### 2. Data Collection
- We create a grid in the room (every 0.5 meters)
- At each grid point:
  * Take 10 RSSI measurements from each ESP32
  * Store the position (x,y) and RSSI values
- This creates our training dataset
- Data is saved in two formats:
  * Raw data: Every individual measurement
  * Processed data: Averaged measurements per position

  Let me explain the data collection process:

Physical Setup:
You have 2 ESP32s fixed at known positions:
ESP32_1 at (-2, 1.5) meters
ESP32_2 at (2.5, -1.5) meters
Collection Process:
You (or someone) physically moves to each grid point with a device (like a phone or another ESP32)
At each point (let's say at position (1, 1) meters):
Your device scans for WiFi signals 10 times
Each scan gets:
One RSSI reading from ESP32_1
One RSSI reading from ESP32_2
So after 10 scans at this position, you have:
10 RSSI values from ESP32_1
10 RSSI values from ESP32_2

### 3. Neural Network
- We use a simple neural network that:
  * Takes input: RSSI values from both ESP32s
  * Gives output: Predicted (x,y) position
- The network learns patterns between:
  * Signal strengths (input)
  * Actual positions (output)

## Project Structure
```
├── src/
│   ├── indoor_positioning_rssi.py  # Main RSSI positioning code
│   ├── indoor_positioning_rtt.py   # RTT positioning implementation
│   ├── models.py                   # Neural network model definitions
│   ├── rtt_collector.py           # RTT data collection
│   └── data_collector.py          # RSSI data collection
├── data/                         # Collected and processed data
│   ├── rssi_raw_data.csv        # Raw RSSI measurements
│   └── rssi_processed_data.csv  # Processed RSSI data
└── requirements.txt             # Python dependencies
```

## Key Components

### 1. Data Generation (`indoor_positioning_rssi.py`)
- Creates synthetic RSSI data
- Simulates real-world measurements
- Helps test the system before real deployment
- Parameters:
  * Room size: 6.47m × 4.85m
  * Grid spacing: 0.5m
  * Samples per position: 10

### 2. Neural Network Model (`models.py`)
- Simple architecture:
  * Input layer: 2 neurons (RSSI_1, RSSI_2)
  * Hidden layers with ReLU activation
  * Output layer: 2 neurons (x, y position)
- Trained using Mean Squared Error loss
- Optimized with Adam optimizer

### 3. Data Processing
- Raw data includes:
  * device_id
  * x_position
  * y_position
  * rssi_value
- Processed data includes:
  * x_position
  * y_position
  * averaged_rssi_esp32_1
  * averaged_rssi_esp32_2

## Setup Instructions

1. **Environment Setup**
   ```bash
   pip install -r requirements.txt
   ```

2. **Running in Google Colab**
   - Data is saved to Google Drive
   - Mount drive: `drive.mount('/content/drive')`
   - Access data in: `/content/drive/MyDrive/indoor_positioning_data/`

3. **Training the Model**
   - Run `indoor_positioning_rssi.py`
   - Model will train on generated/collected data
   - Results show positioning accuracy

## Expected Results
- Position prediction accuracy typically within 1-2 meters
- Better accuracy in areas with stronger signal coverage
- Performance depends on:
  * Number of ESP32 devices
  * Room layout
  * Environmental factors

## Future Improvements
1. Add more ESP32 devices for better accuracy
2. Implement real-time positioning
3. Add signal filtering for noise reduction
4. Integrate with mobile app for visualization

## License
MIT License

## Remerciements
- Imen
- Coulin
- Amélie
