# IA-Ptut-Imen

# RTT Positioning System Using ESP32

## Overview
This project implements an indoor positioning system based on RTT (Round Trip Time) distance measurements and position prediction using two ESP32 devices. The system leverages deep learning models to correct measured distances and predict (x,y) coordinates of reference points within a defined room.

## Room Configuration
- Room Dimensions: 6.47m × 4.85m
- ESP32 Placements:
  - ESP32_1: (-2, 1.5)
  - ESP32_2: (2.5, -1.5)
- Grid Resolution: 0.5m × 0.5m

## System Architecture

### Deep Learning Models

#### 1. RCDN (RTT Compensation Distance Network)
- 1D convolutional network designed to reduce RTT distance noise
- Input: Raw RTT samples
- Output: Corrected distances

#### 2. RPN (Region Proposal Network)
- GRU-based network for (x,y) coordinate prediction
- Input: Corrected distances
- Output: Predicted coordinates

#### 3. Combined Model
- End-to-end integration of RCDN and RPN
- Optimized for both distance and position errors

## Installation

### Prerequisites
- Python 3.8 or higher
- ESP32 development environment
- Required Python packages:

```bash
pip install tensorflow pandas numpy matplotlib seaborn scikit-learn
```

### Hardware Requirements
- 2x ESP32 development boards
- USB cables for programming
- Power supply for ESP32s

## Project Structure

```
positioning_system/
├── data/
│   ├── synthetic/         # Synthetic training data
│   └── real/             # Real measurement data
├── models/
│   ├── rcdn.py           # RTT Compensation Distance Network
│   ├── rpn.py            # Region Proposal Network
│   └── combined.py       # Combined model implementation
├── utils/
│   ├── data_generator.py # Synthetic data generation
│   └── visualization.py  # Plotting and visualization tools
└── main.py              # Main execution script
```

## Data Generation
The system generates synthetic data with the following characteristics:
- Random reference points within the room grid
- True distances to ESP32s
- Gaussian-distributed noisy RTT samples

Example data format:
```
Reference_Point_X | Reference_Point_Y | True_Distance | RTT_Samples
1.0              | 2.5              | [3.1, 2.7]    | [[3.0, 2.8], [3.1, 2.6], ...]
3.0              | 1.0              | [2.2, 4.1]    | [[2.3, 4.0], [2.1, 4.2], ...]
```

## Usage

1. Clone the repository:
```bash
git clone [repository-url]
cd rtt-positioning-system
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Run the main script:
```bash
python main.py
```

## Results and Visualization
The system provides:
- Training history plots
- True vs predicted position visualization
- Error distribution analysis
- RMSE and mean error metrics

## Future Improvements
- [ ] Real-world data collection and validation
- [ ] Hyperparameter optimization
- [ ] Real-time embedded deployment
- [ ] Multi-ESP32 support for improved accuracy

## Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments
- ESP32 development community
- TensorFlow team
- Contributors and testers
