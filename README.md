# Vivado on macOS via Docker

This repository provides a solution to run Xilinx Vivado on macOS using Docker containerization.

## Normal Vivado Workflow

The typical FPGA development workflow in Vivado consists of:
1. RTL Design (Verilog/VHDL)
2. Synthesis
3. Implementation
4. Generate Bitstream

### Programming with Docker Limitation

When running Vivado in a container, direct hardware programming is not possible due to USB device access restrictions. To solve this, we use `openFPGALoader`:

1. Generate bitstream in containerized Vivado
2. Locate bitstream in your project directory (typically at `<project_name>/<project_name>.runs/impl_1/<top_level_module>.bit`)
3. Use `openFPGALoader` on host to program FPGA:
    ```bash
    brew install openfpgaloader
    openFPGALoader -b basys3 /path/to/project/<project_name>.runs/impl_1/<top_level_module>.bit
    ```

Note: Supported board names can be listed using `openFPGALoader -h`

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Usage](#usage)
4. [Troubleshooting](#troubleshooting)

## Prerequisites
0. **Disk Space**
    - Ensure you have at least 120GB of free disk space:
        - ~80GB for Vivado download and Extract (this space will be freed after installation)
        - ~40GB for program data
1. **Homebrew**
    - Install Homebrew by running:
        ```bash
        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
        ```
    - Follow any additional setup instructions provided by the installer

2. **Docker Desktop**
    - Install Docker Desktop for macOS from [docker.com](https://www.docker.com/products/docker-desktop)
    - Alternatively, install via Homebrew:
        ```bash
        brew install --cask docker
        ```

3. **XQuartz**
    - Install via Homebrew:
        ```bash
        brew install --cask xquartz
        ```
    - After installation, restart your computer
    - Open XQuartz and enable "Allow connections from network clients" in XQuartz preferences
    - Navigate to XQuartz -> Settings -> Security -> Allow connections from network clients

4. **Vivado Installer**
    - Download Vivado installer for Linux from [AMD/Xilinx website](https://www.xilinx.com/support/download.html)

## Installation

1. **Get the Repository**
    ```bash
    git clone https://github.com/yokeTH/vivado-mac.git
    # or download and extract the ZIP file
    ```

2. **Run Setup Script**
    ```bash
    cd vivado-mac
    ./scripts/setup.sh
    ```

3. **Install Vivado**
    - When prompted, drag and drop the downloaded Vivado installer into the terminal
    - Follow the installation instructions in the Vivado installer
    - Select desired Vivado components

## Usage
0. **Ensure Display Setup**
    - Check [X11 Display Issues](#x11-display-issues) if you encounter problems
    - XQuartz must be running before starting Vivado

1. Start XQuartz
    ```bash
    xhost + localhost
    ```

2. Launch Vivado container:
    ```bash
    ./scripts/start_container.sh
    ```
3. Vivado GUI will appear in XQuartz window

## Troubleshooting

### Common Issues

1. **X11 Display Issues**
    - Ensure XQuartz is running
    - In XQuartz preferences:
      - Go to Security tab
      - Check "Allow connections from network clients"
    - Try restarting XQuartz
    - Run `xhost + localhost` before starting container
- For permission issues, ensure setup script has executable permissions (`chmod +x scripts/setup.sh`)


## License

This project is licensed under the BSD 3-Clause License - see the LICENSE file for details.

## Vivado License

Vivado requires a license from AMD/Xilinx. Please obtain appropriate licensing from AMD/Xilinx website.

## Disclaimer

This repository only provides the environment setup to run Vivado on Apple Silicon Macs via Docker. It does not include Vivado software itself. Users must:
- Download Vivado separately from AMD/Xilinx
- Comply with AMD/Xilinx's licensing terms
- Use at their own risk