# Bating-Platform.Tools
<img width="965" height="641" alt="Untitled5_20261002210321" src="https://github.com/user-attachments/assets/78f39e35-aab9-4474-a51e-b00197400118" />

Credits: Google
An easy-to-find Platform-Tools repository
## Building and Installing Standard scrcpy Win32 (32-bit)

This guide builds the original, unpatched scrcpy v5.0.1 for Windows 32-bit and installs it into Bating-Platform.Tools.

### 1. Download clean upstream source

Download scrcpy v5.0.1:

[https://github.com/Genymobile/scrcpy/archive/refs/tags/v5.0.1.zip](https://github.com/Genymobile/scrcpy/archive/refs/tags/v5.0.1.zip)

Extract to:

```text
D:\Bating Tools SAVED\scrcpy-5.0.1
```

Keep an untouched copy of the original source.

### 2. Prepare the 32-bit build environment

Use a compatible 32-bit MinGW-w64 toolchain and matching 32-bit dependencies. Do not use the UCRT64 compiler for this build because it produces 64-bit binaries.

Open the appropriate 32-bit build terminal and install the required compiler, Meson, Ninja, SDL3, FFmpeg, and libusb development dependencies.

### 3. Enter the source directory

```bash
cd "/d/Bating Tools SAVED/scrcpy-5.0.1"
```

### 4. Configure the Win32 build

Use the 32-bit compiler and dependency paths:

```bash
meson setup build-win32 --buildtype=release --strip
```

### 5. Compile

```bash
ninja -C build-win32
```

Expected outputs:

```text
build-win32/app/scrcpy.exe
build-win32/server/scrcpy-server
```

### 6. Prepare the Win32 package

```bash
stage="/d/Bating Tools SAVED/scrcpy-win32"

mkdir -p "$stage"

cp -f build-win32/app/scrcpy.exe "$stage/"
cp -f build-win32/server/scrcpy-server "$stage/"
```

Copy all required 32-bit runtime DLLs from the matching toolchain into the package directory.

Check dependencies:

```bash
ldd build-win32/app/scrcpy.exe
```

### 7. Validate the package

```bash
"$stage/scrcpy.exe" --version
```

Check the required files:

```bash
test -f "$stage/scrcpy.exe" && echo "Win32 executable: OK"
test -f "$stage/scrcpy-server" && echo "scrcpy server: OK"
```

### 8. Install into Bating-Platform.Tools

**Bating Tools installation directory:**

```text
D:\Program Files\batingtools or C:\Program Files\batingtools
```

Copy the completed Win32 package into Bating Tools using an elevated Windows Command Prompt:

```bat
robocopy "D:\Bating Tools SAVED\scrcpy-win32" "D:\Program Files\batingtools\scrcpy-win32" /E
```

The final directory structure should be:

```text
D:\Program Files\batingtools\ [or C:\Program Files\batingtools\
│
├── platfrom.bat
├── batingtools.exe
│
├── scrcpy-win32\
│   ├── scrcpy.exe
│   ├── scrcpy-server
│   └── required DLL files
│
├── scrcpy-win64\
│
└── bating-scrcpy-patched-win64\
```

### Architecture support

| Package                       | Build type              |
| ----------------------------- | ----------------------- |
| `scrcpy-win32`                | Standard upstream Win32 |
| `scrcpy-win64`                | Standard upstream Win64 |
| `bating-scrcpy-patched-win64` | Patched Win64           |


### Official references

- Source: [https://github.com/Genymobile/scrcpy](https://github.com/Genymobile/scrcpy)
- Build documentation: [https://github.com/Genymobile/scrcpy/blob/v5.0.1/doc/build.md](https://github.com/Genymobile/scrcpy/blob/v5.0.1/doc/build.md)
- Windows documentation: [https://github.com/Genymobile/scrcpy/blob/v5.0.1/doc/windows.md](https://github.com/Genymobile/scrcpy/blob/v5.0.1/doc/windows.md)
