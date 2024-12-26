# qt5-windows-static

[![Build Qt5 Static Libraries](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml/badge.svg)](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml)

使用 GitHub Actions 和 vcpkg 打包 Qt5.15.2 的 Windows 静态库，用于加速其他项目的构建过程。

## 构建信息

- Qt: 5.15.2
- 系统: Windows
- 编译器: MSVC 2019
- 类型: 静态库（Static Library）
- 配置: Debug & Release
- 依赖管理: vcpkg

### 包含的 Qt 组件
- qt5-base (核心组件)
- qt5-multimedia (多媒体支持)
- qt5-tools (开发工具)
- qt5-declarative (QML 支持)
- qt5-activeqt (ActiveX 支持)
- qt5-imageformats (图像格式支持)
- qt5-svg (SVG 支持)
- qt5-charts (图表支持)
- qt5-networkauth (网络认证)
- qt5-winextras (Windows 特性支持)

### 功能特性
- 完整的静态库支持
- MSVC 2019 编译
- vcpkg 包管理
- Debug 和 Release 配置
- 自动依赖处理

## 使用方法

### 在 GitHub Actions 中使用

```yaml
steps:
  - name: Setup Qt5 Static Environment
    uses: hewenyu/qt5-windows-static@v5.15
    with:
      qt-version: '5.15.2'      # 可选，默认为 5.15.2
      install-dir: '${{ github.workspace }}/qt5-static'  # 可选，默认为此路径
```

### 在本地使用 vcpkg 安装

如果你想在本地复制这个环境，可以使用以下命令：

```batch
vcpkg install ^
    qt5-base:x64-windows-static ^
    qt5-multimedia:x64-windows-static ^
    qt5-tools:x64-windows-static ^
    qt5-declarative:x64-windows-static ^
    qt5-activeqt:x64-windows-static ^
    qt5-imageformats:x64-windows-static ^
    qt5-svg:x64-windows-static ^
    qt5-charts:x64-windows-static ^
    qt5-networkauth:x64-windows-static ^
    qt5-winextras:x64-windows-static
```

### 在 CMake 项目中使用

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.15)
project(YourProject)

set(CMAKE_PREFIX_PATH "$ENV{Qt5_DIR}")
set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
set(VCPKG_TARGET_TRIPLET "x64-windows-static")

find_package(Qt5 COMPONENTS Core Widgets REQUIRED)
# ... 其他配置
```

然后在 GitHub Actions 中构建：

```yaml
  - name: Build Your Project
    run: |
      cmake -B build -S . -DCMAKE_PREFIX_PATH=${{ env.Qt5_DIR }}
      cmake --build build --config Release
```

