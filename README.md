# qt5-windows-static

[![Build Qt5 Static Libraries](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml/badge.svg)](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml)

使用 GitHub Actions 自动编译打包 Qt5.15.2 的 Windows 静态库，用于加速其他项目的构建过程。

## 构建信息

- Qt: 5.15.2
- 系统: Windows
- 编译器: MSVC 2019
- 类型: 静态库（Static Library）
- 配置: Debug & Release

### 编译优化
- 多处理器编译 (-mp)
- 代码体积优化 (-optimize-size)
- 短路径优化 (避免 MSVC 长路径限制)

### 包含的功能和模块

#### 核心功能
- 可访问性支持 (Accessibility)
- 并发支持 (Concurrent)
- XML 支持
- SQL 和 SQLite 支持
- 网络功能
- OpenGL 支持
- 测试库支持 (TestLib)

#### 内置第三方库
- zlib (qt-zlib)
- libpng (qt-libpng)
- libjpeg (qt-libjpeg)
- freetype (qt-freetype)
- pcre (qt-pcre)
- harfbuzz (qt-harfbuzz)
- OpenSSL (linked)

#### Qt Quick 和 GUI 功能
- DBus 支持
- QML 调试
- Quick Designer 支持
- Windows 部署工具 (windeployqt)
- 语言工具 (Linguist)
- PDF 支持
- 打印机支持
- Quick Controls 2
- Quick Templates 2
- Widgets 支持
- Quick Widgets
- Quick Particles
- Shader Tools
- Vulkan 支持

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

之后可以直接使用 Qt5 的静态库：

```yaml
  - name: Build Your Project
    run: |
      cmake -B build -S . -DCMAKE_PREFIX_PATH=${{ env.Qt5_DIR }}
      cmake --build build --config Release
```

### 构建配置详情

完整的构建配置参数如下：

```batch
configure.bat -static -debug-and-release -platform win32-msvc ^
    -prefix "<install_path>" ^
    -opensource -confirm-license ^
    -nomake examples -nomake tests ^
    -skip qtwebengine ^
    -feature-accessibility -feature-concurrent ^
    -feature-xml -feature-sql -feature-sql-sqlite ^
    -feature-network -feature-opengl -feature-testlib ^
    -qt-zlib -qt-libpng -qt-libjpeg ^
    -qt-freetype -qt-pcre -qt-harfbuzz ^
    -openssl-linked ^
    -feature-dbus -feature-qml-debug ^
    -feature-quick-designer -feature-windeployqt ^
    -feature-linguist -feature-pdf -feature-printer ^
    -feature-quickcontrols2 -feature-quicktemplates2 ^
    -feature-widgets -feature-quickwidgets ^
    -feature-quickparticles -feature-shadertools ^
    -feature-vulkan
```

