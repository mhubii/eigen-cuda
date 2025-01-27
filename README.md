# eigen-cuda
Using the Eigen library in CUDA kernels

[Eigen](http://eigen.tuxfamily.org/) is a fantastic library, which since version 3.3 can be used inside CUDA kernels.
**This MWE** shows the calculation of a sum of dot products using a `std::vector<Eigen::Vector3d>`.
The `USE_CUDA` flag switches from the regular C++ (CPU) implementation to the CUDA implementation.

## Usage
Clone this repository, then
```shell
cmake -E make_directory build
cmake . -B build -DUSE_CUDA=OFF # or -DUSE_CUDA=ON
cmake --build build
```
To run an example, do
```shell
.build/main
```