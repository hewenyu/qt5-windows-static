# Qt5 Windows Static Action

[![Build Qt5 Static Libraries](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml/badge.svg)](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml)

这是一个 GitHub Action，用于在 Windows 环境中提供预编译的 Qt5 静态库。它使用 vcpkg 管理依赖，提供了一个完整的 Qt5 静态库环境，特别适合需要在 Windows 下进行静态编译的 Qt 项目。

## 特性

- ✨ 基于 vcpkg 的依赖管理
- 🔨 MSVC 2019 静态编译
- 📦 预编译的静态库
- 🔄 自动依赖处理
- 🎯 支持 Debug 和 Release 配置

## 包含的 Qt 组件

| 组件 | 描述 | 用途 |
|------|------|------|
| qt5-base | 核心组件 | Qt 基础功能 |
| qt5-multimedia | 多媒体支持 | 音视频处理 |
| qt5-tools | 开发工具 | 开发辅助工具 |
| qt5-declarative | QML 支持 | 界面开发 |
| qt5-activeqt | ActiveX 支持 | Windows 集成 |
| qt5-imageformats | 图像格式支持 | 图像处理 |
| qt5-svg | SVG 支持 | 矢量图形 |
| qt5-charts | 图表支持 | 数据可视化 |
| qt5-networkauth | 网络认证 | 网络功能 |
| qt5-winextras | Windows 特性支持 | Windows 平台特性 |

## 使用方法

### 基本用法

在你的 GitHub Actions 工作流程中添加：

```yaml
steps:
  - name: Setup Qt5 Static Environment
    uses: hewenyu/qt5-windows-static@v5.15
    with:
      qt-version: '5.15.2'      # 可选，默认为 5.15.2
      install-dir: '${{ github.workspace }}/qt5-static'  # 可选，默认路径
```

### 完整工作流示例

```yaml
name: Build with Qt Static

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: windows-2019
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Qt5 Static Environment
      uses: hewenyu/qt5-windows-static@v5.15
      
    - name: Setup MSVC
      uses: microsoft/setup-msbuild@v1.1
      
    - name: Build Project
      run: |
        cmake -B build -S . -DCMAKE_PREFIX_PATH=${{ env.Qt5_DIR }}
        cmake --build build --config Release
```

### CMake 集成

在你的 CMakeLists.txt 中：

```cmake
cmake_minimum_required(VERSION 3.15)
project(YourProject)

# 设置 Qt 环境
set(CMAKE_PREFIX_PATH "$ENV{Qt5_DIR}")
set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
set(VCPKG_TARGET_TRIPLET "x64-windows-static")

# 查找 Qt 组件
find_package(Qt5 COMPONENTS 
    Core
    Widgets
    # 其他需要的组件...
    REQUIRED
)

# 设置静态编译
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# 添加可执行文件
add_executable(${PROJECT_NAME} main.cpp)

# 链接 Qt 库
target_link_libraries(${PROJECT_NAME} PRIVATE
    Qt5::Core
    Qt5::Widgets
    # 其他需要的库...
)
```

## 输入参数

| 参数 | 描述 | 必需 | 默认值 |
|------|------|------|---------|
| qt-version | Qt 版本 | 否 | 5.15.2 |
| install-dir | 安装目录 | 否 | ${{ github.workspace }}/qt5-static |

## 输出变量

| 变量 | 描述 |
|------|------|
| qt5-dir | Qt5 安装目录的路径 |

## 环境变量

Action 会自动设置以下环境变量：

- `Qt5_DIR`: Qt5 安装目录
- `CMAKE_PREFIX_PATH`: CMake 查找包的路径

## 系统要求

- Windows 环境
- MSVC 2019 或更高版本
- CMake 3.15 或更高版本（如果使用 CMake 构建）

## 许可证

MIT License

## 贡献

欢迎提交 Issues 和 Pull Requests！

## 支持

如果遇到问题，请提交 Issue。

