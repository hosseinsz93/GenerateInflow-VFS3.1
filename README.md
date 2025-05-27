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

## References


## License



## Contact

For any questions or suggestions, reach out via GitHub issues or email at hossein.seyyedzadeh@stonybrook.edu.
