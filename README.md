# NurbsFiller — 自研约束 NURBS 曲面填充 / 拟合工具

> **法律声明 (Legal Notice)**
>
> 本工程是一个**独立的开源实现**，仅基于公开的算法文献与开源参考
> （Piegl & Tiller《The NURBS Book》、Dierckx《Curve and Surface Fitting
> with Splines》、OpenCascade、NURBS-Python (geomdl)、TinySpline 等）。
>
> 本工程**没有对任何商业闭源软件进行反汇编、反编译或逆向工程**，所有
> 代码均为从公开算法理论自研。本仓库不包含 `xnkernel.dll` / `XNurbs.dll`
> 或任何其他 XNurbs 专有文件的代码、字节码、二进制副本、提取常量、符号或结构。
>
> 工程名 "NurbsFiller" 的灵感来自"约束曲面拟合 (constrained surface
> fitting)"这一公开算法研究领域，与任何商业产品无任何关联。

## 目标

实现一套**可独立编译、可单元测试、可命令行运行**的有约束 NURBS 曲面拟合
工具，覆盖以下算法能力：

1. B-样条基函数与 NURBS 曲线/曲面的求值与差分
2. 节点向量生成（均匀 / 节点平均法 chord length）
3. 曲线 / 曲面上的全局插值（interpolation）
4. 曲线 / 曲面上的最小二乘近似（least-squares approximation）
5. 加权最小二乘，支持三种硬约束：
   - 点约束（穿过指定 3D 点）
   - 法向约束（在指定点处切平面法向贴合）
   - 边界约束（指定四条 NURBS 曲线作为曲面边界）

## 工程结构

```
NurbsFiller/
├── CMakeLists.txt
├── README.md
├── LICENSE                       # MIT
├── cmake/                        # CMake 辅助模块
├── include/nurbs_filler/         # 公共头文件
│   ├── types.h
│   ├── knot_vector.h
│   ├── basis.h
│   ├── curve.h
│   ├── surface.h
│   └── fitting.h
├── src/                          # 实现
│   ├── knot_vector.cpp
│   ├── basis.cpp
│   ├── curve.cpp
│   ├── surface.cpp
│   └── fitting.cpp
├── tests/                        # GoogleTest 单元测试
│   ├── test_knot_vector.cpp
│   ├── test_basis.cpp
│   ├── test_curve.cpp
│   ├── test_surface.cpp
│   └── test_fitting.cpp
├── app/                          # 命令行工具
│   └── main.cpp
└── examples/                     # 示例输入与期望输出
```

## 依赖

- C++17 编译器（MSVC 2019+ / GCC 12+ / Clang 14+）
- CMake 3.16+
- Eigen 3.4+（FetchContent 自动拉取）
- GoogleTest（FetchContent 自动拉取，仅测试时需要）

## 编译

```bash
cmake -S . -B build -G Ninja          # 或 "Visual Studio 17 2022"
cmake --build build -j
ctest --test-dir build --output-on-failure
```

## 运行 CLI

```bash
./build/app/nurbsfiller examples/torus.csv --degree 3 --nu 12 --nv 24 --out torus.obj
```

## 参考文献 / 教程（合法学习资源）

1. Piegl L., Tiller W. *The NURBS Book* (2nd ed.), Springer, 1997.
2. Dierckx P. *Curve and Surface Fitting with Splines*, Oxford, 1993.
3. The NURBS-Python (geomdl) project: https://github.com/orbingol/NURBS-Python
4. OpenCascade 文档: https://dev.opencascade.org/doc/occt-7.7.0/overview/html/
5. tinygltf / libigl 论坛与课件
6. MIT OCW 6.838 *Shape Analysis* (Solomon)

## 许可证

MIT。详见 `LICENSE`。