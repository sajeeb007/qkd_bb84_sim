# QKD BB84 Simulation with Image Encryption and Noise Effects

## Overview

This repository contains a comprehensive Jupyter Notebook (`qkd-image-encryption-noise.ipynb`) that simulates the Quantum Key Distribution (QKD) protocol using the BB84 scheme, integrated with image encryption and decryption using AES. The simulation demonstrates how quantum key distribution works, the impact of eavesdropping (Eve), and the effects of noise on the quantum channel, ultimately showing how these factors influence the quality of decrypted images.

The notebook is designed for educational and research purposes, providing a hands-on exploration of quantum cryptography concepts, quantum mechanics principles, and their practical applications in secure communication and data encryption.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
  - [BB84 Protocol Overview](#bb84-protocol-overview)
  - [Quantum Key Distribution Phases](#quantum-key-distribution-phases)
  - [Image Encryption and Decryption](#image-encryption-and-decryption)
  - [Noise Simulation](#noise-simulation)
- [Code Structure](#code-structure)
- [Results and Visualization](#results-and-visualization)
- [Dependencies](#dependencies)
- [Contributing](#contributing)
- [License](#license)
- [References](#references)

## Features

- **Complete BB84 QKD Simulation**: Implements the full BB84 quantum key distribution protocol with Alice, Bob, and optional Eve (eavesdropper).
- **Quantum Circuit Encoding**: Uses Qiskit to create and manipulate quantum circuits for qubit encoding in Z and X bases.
- **Measurement Simulation**: Simulates quantum measurements with configurable bases and noise introduction.
- **Eavesdropping Demonstration**: Shows how an eavesdropper (Eve) can intercept and resend qubits, affecting key security.
- **Key Comparison and Validation**: Implements public discussion phase for basis comparison and key distillation.
- **Noise Modeling**: Incorporates realistic noise effects in quantum channels, including bit flip probabilities based on transmission distance.
- **Image Encryption/Decryption**: Demonstrates practical application by encrypting and decrypting grayscale images using AES with the generated quantum keys.
- **Gradual Noise Effects**: Shows how key similarity affects decryption quality, with gradual noise introduction based on key mismatch.
- **Visualization**: Includes plots and image displays to visualize the encryption/decryption process and noise effects.
- **Statistical Analysis**: Provides metrics like key similarity and bit error rates.

## Prerequisites

- Python 3.7 or higher
- Jupyter Notebook or JupyterLab
- Basic understanding of quantum mechanics and cryptography concepts
- Familiarity with Python programming

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/qkd-bb84-sim.git
   cd qkd-bb84-sim
   ```

2. **Install required packages**:
   The notebook includes installation commands for Qiskit and Qiskit Aer. Run the first two cells to install:
   ```python
   !pip install --quiet qiskit
   !pip install --quiet qiskit-aer
   ```

3. **Additional dependencies**:
   Install other required packages:
   ```bash
   pip install numpy matplotlib opencv-python pycryptodome pillow
   ```

4. **Launch Jupyter Notebook**:
   ```bash
   jupyter notebook qkd-image-encryption-noise.ipynb
   ```

## Usage

1. **Open the notebook** in Jupyter and run cells sequentially.

2. **Phase 1: Key Generation**
   - Alice generates random bits and bases
   - Qubits are encoded and "sent" to Bob

3. **Phase 2: Eavesdropping (Optional)**
   - Eve can intercept qubits, measure them, and resend
   - This introduces noise and compromises security

4. **Phase 3: Bob's Measurement**
   - Bob measures received qubits with random bases

5. **Phase 4: Key Comparison**
   - Alice and Bob compare bases publicly
   - They distill the final secret key

6. **Image Processing**
   - Load a grayscale image (256x256)
   - Encrypt using Alice's key
   - Decrypt using Bob's potentially noisy key
   - Visualize results

7. **Experiment with Parameters**:
   - Adjust the number of qubits (default: 600)
   - Modify noise levels and transmission distances
   - Change block sizes for encryption (default: 16x16)

## How It Works

### BB84 Protocol Overview

The BB84 protocol, proposed by Charles Bennett and Gilles Brassard in 1984, is a quantum key distribution method that allows two parties (Alice and Bob) to securely share a secret key over an insecure quantum channel. The security is guaranteed by the laws of quantum mechanics, specifically the no-cloning theorem and the uncertainty principle.

**Key Principles**:
- Quantum states cannot be perfectly copied
- Measuring a quantum state disturbs it
- Information-theoretic security

### Quantum Key Distribution Phases

#### Phase 1: Quantum Transmission
- **Alice** generates random bits (0 or 1) and random bases (Z or X)
- She encodes each bit into a qubit using the chosen basis:
  - Z basis: |0⟩ or |1⟩
  - X basis: |+⟩ = (|0⟩ + |1⟩)/√2 or |-⟩ = (|0⟩ - |1⟩)/√2
- Encoded qubits are sent to Bob via quantum channel

#### Phase 2: Eavesdropping
- **Eve** (eavesdropper) intercepts qubits
- She measures them using random bases
- Due to quantum measurement rules, this introduces errors
- Eve resends measured qubits to Bob

#### Phase 3: Measurement
- **Bob** receives qubits and measures them using random bases
- He records measurement results

#### Phase 4: Public Discussion and Key Distillation
- Alice and Bob publicly compare their chosen bases (not the bits)
- They keep only bits where bases matched
- They perform error correction and privacy amplification
- Final secret key is established

### Image Encryption and Decryption

The notebook demonstrates practical application of QKD-generated keys:

1. **Key Preparation**:
   - Convert binary key lists to byte arrays suitable for AES
   - Use SHA-256 hashing for key derivation

2. **AES Encryption**:
   - Divide 256x256 grayscale image into 16x16 blocks
   - Encrypt each block using AES in CBC mode
   - Reconstruct encrypted image

3. **Decryption with Noise Effects**:
   - Decrypt using potentially corrupted key
   - Calculate key similarity between Alice's and Bob's keys
   - Introduce gradual noise based on similarity:
     - High similarity (>90%): Clean decryption
     - Low similarity: Increasing noise levels

### Noise Simulation

The simulation includes realistic noise modeling:

- **Bit Flip Noise**: Probability based on transmission distance
- **Gaussian Variation**: Adds randomness to noise levels
- **Distance-Based Attenuation**: Noise increases with transmission distance

```python
noise_prob = min(0.1, np.random.normal(noise_level, 0.005))
```

## Code Structure

### Core Functions

- `encode_qubits(bits, bases)`: Encodes classical bits into quantum states
- `measure_qubits(qubits, bases)`: Measures qubits in specified bases
- `measure_qubits_with_noise()`: Measurement with noise introduction
- `generate_bits(n)` / `generate_bases(n)`: Random bit/basis generation
- `eliminate_differences()`: Key distillation after basis comparison

### Image Processing Functions

- `preprocess_image()`: Resize and prepare images
- `encrypt_image()`: AES block-wise encryption
- `decrypt_image_with_gradual_noise()`: Decryption with similarity-based noise
- `key_similarity()`: Calculate bit-wise key matching percentage

### Simulation Parameters

- `num_bits`: Number of qubits (default: 600)
- `BLOCK_SIZE`: Image block size for encryption (default: 16)
- `noise_coefficient`: Noise per km (default: 0.001)
- `transmission_distances`: Simulated distance in km

## Results and Visualization

The notebook provides several visualization outputs:

1. **Key Comparison**:
   - Alice's and Bob's distilled keys
   - Key length and security status

2. **Image Visualization**:
   - Original grayscale image
   - Encrypted (noise-like) image
   - Decrypted image with distortion based on key quality

3. **Statistical Metrics**:
   - Key similarity percentage
   - Bit error rates

Example output visualization shows how noise progressively degrades image quality as key similarity decreases.

## Dependencies

- **Qiskit**: Quantum computing framework
- **Qiskit Aer**: High-performance quantum simulator
- **NumPy**: Numerical computing
- **Matplotlib**: Plotting and visualization
- **OpenCV**: Computer vision and image processing
- **PyCryptoDome**: Cryptographic functions (AES)
- **Pillow**: Image handling
- **IPython**: Interactive Python (for notebook displays)

## Contributing

Contributions are welcome! Please feel free to submit issues, feature requests, or pull requests.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Areas for improvement:
- Add more sophisticated noise models
- Implement full error correction protocols
- Add support for color images
- Include performance benchmarks
- Add unit tests

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## References

1. Bennett, C. H., & Brassard, G. (1984). Quantum cryptography: Public key distribution and coin tossing. In Proceedings of IEEE International Conference on Computers, Systems and Signal Processing (pp. 175-179).

2. Nielsen, M. A., & Chuang, I. L. (2010). Quantum computation and quantum information. Cambridge University Press.

3. Qiskit Documentation: https://qiskit.org/documentation/

4. AES Specification: https://csrc.nist.gov/publications/detail/fips/197/final

For more information on quantum cryptography and QKD implementations, refer to the Qiskit tutorials and quantum information resources.

---

**Note**: This simulation is for educational purposes and demonstrates quantum key distribution concepts. Real-world QKD systems require specialized hardware and are subject to various practical limitations not covered in this notebook.