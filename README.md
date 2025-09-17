# MyApp

A cross-platform GUI application using **GLFW + GLAD + Dear ImGui**.

## Features
- Lightweight, runs on low-end hardware
- Modern CMake build system
- Clean Core/App architecture

## Requiremets
- CMake 3.3+

## Build Instructions Linux

```bash
git clone https://github.com/yourname/my_app.git
cd my_app
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . --config Release
./my_app    # or my_app.exe on Windows
```
