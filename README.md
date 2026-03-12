# pybind11 Hello Project

A simple pybind11 example that exposes a C++ `hello` function to Python.

## Prerequisites

* Python 3.7+
* CMake 3.12+
* C++ compiler (MSVC on Windows, GCC on Linux, Clang on macOS)
* pybind11 (will be installed as part of the build)
* scikit-build-core (build-backend)

## Project Structure

```
.
├── CMakeLists.txt       # CMake build configuration
├── pyproject.toml       # Python project configuration
└── src/
    ├── hello.cpp        # C++ implementation of hello function
    └── bindings.cpp     # pybind11 bindings
```

## Quick Start (for development environment)

### Create virtual environment

```bash
uv venv --python 3.14
```

That will create virtual environment `.venv` directory with Python 3.14.

### To activate virtual environment (on Linux)

```bash
source .venv/bin/activate
```

### To activate virtual environment (on Windows Bash)

```bash
source .venv/Scripts/activate
```

### To install dependencies

```bash
uv sync --group dev
```

### CMake

The cmake command needs to call pybind11, which was added to the virtual environment above. So you need to use the virtual environment like this:

```sh
cd build
uv run cmake ..
```

This step is essential, so that `uv build` below does not polute project root with Cmake build artifacts.

To build (shared library):

```bash
uv run cmake --build . --config Release
```

It will generate `build/Release/hello.cp314-win_amd64.pyd`.
`pip install -e .` below will use that.
But `uv build wheel` below will also build that internally.

### To build and install in development mode

```bash
cd <project-root>
uv pip install -e .
```

> **Note:** That will build with the `scikit-build-core` build-backend, and install the built module the virtual environment. Then you can try it next.

### Try it out:

```bash
python -c "from hello import hello; print(hello('world'))"
```

Or use a script:

```python
from hello import hello

print(hello("World"))
```

It should print out "Hello, world!".

### To uninstall dev mode:

To uninstall a package installed with `pip install -e .`, run:

```bash
uv pip uninstall pybind11-test
```

This removes the editable installation from your pixi environment.

## Release build

### To build wheel (whl) for distribution

```bash
cd <project-root>
uv build --wheel -o dist .
```

It will use `scikit-build-core` build backend to run CMake configure/generate/build internally.

It will create  a `.whl` file in the `dist` directory for distribution.

### To build source distribution package (sdist)

```bash
uv build --sdist -o dist .
```

### To build both wheel and source distribution:

You can also run `build` as a module:

```bash
uv run python -m build
```

This creates both a wheel (.whl) and source distribution (.tar.gz) in the `dist` directory.

## To build wheel for different Python version

```bash
uv build --wheel --python 3.12
```

That build wheel for Python 3.12.
