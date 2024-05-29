# Radeon Open Compute Tradecraft
This is a collection of Radeon Open Compute (ROCm) commnds, best practices and what not that will make you more confident with the AMD ROCm toolkit. And assist in transitioning from CUDA, over to a community driven parallel programming ecoystem you have control over. 

Welcome to the future folks...lets get started.

## Table of Contents

- [Radeon Open Compute Tradecraft](#radeon-open-compute-tradecraft)
  - [ROCm Tradecraft for GPU Performance Tuning](#rocm-tradecraft-for-gpu-performance-tuning)
    - [System Setup and Installation](#system-setup-and-installation)
      - [Installing ROCm](#installing-rocm)
      - [Adding ROCm Repository](#adding-rocm-repository)
      - [Installing ROCm Components](#installing-rocm-components)
  - [ROCm Component Packages](#rocm-component-packages)
    - [rocm-dev](#rocm-dev)
    - [rocm-utils](#rocm-utils)
    - [rocm-libs](#rocm-libs)
    - [miopen-hip](#miopen-hip)
  - [Monitoring and Managing GPU Usage](#monitoring-and-managing-gpu-usage)
    - [Listing Available GPUs](#listing-available-gpus)
    - [Monitoring GPU Utilization](#monitoring-gpu-utilization)
    - [Checking GPU Temperature](#checking-gpu-temperature)
    - [Checking GPU Power Consumption](#checking-gpu-power-consumption)
    - [Checking GPU Fan Speed](#checking-gpu-fan-speed)
    - [Checking GPU Clock Frequencies](#checking-gpu-clock-frequencies)
  - [GPU Performance Tuning](#gpu-performance-tuning)
    - [Setting GPU Power Profile](#setting-gpu-power-profile)
    - [Setting GPU Fan Speed](#setting-gpu-fan-speed)
    - [Setting GPU Memory Clock](#setting-gpu-memory-clock)
    - [Setting GPU Performance Level](#setting-gpu-performance-level)
  - [Diagnostics and Debugging](#diagnostics-and-debugging)
    - [Checking GPU Health](#checking-gpu-health)
    - [Resetting GPU](#resetting-gpu)
    - [Saving GPU Logs](#saving-gpu-logs)
    - [Clearing GPU Logs](#clearing-gpu-logs)
  - [GPU Memory Management](#gpu-memory-management)
    - [Checking GPU Memory Usage](#checking-gpu-memory-usage)
    - [Clearing GPU Memory](#clearing-gpu-memory)
  - [Advanced GPU Configuration](#advanced-gpu-configuration)
    - [Overclocking GPU](#overclocking-gpu)
    - [Underclocking GPU](#underclocking-gpu)
    - [Setting Power Cap](#setting-power-cap)
  - [ROCm-SMI Commands Summary](#rocm-smi-commands-summary)
    - [General Information](#general-information)
    - [Temperature and Fan](#temperature-and-fan)
    - [Power and Performance](#power-and-performance)
    - [Clock Speeds](#clock-speeds)
    - [Memory](#memory)
    - [Logs and Health](#logs-and-health)
    - [Reset](#reset)
  - [Development Tools](#development-tools)
    - [Installing HIP](#installing-hip)
    - [Compiling HIP Programs](#compiling-hip-programs)
    - [Running HIP Programs](#running-hip-programs)
    - [Using ROCm Libraries](#using-rocm-libraries)
  - [Performance Tuning and Benchmarking](#performance-tuning-and-benchmarking)
    - [Installing ROCm Bandwidth Test](#installing-rocm-bandwidth-test)
    - [Running Bandwidth Test](#running-bandwidth-test)
    - [Using rocprof for Profiling](#using-rocprof-for-profiling)
    - [Using rocminfo for System Information](#using-rocminfo-for-system-information)
  - [RoCE and GPU Network Fabrics](#roce-and-gpu-network-fabrics)
    - [Installing RDMA Tools](#installing-rdma-tools)
    - [Configuring RoCE](#configuring-roce)
    - [Checking RDMA Devices](#checking-rdma-devices)
    - [Displaying RDMA Configuration](#displaying-rdma-configuration)
    - [Setting Up RDMA Over TCP/IP](#setting-up-rdma-over-tcpip)
    - [Listing RDMA Interfaces](#listing-rdma-interfaces)
    - [Running RDMA Bandwidth Test](#running-rdma-bandwidth-test)
    - [RDMA Latency Test](#rdma-latency-test)
    - [Connecting RDMA Devices](#connecting-rdma-devices)
    - [Using rping for RDMA Connectivity Testing](#using-rping-for-rdma-connectivity-testing)
      - [Server-side](#server-side)
      - [Client-side](#client-side)
    - [Configuring QoS for RDMA Traffic](#configuring-qos-for-rdma-traffic)
    - [Setting up RDMA Multicast](#setting-up-rdma-multicast)
    - [Monitoring RDMA Traffic](#monitoring-rdma-traffic)
  - [Performance and Benchmarking Cookbook Using ROCm, RDMA, and RoCE](#performance-and-benchmarking-cookbook-using-rocm-rdma-and-roce)
    - [Installing Required Tools](#installing-required-tools)
    - [Basic Performance Testing](#basic-performance-testing)
    - [Profiling Applications with rocprof](#profiling-applications-with-rocprof)
    - [Using rocminfo for System Information](#using-rocminfo-for-system-information)
    - [RDMA and RoCE Performance Testing](#rdma-and-roce-performance-testing)
    - [Advanced Performance Benchmarking](#advanced-performance-benchmarking)
      - [Using rocblas-bench for BLAS Performance](#using-rocblas-bench-for-blas-performance)
      - [Using rocFFT for FFT Performance](#using-rocfft-for-fft-performance)
      - [Using RVS (ROCm Validation Suite)](#using-rvs-rocm-validation-suite)
        - [Example Configuration for RVS Stress Test](#example-configuration-for-rvs-stress-test)
        - [Running the Stress Test](#running-the-stress-test)
    - [Custom Performance Scripts](#custom-performance-scripts)
      - [Writing a Custom Benchmark Script](#writing-a-custom-benchmark-script)
      - [Running the Custom Script](#running-the-custom-script)
    - [Performance Optimization Tips](#performance-optimization-tips)
      - [Optimizing Kernel Execution](#optimizing-kernel-execution)
      - [Optimizing Data Transfers](#optimizing-data-transfers)
      - [Analyzing Bottlenecks](#analyzing-bottlenecks)
    - [Summary of Commands](#summary-of-commands)

## Transitioning from Nvidia CUDA to AMD ROCm

### Hardware Concepts

**CUDA (Nvidia)**   | **ROCm (AMD)**       | **Description**
--------------------|----------------------|---------------------------------------------------
Nvidia GPU          | AMD GPU              | Graphics Processing Unit (GPU) for parallel processing
Tensor Cores        | Matrix Cores         | Specialized cores for deep learning operations
NVLink              | Infinity Fabric      | High-bandwidth interconnect for communication between GPUs
CUDA Cores          | Stream Processors    | Fundamental processing units in the GPU
SM (Streaming Multiprocessor) | CU (Compute Unit)   | Hardware block containing multiple processing units
Warp                | Wavefront            | A group of threads executed in lock-step
Unified Memory      | Unified Address Space| Memory management allowing shared address space between CPU and GPU
nvcc (CUDA Compiler)| hipcc (HIP Compiler) | Compiler for converting code into executable for the GPU
CUDA Driver         | ROCm Driver          | Software component to manage GPU resources and execution

### CUDA vs ROCm General Terminology Concepts

**CUDA (Nvidia)** | **ROCm (AMD)**        | **Description**
------------------|-----------------------|--------------------------------------------------
CUDA              | HIP                   | ROCm's Heterogeneous-Computing Interface for Portability (HIP)
CUDA Toolkit      | ROCm Toolkit          | The software suite for developing applications for AMD GPUs
cuBLAS            | rocBLAS               | Library for basic linear algebra subprograms
cuDNN             | MIOpen                | Library for deep neural networks
cuFFT             | rocFFT                | Library for Fast Fourier Transform operations
cuRAND            | rocRAND               | Library for random number generation
cuSPARSE          | rocSPARSE             | Library for sparse matrix operations
nvprof            | rocprof               | Profiling tools for performance analysis
Nsight Compute    | rocProfiler, CodeXL   | Tools for performance analysis and debugging
Nsight Systems    | rocTracer             | Tools for system-wide tracing and performance optimization

## Programming Concepts

### CUDA vs ROCm

**CUDA (Nvidia)** | **ROCm (AMD)**                                 | **Description**
------------------|------------------------------------------------|-----------------------------------------------
__global__        | __global__                                     | Qualifier to define a kernel function
__device__        | __device__                                     | Qualifier for functions executed on the device
__host__          | __host__                                       | Qualifier for functions executed on the host
__shared__        | __shared__                                     | Qualifier for shared memory
__constant__      | __constant__                                   | Qualifier for constant memory
threadIdx         | hipThreadIdx_x, hipThreadIdx_y, hipThreadIdx_z | Variable for thread indices within a block
blockIdx          | hipBlockIdx_x, hipBlockIdx_y, hipBlockIdx_z    | Variable for block indices within a grid
blockDim          | hipBlockDim_x, hipBlockDim_y, hipBlockDim_z    | Variable for block dimensions
gridDim           | hipGridDim_x, hipGridDim_y, hipGridDim_z       | Variable for grid dimensions
cudaMalloc        | hipMalloc                                      | Function to allocate device memory
cudaFree          | hipFree                                        | Function to free device memory
cudaMemcpy        | hipMemcpy                                      | Function to copy memory between host and device

### System Setup and Installation

**Installing ROCm:**

```bash 
sudo apt update
sudo apt install -y rocm-dkms
```

**Adding ROCm Repository:**

```bash
echo 'deb [arch=amd64] http://repo.radeon.com/rocm/apt/debian/ xenial main' | sudo tee /etc/apt/sources.list.d/rocm.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key 609B6B9E
sudo apt update
```

**Installing ROCm Components:**

```bash
sudo apt install -y rocm-dev rocm-utils rocm-libs miopen-hip
```

## ROCm Component Packages

### rocm-dev
- **Description**: Development package for ROCm, including compiler and libraries needed for developing applications.
- **Components**: Includes the ROCm compiler, ROCm runtime, and various ROCm libraries for development.

### rocm-utils
- **Description**: Utility package for ROCm, providing tools for monitoring and managing ROCm-enabled devices.
- **Components**: Contains utilities like `rocm-smi` for system management and monitoring of ROCm devices.

### rocm-libs
- **Description**: Library package for ROCm, providing essential libraries for ROCm applications.
- **Components**: Includes libraries like ROCm Math Libraries (rocBLAS, rocFFT, rocSPARSE), ROCm Communication Libraries, and others necessary for running ROCm applications.

### miopen-hip
- **Description**: Machine Intelligence library for deep learning frameworks on ROCm, optimized for AMD GPUs.
- **Components**: Includes MIOpen, a GPU-accelerated library providing highly optimized implementations of standard deep learning operations.

## Monitoring and Managing GPU Usage

### Listing Available GPUs

To list all available GPUs:

```bash
/opt/rocm/bin/rocm-smi
```

### Monitoring GPU Utilization

To monitor GPU utilization in real-time:

```bash
watch -n 1 /opt/rocm/bin/rocm-smi --showhw
```

### Checking GPU Temperature

To check the current temperature of the GPUs:

```bash
/opt/rocm/bin/rocm-smi --showtemp
```

### Checking GPU Power Consumption

To check the power consumption of the GPUs:

```bash
/opt/rocm/bin/rocm-smi --showpower
```

### Checking GPU Fan Speed

To check the fan speed of the GPUs:

```bash
/opt/rocm/bin/rocm-smi --showfan
```

### Checking GPU Clock Frequencies

To check the clock frequencies of the GPUs:

```bash
/opt/rocm/bin/rocm-smi --showclk
```

## GPU Performance Tuning

### Setting GPU Power Profile

To set the power profile of a GPU:

```bash
sudo /opt/rocm/bin/rocm-smi --setsclk 4 --device 0
```

- **Description**: Sets the GPU power profile to a specific state.
- **Example**: `--setsclk 4` sets the GPU's power state to level 4.
- **Device**: `--device 0` specifies the target GPU (device 0).
- **Real-World Reasons**:
  - **Energy Efficiency**: Reduce power consumption and heat output in low-demand scenarios.
  - **Performance Tuning**: Increase power limits for high-performance tasks like deep learning model training.

### Setting GPU Fan Speed

To set the fan speed of a GPU:

```bash
sudo /opt/rocm/bin/rocm-smi --setfan 100 --device 0
```

- **Description**: Sets the GPU fan speed to a specific percentage.
- **Example**: `--setfan 100` sets the fan speed to 100% (full speed).
- **Device**: `--device 0` specifies the target GPU (device 0).
- **Real-World Reasons**:
  - **Overheating Prevention**: Increase fan speed during intensive tasks to prevent overheating.
  - **Noise Management**: Lower fan speed in less demanding scenarios to reduce noise in quiet environments.

### Setting GPU Memory Clock

To set the memory clock speed of a GPU:

```bash
sudo /opt/rocm/bin/rocm-smi --setmclk 2 --device 0
```

- **Description**: Sets the memory clock speed of the GPU to a specific level.
- **Example**: `--setmclk 2` sets the memory clock to level 2.
- **Device**: `--device 0` specifies the target GPU (device 0).
- **Real-World Reasons**:
  - **Performance Optimization**: Increase memory clock speed to boost performance in memory-intensive applications.
  - **Power Saving**: Reduce memory clock speed when high performance is not required to save energy.

### Setting GPU Performance Level

To set the performance level of a GPU:

```bash
sudo /opt/rocm/bin/rocm-smi --setperflevel high --device 0
```

- **Description**: Sets the GPU performance level.
- **Example**: `--setperflevel high` sets the performance level to high.
- **Device**: `--device 0` specifies the target GPU (device 0).
- **Real-World Reasons**:
  - **High Performance**: Set to high performance for demanding tasks like real-time rendering or complex simulations.
  - **Balanced Mode**: Use balanced performance levels to maintain a balance between performance and power consumption for typical workloads.

### Diagnostics and Debugging

**Checking GPU Health:**
```bash
/opt/rocm/bin/rocm-smi --showhealth
```
- **Summary:** This command is used to display the health status of the GPU. It provides information on various health metrics such as temperature, fan speed, power consumption, and more.

**Resetting GPU:**
```bash
sudo /opt/rocm/bin/rocm-smi --reset
```
- **Summary:** This command resets the GPU. It can be useful for troubleshooting and resolving issues related to the GPU by restarting its state.

**Saving GPU Logs:**
```bash
/opt/rocm/bin/rocm-smi --save log.txt
```
- **Summary:** This command saves the current GPU logs to a specified file (in this case, `log.txt`). These logs can be used for diagnostics and analyzing the performance and issues of the GPU.

**Clearing GPU Logs:**
```bash
sudo /opt/rocm/bin/rocm-smi --clearlog
```
- **Summary:** This command clears the GPU logs. It can help in managing disk space and removing old log data that is no longer needed for analysis.

### GPU Memory Management

**Checking GPU Memory Usage:**

```bash
/opt/rocm/bin/rocm-smi --showmemuse
```
- **Summary:** This command displays the current GPU memory usage, helping you monitor the memory allocation and identify any potential memory bottlenecks.

**Clearing GPU Memory:**

```bash
sudo /opt/rocm/bin/rocm-smi --clearmem
```
- **Summary:** This command clears the GPU memory, which can be useful for freeing up memory resources and resolving memory-related issues.

### Advanced GPU Configuration

**Overclocking GPU:**

```bash
sudo /opt/rocm/bin/rocm-smi --setsclkoc 1700 --device 0
```
- **Summary:** This command overclocks the GPU to a specified frequency (1700 MHz in this case), potentially improving performance but also increasing power consumption and heat.

**Underclocking GPU:**

```bash
sudo /opt/rocm/bin/rocm-smi --setsclkoc 1400 --device 0
```
- **Summary:** This command underclocks the GPU to a specified frequency (1400 MHz in this case), reducing power consumption and heat, which may be beneficial for stability and longevity.

**Setting Power Cap:**

```bash
sudo /opt/rocm/bin/rocm-smi --setpoweroverdrive 200 --device 0
```
- **Summary:** This command sets a power cap (200W in this case) on the GPU, limiting its maximum power consumption to control thermal output and power usage.

### ROCm-SMI Commands Summary

**General Information:**

```bash
/opt/rocm/bin/rocm-smi --showhw
/opt/rocm/bin/rocm-smi --showallinfo
```
- **Summary:** These commands provide detailed hardware information and all available information about the GPU, respectively, helping in understanding the system's configuration and capabilities.

**Temperature and Fan:**

```bash
/opt/rocm/bin/rocm-smi --showtemp
/opt/rocm/bin/rocm-smi --showfan
/opt/rocm/bin/rocm-smi --setfan <percentage> --device <device_id>
```
- **Summary:** These commands display the GPU temperature and fan speed, and allow setting the fan speed to a specific percentage, useful for thermal management.

**Power and Performance:**

```bash
/opt/rocm/bin/rocm-smi --showpower
/opt/rocm/bin/rocm-smi --setperflevel <level> --device <device_id>
/opt/rocm/bin/rocm-smi --setpoweroverdrive <value> --device <device_id>
```
- **Summary:** These commands display power consumption, set performance levels, and configure power overdrive settings, crucial for optimizing power efficiency and performance.

**Clock Speeds:**

```bash
/opt/rocm/bin/rocm-smi --showclk
/opt/rocm/bin/rocm-smi --setsclk <value> --device <device_id>
/opt/rocm/bin/rocm-smi --setmclk <value> --device <device_id>
/opt/rocm/bin/rocm-smi --setsclkoc <value> --device <device_id>
```
- **Summary:** These commands show and set the GPU's clock speeds, including overclocking and underclocking, to adjust performance and power consumption.

**Memory:**

```bash
/opt/rocm/bin/rocm-smi --showmemuse
/opt/rocm/bin/rocm-smi --clearmem
```
- **Summary:** These commands display current GPU memory usage and clear the GPU memory, aiding in memory management and troubleshooting.

**Logs and Health:**

```bash
/opt/rocm/bin/rocm-smi --showhealth
/opt/rocm/bin/rocm-smi --save <filename>
/opt/rocm/bin/rocm-smi --clearlog
```
- **Summary:** These commands show the health status of the GPU, save logs to a file, and clear existing logs, useful for diagnostics and maintenance.

**Reset:**

```bash
sudo /opt/rocm/bin/rocm-smi --reset
```
- **Summary:** This command resets the GPU, which can be helpful for recovering from errors and ensuring the GPU is in a clean state.



### RoCE (RDMA over Converged Ethernet) and GPU Network Fabrics

**Installing RDMA Tools:**

```bash
sudo apt install -y rdma-core
```

**Configuring RoCE:**

```bash
sudo modprobe mlx4_ib
sudo modprobe rdma_ucm
sudo modprobe ib_ipoib
```

**Checking RDMA Devices:**

```bash
ibv_devices
```

**Displaying RDMA Configuration:**

```bash
ibv_devinfo
```

**Setting Up RDMA Over TCP/IP:**

```bash
sudo rdma link add rxe0 type rxe netdev eth0
```

**Listing RDMA Interfaces:**

```bash
rdma link show
```

**Running RDMA Bandwidth Test:**

```bash
ib_send_bw -d mlx5_0 -i 1
```

**RDMA Latency Test:**
```bash
ib_send_lat -d mlx5_0 -i 1
```

**Connecting RDMA Devices:**
```bash
rdma resource show qp
rdma resource show pd
Using rping for RDMA Connectivity Testing:
```

**Server-side:**
```bash

Copy code
sudo rping -s -a <server_ip> -v -C 10
```

**Client-side:**
```bash
Copy code
sudo rping -c -a <server_ip> -v -C 10
```

**Configuring QoS for RDMA Traffic:**
```bash
sudo tc qdisc add dev eth0 root handle 1: htb default 10
sudo tc class add dev eth0 parent 1:1 classid 1:10 htb rate 10Gbit
sudo tc filter add dev eth0 protocol ip parent 1:0 prio 1 u32 match ip dport 4791 0xffff flowid 1:10
```

**Setting up RDMA Multicast:**
```bash
sudo ib_mcjoin -v <multicast_group>
```

**Monitoring RDMA Traffic:**
```bash
sudo perfquery -a
```

## Performance and Benchmarking Cookbook Using ROCm, RDMA, and RoCE

### Installing Required Tools

**Installing ROCm and Performance Tools:**

```bash
sudo apt update
sudo apt install -y rocm-dkms rocm-utils rocm-libs rocm-bandwidth-test rocprof rdma-core
```

### Basic Performance Testing

**Running ROCm Bandwidth Test:**

```bash
/opt/rocm/bin/rocm-bandwidth-test
```

**Interpreting Bandwidth Test Results:**

- The output will display various bandwidth metrics for device-to-device, host-to-device, and device-to-host transfers.
- Look for metrics like `Peak Bandwidth` to understand the data transfer capabilities of your system.

### Profiling Applications with rocprof

**Profiling a HIP Program:**

```bash
rocprof --hip-trace ./my_program
```

**Generating a Profile Report:**

```bash
rocprof --hsa-trace ./my_program
```

**Analyzing Profile Data:**

- Use `rocprof` output to analyze kernel execution times, memory transfer times, and other performance metrics.
- Look for bottlenecks in kernel execution or data transfers.

### Using rocminfo for System Information

**Checking System Information:**

```bash
/opt/rocm/bin/rocminfo
```

- This provides detailed information about the GPUs in your system, including device IDs, memory sizes, and supported features.

### RDMA and RoCE Performance Testing

**Configuring RoCE:**

```bash
sudo modprobe mlx4_ib
sudo modprobe rdma_ucm
sudo modprobe ib_ipoib
```

**Setting Up RDMA Over TCP/IP:**

```bash
sudo rdma link add rxe0 type rxe netdev eth0
sudo ip link set rxe0 up
```

**Checking RDMA Devices:**

```bash
ibv_devices
```

**Displaying RDMA Configuration:**

```bash
ibv_devinfo
```

**Running RDMA Bandwidth Test:**

```bash
ib_send_bw -d mlx5_0 -I 1
```

**RDMA Latency Test:**

```bash
ib_send_lat -d mlx5_0 -I 1
```

**Using `rping` for RDMA Connectivity Testing:**

**Server-side:**

```bash
sudo rping -s -a <server_ip> -v -C 10
```

**Client-side:**

```bash
sudo rping -c -a <server_ip> -v -C 10
```

**Configuring QoS for RDMA Traffic:**

```bash
sudo tc qdisc add dev eth0 root handle 1: htb default 10
sudo tc class add dev eth0 parent 1:1 classid 1:10 htb rate 10Gbit
sudo tc filter add dev eth0 protocol ip parent 1:0 prio 1 u32 match ip dport 4791 0xffff flowid 1:10
```

**Setting up RDMA Multicast:**

```bash
sudo ib_mcjoin -v <multicast_group>
```

**Monitoring RDMA Traffic:**

```bash
sudo perfquery -a
```

### Advanced Performance Benchmarking

**Using rocblas-bench for BLAS Performance:**

**Installing rocBLAS:**

```bash
sudo apt install -y rocblas
```

**Running rocblas-bench:**

```bash
rocblas-bench -f gemm -r f32_r -m 4096 -n 4096 -k 4096 --a_type f32_r --b_type f32_r --c_type f32_r --d_type f32_r --alpha 1 --beta 0
```

- This command benchmarks the General Matrix Multiply (GEMM) operation for single-precision floating-point numbers.

### Using rocFFT for FFT Performance

**Installing rocFFT:**

```bash
sudo apt install -y rocfft
```

**Running rocFFT Benchmarks:**

```bash
/opt/rocm/bin/rocfft-bench -n 1000 -b 2048 -p single -t complex_forward
```

- This command benchmarks the FFT operation for single-precision complex numbers.

### Using RVS (ROCm Validation Suite)

**Installing RVS:**

```bash
sudo apt install -y rocm-validation-suite
```

**Example Configuration for RVS Stress Test:**

**Creating `stress.json`:**

```json
{
  "stress": {
    "device": ["all"],
    "count": 1,
    "duration": 5000,
    "metrics": ["gpu_busy", "mem_busy"]
  }
}
```

**Running the Stress Test:**

```bash
sudo rvs -c stress.json
```

- This command runs a stress test on all available GPUs for 5000 milliseconds and measures GPU and memory busy metrics.

### Custom Performance Scripts

**Writing a Custom Benchmark Script:**

**Example:**

```bash
#!/bin/bash

# Set environment variables for ROCm
export ROCM_PATH=/opt/rocm
export PATH=$ROCM_PATH/bin:$PATH
export LD_LIBRARY_PATH=$ROCM_PATH/lib:$LD_LIBRARY_PATH

# Run rocminfo
echo "Running rocminfo..."
/opt/rocm/bin/rocminfo

# Run rocprof on a HIP program
echo "Profiling HIP program..."
rocprof --hip-trace ./my_program

# Run rocBLAS benchmark
echo "Running rocBLAS GEMM benchmark..."
rocblas-bench -f gemm -r f32_r -m 4096 -n 4096 -k 4096 --a_type f32_r --b_type f32_r --c_type f32_r --d_type f32_r --alpha 1 --beta 0

# Run rocFFT benchmark
echo "Running rocFFT benchmark..."
/opt/rocm/bin/rocfft-bench -n 1000 -b 2048 -p single -t complex_forward

# Configure RDMA
echo "Configuring RDMA..."
sudo modprobe mlx4_ib
sudo modprobe rdma_ucm
sudo modprobe ib_ipoib
sudo rdma link add rxe0 type rxe netdev eth0
sudo ip link set rxe0 up

# Run RDMA bandwidth test
echo "Running RDMA bandwidth test..."
ib_send_bw -d mlx5_0 -I 1

# Run RDMA latency test
echo "Running RDMA latency test..."
ib_send_lat -d mlx5_0 -I 1

# Run RVS stress test
echo "Running RVS stress test..."
sudo rvs -c stress.json
```

**Running the Custom Script:**

```bash
chmod +x custom_benchmark.sh
./custom_benchmark.sh
```

### Performance Optimization Tips

**Optimizing Kernel Execution:**

- Use the `__global__` and `__device__` qualifiers correctly to ensure that kernel functions are executed on the GPU.
- Optimize memory access patterns to maximize memory bandwidth utilization.
- Use shared memory and registers effectively to reduce global memory access latency.

**Optimizing Data Transfers:**

- Minimize data transfers between host and device to reduce overhead.
- Use pinned (page-locked) memory for faster data transfers between host and device.

**Analyzing Bottlenecks:**

- Use profiling tools like `rocprof` to identify bottlenecks in kernel execution and data transfers.
- Experiment with different kernel configurations and memory layouts to improve performance.

### Summary of Commands

**System Information:**

```bash
/opt/rocm/bin/rocminfo
```

**Bandwidth Test:**

```bash
/opt/rocm/bin/rocm-bandwidth-test
```

**Profiling:**

```bash
rocprof --hip-trace ./my_program
```

**rocBLAS Benchmark:**

```bash
rocblas-bench -f gemm -r f32_r -m 4096 -n 4096 -k 4096 --a_type f32_r --b_type f32_r --c_type f32_r --d_type f32_r --alpha 1 --beta 0
```

**rocFFT Benchmark:**

```bash
/opt/rocm/bin/rocfft-bench -n 1000 -b 2048 -p single -t complex_forward
```

**RVS Stress Test:**

```bash
sudo rvs -c stress.json
```

**RDMA Configuration:**

```bash
sudo modprobe mlx4_ib
sudo modprobe rdma_ucm
sudo modprobe ib_ipoib
sudo rdma link add rxe0 type rxe netdev eth0
sudo ip link set rxe0 up
```

**RDMA Bandwidth Test:**

```bash
ib_send_bw -d mlx5_0 -I 1
```

**RDMA Latency Test:**

```bash
ib_send_lat -d mlx5_0 -I 1
```

**RDMA Connectivity Testing with `rping`:**

**Server-side:**

```bash
sudo rping -s -a <server_ip> -v -C 10
```

**Client-side:**

```bash
sudo rping -c -a <server_ip> -v -C 10
```

