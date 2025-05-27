# GenerateInflow-VFS3.1

A computational fluid dynamics (CFD) tool for generating synthetic turbulent inflow conditions for Large Eddy Simulation (LES) using the von Kármán energy spectrum and FFTW-based synthetic turbulence generation.

## Overview

This project generates inflow boundary conditions for CFD simulations, particularly for atmospheric boundary layer flows and turbulent channel flows. It implements synthetic turbulence generation using spectral methods and provides various inlet velocity profiles including logarithmic law-of-the-wall profiles.

## Features

- **Synthetic Turbulence Generation**: Uses von Kármán energy spectrum with FFTW for generating divergence-free turbulent velocity fields
- **Multiple Inlet Profiles**: 
  - Uniform flow
  - Logarithmic boundary layer profiles
  - Power law boundary layer profiles
  - Constant shear profiles
  - Custom mean velocity profiles from data files
- **Temperature Field Generation**: Support for multiple temperature scalars with localized heating sources
- **Grid Generation**: Tools for creating structured grids in XYZ format
- **Tecplot Output**: Direct output to Tecplot format for visualization
- **Parallel Processing**: Built with PETSc for parallel execution

## Files Description

- [`GenerateInflow.c`](GenerateInflow.c) - Main C++ source code implementing the inflow generation algorithm
- [`GenerateInflow.ipynb`](GenerateInflow.ipynb) - Jupyter notebook with Python scripts for grid generation
- [`xyz.dat`](xyz.dat) - Generated grid file containing coordinate information (397×397 points)
- [`README.md`](README.md) - This documentation file

## Dependencies

- **PETSc**: Portable, Extensible Toolkit for Scientific Computation
- **FFTW3**: Fast Fourier Transform library
- **TECIO** (optional): Tecplot I/O library for direct Tecplot output
- **C++ Standard Library**

## Compilation

The code requires linking with PETSc and FFTW3 libraries:

```bash
# Example compilation command (adjust paths as needed)
mpicc -o GenerateInflow GenerateInflow.c -I$PETSC_DIR/include -L$PETSC_DIR/lib -lpetsc -lfftw3 -lm
```

## Usage

### Grid Generation

Use the Python scripts in [`GenerateInflow.ipynb`](GenerateInflow.ipynb) to generate grid files:

```python
# Generate 397x397 grid with domain size 2.2x2.2
Nx = 397
Ny = 397  
Lx = 2.2
Ly = 2.2
```

### Running the Inflow Generator

```bash
# Run with default parameters
./GenerateInflow

# Run with custom input file
./GenerateInflow -options_file GenerateInflow.inp
```

## Key Parameters

The code accepts numerous command-line parameters. Key ones include:

### Grid Parameters
- `-Nx`, `-Ny`, `-Nz`: Grid points for synthetic turbulence generation
- `-Lx`, `-Ly`, `-Lz`: Domain lengths

### Turbulence Parameters
- `-ustar`: Friction velocity
- `-L_coef`: Length scale coefficient (default: 0.59)
- `-D_coef`: Dissipation coefficient (default: 3.2)
- `-Gamma`: Shear parameter for boundary layer flows

### Flow Parameters
- `-InletProfile`: Type of inlet profile (1-7)
- `-Ue`: Edge velocity for boundary layer
- `-h_bl`: Boundary layer thickness
- `-alfa_bl`: Power law exponent

### Time Integration
- `-Nt_LES`: Number of time steps
- `-dt_LES`: Time step size
- `-save_inflow_period`: Output frequency

## Inlet Profile Types

1. **Uniform Flow**: Constant velocity field
2. **Logarithmic Profile**: `u = (u*/κ) × ln(y/z0)` where κ is von Kármán constant
3. **Data-based Profile**: Read mean profiles from `Wavg_In.dat` and `Tavg_In.dat`
4. **Linear Shear**: `u = U0 + shear_rate × y`
5. **Power Law**: `u = Ue × (y/δ)^α`
6. **Logarithmic with Temperature**: Log profile plus temperature sources
7. **Prescribed Field**: Read from Tecplot slice file

## Output Files

- `inflow_XXXXXX_dt=X.dat`: Binary velocity field files
- `inflow_t_XXXXXX_dt=X.dat`: Temperature field files
- `inflowcheck_XXXXXX_dt=X.plt`: Tecplot visualization files
- `inflow_avg.plt`: Time-averaged statistics
- `E_k.dat`: Energy spectrum data

## Theory

The synthetic turbulence generation is based on:

1. **von Kármán Energy Spectrum**: 
   ```
   E(k) = α ε^(2/3) L^(5/3) × (L^4 k^4) / (1 + L^2 k^2)^(17/6)
   ```

2. **Divergence-free Condition**: Enforced through proper construction of Fourier coefficients

3. **Convection**: Turbulent structures are convected downstream using Taylor's frozen turbulence hypothesis

## Example Configuration

```bash
# Logarithmic boundary layer with synthetic turbulence
./GenerateInflow \
  -InletProfile 2 \
  -ustar 0.115 \
  -z0 0.0000148 \
  -Turb 1 \
  -Nx 100 -Ny 100 -Nz 100 \
  -Lx 1.2 -Ly 1.2 -Lz 1.2 \
  -Nt_LES 1200 \
  -dt_LES 0.01
```

## References

This implementation is based on synthetic turbulence generation methods commonly used in computational fluid dynamics for LES simulations, particularly for atmospheric boundary layer modeling.

## License

[Add your license information here]

## Contact

[Add contact information here]