# GUST: Graph Edge Coloring Utilization to Accelerate Sparse Matrix Vector Multiplication
This is the code-base for GUST (a software/hardware co-design for SpMV acceleration) with FPGA as the target hardware, and the paper can be found [here](https://arxiv.org/abs/2410.09106). There are 3 folders: **format convertor**, **pre-processing**, and **SpMV**.
1. **format convertor**: This folder includes a matlab file which turn the matrices stored as ".mat" format to a COO format in a ".txt" file. This folder also includes few matrices from SuitSparse.
2. **pre-processing**: The C++ code in this folder processes the input ".txt" file generated by the MATLAB code. It performs preprocessing on the data and outputs a new ".txt" file containing a matrix in the edge-coloring scheduled format.
   This code also prints the number of clock cycles ($n$) expected for the SpMV (To compare GUST with your own implementation, multiply this number by the clock duration to obtain the total SpMV execution time).
   <br/>The schduled format has $n$ rows, and each row has $2x+log_2(x)+1$ elements, where $x$ is GUST's length (default = 256). The first $x$ elements are the scheduled matrix elements, the second $x$ elements the scheduled vector elements, the next $log_2(x)$ the indices to control the crossbar connector, and the last element the dump signal.
   <br/> Remember to include the "-Ofast -m64 -funroll-loops -finline-functions" flags when compiling the CPP file.
4. **SpMV**: This folder contains all necessary files to create a Vivado project using the GUST Verilog codes. Once imported into Vivado, you can perform synthesis, analyze power consumption, evaluate worst negative slack, etc. The Verilog codes are designed for dynamic adjustment of GUST size and input variable size. For example, changing the GUST size from 256 to 64, or modifying the input variable size from 32 bits to 64 bits, requires altering only a single parameter in the parent Verilog file. However, if you modify the input variable size from the default 32 bits, you'll also need to update the addition and multiplication files, as these are specifically written for float32 inputs.

----

### Citation

If you use GUST in your research, please cite the following work:

```bibtex
@article{gerami2024gust,
  title={GUST: Graph Edge-Coloring Utilization for Accelerating Sparse Matrix Vector Multiplication},
  author={Gerami, Armin and Asgari, Bahar},
  journal={arXiv preprint arXiv:2410.09106},
  year={2024}
}
```
