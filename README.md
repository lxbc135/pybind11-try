# pybind11 Hello Project

A simple pybind11 example that exposes a C++ `hello` function to Python.

## Building

### Prerequisites

- Python 3.7+
- CMake 3.12+
- C++ compiler (MSVC on Windows, GCC on Linux, Clang on macOS)
- pybind11 (will be installed as part of the build)

### Build Instructions

#### Setup Pixi

```bash
pixi init
```

#### Install Dependencies

```sh
pixi add pybind11
pixi add scikit-build-core
```

#### Using CMake directly

```bash
mkdir build
cd build
cmake ..
cmake --build . --config Release
```

The cmake command needs to call pybind11, which was added to the virtual environment above. So you need to use the pixi virtual environment like this:

```sh
cd build
pixi run cmake ..
pixi run cmake --build . --config Release
```

It will generate `build/Release/hello.cp314-win_amd64.pyd`

#### To Install in Development Mode

```bash
cd <project-root>
pixi run pip install -e .
```

That creates a module called `hello-module`, and installs it in your virtual environment. You can import it in python, as described below.

## Try It Out:

```bash
pixi run python -c "import hello; print(hello.hello('world'))"
```

Or use the script below:

```python
import hello

# Call the hello function with a name
result = hello.hello("World")
print(result)  # Output: Hello, World!
```

#### Uninstall Dev Mode:

To uninstall a package installed with `pip install -e .`, run:

```bash
pixi run pip uninstall hello-module
```

This removes the editable installation from your pixi environment.

#### To Build for Distribution

```bash
pixi run pip wheel . --no-deps -w dist
```

It will creat  a `.whl` file in the `dist` directory without installing the package.

The --no-deps flag ensures it only builds your package without trying to build dependencies.

Alternatively, if you want to use the standard python -m build approach, first install the build package:

```bash
pixi run pip install build
pixi run python -m build
```

This creates both a wheel (.whl) and source distribution (.tar.gz) in the `dist/` directory. This does not install the package in the virtual environment like `pip install -e .`.

## Project Structure

```
.
├── CMakeLists.txt       # CMake build configuration
├── pyproject.toml       # Python project configuration
├── src/
│   ├── hello.cpp        # C++ implementation of hello function
│   └── bindings.cpp     # pybind11 bindings
└── example.py           # Example usage script
```

## Notes

- The `hello` function takes a single parameter `name` (string) and returns a greeting string
- The module is compiled as a shared library that Python can import as `hello`
