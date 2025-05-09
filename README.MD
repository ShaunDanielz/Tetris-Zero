# Tetris Zero

## Project Overview
Tetris Zero is an implementation of the classic Tetris game enhanced with artificial intelligence inspired by the AlphaZero framework. The project leverages a combination of Monte Carlo Tree Search (MCTS) and deep neural networks (using TensorFlow) to train an AI agent to play Tetris autonomously. The AI is trained using a self-play mechanism with a dual-head neural network that simultaneously predicts move probabilities and position evaluations.

This project aims to:
- Demonstrate the application of AlphaZero-like techniques to stochastic environments like Tetris
- Utilize GPU acceleration for faster training where available
- Provide a visual interface for monitoring training progress and AI performance
- Support model export for web-based applications

## Project Structure

The project is organized into the following modules:

- `main.py`: Entry point for the application
- `config.py`: Configuration settings, constants, and Tetris piece definitions
- `utils.py`: Utility functions for GPU setup and model conversion
- `tetris_env.py`: Tetris game environment used for training the AI
- `tetris_zero_agent.py`: Implementation of the AlphaZero-inspired reinforcement learning agent
- `ui_components.py`: Reusable UI components for the pygame interface
- `plotting.py`: Utilities for plotting training metrics
- `trainer_visualization.py`: Main visualization interface for training

## Prerequisites
Before setting up the project, ensure you have the following installed:
- **Anaconda** or **Miniconda**: For managing Python environments and dependencies
- **NVIDIA GPU** (optional, for GPU acceleration): Requires compatible drivers installed
- **Windows**: This setup is tested on Windows, but it can be adapted for other platforms

## Environment Setup with Conda
To ensure a consistent and isolated environment, this project uses Conda to manage dependencies. Follow these steps to set up the environment with Python 3.10 and TensorFlow 2.10.0, optimized for GPU support.

### Step 1: Clone the Repository
Clone this repository to your local machine:
```bash
git clone https://github.com/ShaunDanielz/tetris-zero.git
cd tetris-zero
```

### Step 2: Create the Conda Environment
Create a new Conda environment named `tetris_zero` with Python 3.10:
```bash
conda create --name tetris_zero python=3.10
```

### Step 3: Activate the Environment
Activate the environment to begin working in it:
```bash
conda activate tetris_zero
```

### Step 4: Install Dependencies
Install the required packages, including TensorFlow with GPU support, Pygame, and other dependencies:

#### Core Dependencies
```bash
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1 tensorflow-gpu=2.10.0 pygame matplotlib
pip install "numpy<2.0" psutil
```

- **TensorFlow 2.10.0**: Chosen for its stability and GPU support with CUDA 11.2
- **CUDA 11.2 and cuDNN 8.1**: Required for TensorFlow 2.10.0 GPU support, installed via Conda to avoid system-wide conflicts
- **NumPy <2.0**: TensorFlow 2.10.0 is incompatible with NumPy 2.x due to breaking changes; this ensures compatibility
- **Pygame**: Used for the Tetris game environment and visualization
- **Matplotlib**: Used for plotting training metrics
- **psutil**: Utility for monitoring system resources during training

#### Optional Dependencies
For web export capability (optional):
```bash
pip install tensorflowjs
```

### Step 5: Verify GPU Support
After installation, test if TensorFlow can detect your GPU:
```bash
python -c "import tensorflow as tf; print('GPU available' if tf.config.list_physical_devices('GPU') else 'No GPU available')"
```
- Expected output: `GPU available`
- If it says `No GPU available`, see the troubleshooting section below.

## Running the Project
To train the AI or play the game:
1. Ensure you're in the `tetris_zero` environment:
   ```bash
   conda activate tetris_zero
   ```
2. Run the main script:
   ```bash
   python main.py
   ```
   - If no saved model is found, it starts fresh training
   - Training will use the GPU if detected; otherwise, it falls back to CPU (slower)

## Training Interface

The application provides a visual interface with several tabs:

- **Training**: Main controls for starting/pausing training, saving/loading models
- **Metrics**: Graphs of loss, value accuracy, policy accuracy, and win rates
- **Network**: Visualization of neural network structure and policy/value predictions
- **Board Analysis**: Analysis of the current game state and board metrics
- **Performance**: Performance metrics for the training process
- **Web Export**: Tools for exporting models for web-based use

## Controls

- **Start/Pause Training**: Toggle training process
- **Step**: Perform a single training step (useful in step mode)
- **Save Model**: Save the current model weights
- **Load Model**: Load model weights from a file
- **Reset**: Reset the training process
- **Export for Web**: Export the model for use in web applications

## Key Parameters

You can adjust several parameters during training:
- **Training Speed**: Control the speed of the training process
- **MCTS Simulations**: Control the number of MCTS simulations per move
- **Frame Skip**: Skip rendering frames to increase training speed
- **Display options**: Toggle heatmap, policy values, and ghost piece visualization

## Model Export

Models are automatically saved during training. They can also be exported to TensorFlow.js format for use in web applications. The exported models are saved in the `models/tetris_zero/web_model_latest/` directory.

## Troubleshooting GPU Issues
If TensorFlow fails to detect your GPU (`No GPU available`), you may see warnings like:
```
Could not load dynamic library 'cufft64_10.dll'; dlerror: cufft64_10.dll not found
Cannot dlopen some GPU libraries...
Skipping registering GPU devices...
```

### Fixes
1. **Ensure CUDA and cuDNN Compatibility**:
   - Verify that `cudatoolkit=11.2` and `cudnn=8.1` are installed in the environment:
     ```bash
     conda list | findstr "cudatoolkit cudnn tensorflow"
     ```
   - If missing, reinstall them:
     ```bash
     conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1
     ```

2. **Check NVIDIA Drivers**:
   - Ensure your NVIDIA GPU drivers are up to date. Download from [NVIDIA Driver Downloads](https://www.nvidia.com/Download/index.aspx).

3. **Reinstall TensorFlow**:
   - If issues persist, uninstall and reinstall TensorFlow:
     ```bash
     pip uninstall tensorflow
     pip install tensorflow==2.10.0
     ```

4. **System CUDA Conflict**:
   - If you have CUDA 12.2 installed system-wide (check with `nvcc --version`), it won't affect the Conda environment since `cudatoolkit` is isolated. Avoid modifying the system PATH unless necessary.

## Notes
- **Performance**: GPU training is significantly faster than CPU. Ensure GPU support is working for optimal performance.
- **Upgrading TensorFlow**: If you prefer a newer TensorFlow version (e.g., 2.13.0 with CUDA 12.0), adjust the `cudatoolkit` version accordingly, but note that versions >=2.11 require WSL2 on Windows for GPU support.
- **Contributing**: Feel free to submit pull requests or issues to improve the project!

## License
This project is open-source under the [MIT License](https://github.com/ShaunDanielz/tetris-zero/blob/main/LICENSE.md).