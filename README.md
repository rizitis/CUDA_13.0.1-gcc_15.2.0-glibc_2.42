# TAKE BACKUP!!! BEFORE DO ANYTHING... OR BE READY TO REINSTALL SLACKWARE
## Do You like to live dangerously? then continue else stop now **(suggested to stop now)**

Build a CUDA-enabled project (like llama.cpp) on Slackware64 Current with (GCC) 15.2.0, glibc-2.42 and CUDA 13.0.1 (for fedora) toolkit. (19/09/2025)

1. DOWNLOAD fedora installer: `wget https://developer.download.nvidia.com/compute/cuda/13.0.1/local_installers/cuda_13.0.1_580.82.07_linux.run`

2. run installer: `sh cuda_13.0.1_580.82.07_linux.run  --override`

3. Patch files:
```
# Fix rsqrt and rsqrtf in system headers
sed -i 's/\(extern double rsqrt (double __x)\) noexcept (true);/\1;/' /usr/include/bits/mathcalls.h
sed -i 's/\(extern float rsqrtf (float __x)\) noexcept (true);/\1;/' /usr/include/bits/mathcalls.h

# Fix __rsqrt and __rsqrtf in CUDA headers
sed -i 's/\(extern double __rsqrt (double __x)\) noexcept (true);/\1;/' /usr/local/cuda-13.0/targets/x86_64-linux/include/crt/math_functions.h
sed -i 's/\(extern float __rsqrtf (float __x)\) noexcept (true);/\1;/' /usr/local/cuda-13.0/targets/x86_64-linux/include/crt/math_functions.h

# Globally remove all 'noexcept (true)' in headers (optional, broader fix)
sed -i 's/noexcept (true)//g' /usr/include/**/*.h
sed -i 's/noexcept (true)//g' /usr/local/cuda-13.0/targets/x86_64-linux/include/**/*.h
```

4. cd to you llama.cpp clone (assume its updated...) and build:
 - `cmake -B build-cuda -DGGML_CUDA=ON`
 - `cmake --build build-cuda --config Release`

 5. You said np to live dangerously?
    
```
  slackpkg reinstall \
  aaa_glibc-solibs \
  glibc \
  linux-headers \
  gcc \
  llvm
  ```
