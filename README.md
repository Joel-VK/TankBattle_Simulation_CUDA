# TankBattle_Simulation_CUDA



## **Table of Contents**

1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Compilation and Execution](#compilation-and-execution)
4. [Input and Output](#input-and-output)
5. [CUDA Kernels](#cuda-kernels)
6. [Performance Considerations](#performance-considerations)

## **Project Overview**

This project implements a tank battle simulation using CUDA for parallel processing. The simulation models a scenario where multiple tanks, each with a starting health, engage in combat based on their coordinates on a grid. The simulation continues until only one tank is left standing. The goal of this project is to demonstrate the use of CUDA to parallelize the computationally intensive task of simulating battles between tanks.

## **Prerequisites**

- **CUDA Toolkit**: Ensure that CUDA is installed on your system.
- **NVIDIA GPU**: A compatible GPU is required to run the CUDA code.
- **C++ Compiler**: A compiler like `g++` that supports C++11 or later.

## **Compilation and Execution**

### **Compilation**

To compile the code, use the following command:

```bash
nvcc tank_simulation.cu -o tank_simulation
```

### **Execution**

To run the compiled program:

```bash
./tank_simulation <input_file> <output_file> <execution_time_file>
```

- `<input_file>`: Path to the input file containing the simulation parameters and tank coordinates.
- `<output_file>`: Path to the file where the tank scores will be saved after the simulation.
- `<execution_time_file>`: Path to the file where the execution time of the simulation will be recorded.

## **Input and Output**

### **Input File Format**

The input file should have the following format:

1. **M N T H**: The first line contains four integers:
    - `M`: Number of rows in the grid.
    - `N`: Number of columns in the grid.
    - `T`: Number of tanks.
    - `H`: Initial health points of each tank.

2. **Coordinates**: The next `T` lines contain two integers each, representing the X and Y coordinates of each tank on the grid.

### **Output File Format**

The output file will contain `T` lines, each with a single integer representing the score of each tank (i.e., the number of successful attacks).

### **Execution Time File**

This file contains a single floating-point value representing the time taken to execute the simulation in microseconds.

## **CUDA Kernels**

Three main CUDA kernels are used in this project:

1. **`initialise`**: Initializes the scores, health points, and other variables.
2. **`simulateFight`**: Simulates the battle for each tank, determining which tank each one attacks based on the Manhattan distance.
3. **`checkActive`**: Updates the health points after each round and checks how many tanks are still active.

These kernels work together to simulate the tank battle in a parallel manner, leveraging the computational power of the GPU.

## **Performance Considerations**

- **Memory Allocation**: CUDA `cudaMalloc` and `cudaFree` are used to allocate and deallocate memory on the GPU. `cudaHostAlloc` is used for pinned memory, which improves the performance of memory transfers between the host and device.
- **Synchronization**: Proper synchronization is enforced using `__syncthreads()` to ensure that shared memory operations are correctly ordered.
- **Atomic Operations**: Atomic operations are used to prevent race conditions when updating shared variables like scores and health points.

