# qt5-windows-static

[![Build Qt5 Static Libraries](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml/badge.svg)](https://github.com/hewenyu/qt5-windows-static/actions/workflows/build-qt5-static.yml)

使用 GitHub Actions 自动编译打包 Qt5.15.2 的 Windows 静态库，用于加速其他项目的构建过程。

## 构建信息

- Qt: 5.15.2
- 系统: Windows
- 编译器: MSVC 2019
- 类型: 静态库（Static Library）
- 配置: Debug & Release

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

### 在本地项目中使用

1. 从 [Actions](https://github.com/hewenyu/qt5-windows-static/actions) 下载最新构建产物
2. 解压到本地目录
3. 设置 `Qt5_DIR` 环境变量指向解压目录

## 许可证

MIT License (Qt 许可证条款另行遵循)

