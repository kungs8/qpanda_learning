# 一、概述
QPanda是一种功能齐全，运行高效的量子软件开发工具包，作为一款开源的量子计算框架，它可以用于构建、运行和优化量子算法。 QPanda提供的功能有：
- **量子计算编程基础组件**
    QPanda提供了丰富的量子计算编程基础组件，包括量子比特和量子门操作。开发者可以使用这些组件来构建复杂的量子算法，从简单的量子门序列到更复杂的量子电路。这些基础组件为开发者提供了构建量子计算任务所需的核心工具。
- **高性能量子虚拟机**
    QPanda的高性能量子虚拟机允许开发者在经典计算机上进行高效的量子模拟。这种虚拟机能够快速模拟量子算法的执行过程，使开发者能够在不依赖实际量子硬件的情况下测试和优化他们的算法。
- **算法组件介绍**
    QPanda提供了多种量子算法组件的介绍，包括量子搜索、量子优化等。这些算法组件的介绍帮助开发者了解如何使用QPanda来实现不同类型的量子计算任务。通过这些介绍，开发者可以更快地上手并开始构建自己的量子算法。
- **量子芯片和高性能集群模拟**
    提供了本源高性能计算集群的模拟服务以及悟源真实芯片的量子计算服务。

pyqpanda是python版本的QPanda，通过pybind11工具，以一种直接和简明的方式，对QPanda2中的函数、类进行封装，并且提供了几乎完美的映射功能。 封装部分的代码在QPanda2编译时会生成为动态库，从而可以作为python的包引入。

# 二、系统配置和安装
为了兼容 高效与便捷，QPanda2提供了C++ 和 Python两个版本，本文中主要介绍python版本的使用。 如要了解和学习C++版本的使用请移步 [QPanda2](https://qpanda-tutorial.readthedocs.io/zh/latest/index.html)。

我们通过pybind11工具，以一种直接和简明的方式，对QPanda2中的函数、类进行封装，并且提供了几乎完美的映射功能。 封装部分的代码在QPanda2编译时会生成为动态库，从而可以作为python的包引入。
## 1. 系统配置
pyqpanda是以C++为宿主语言，其对系统的环境要求如下：
- Windows

    | software                                 |      version      |
    | :--------------------------------------- | :---------------: |
    | Microsoft Visual C++ Redistributable x64 |       2015        |
    | Python                                   | >= 3.8 && <= 3.11 |
    
- Linux

    | software |      version      |
    | :------- | :---------------: |
    | GCC      |      >=5.4.0      |
    | Python   | >= 3.8 && <= 3.11 |
## 2.下载pyqpanda
如果你已经安装好了python环境和pip工具， 在终端或者控制台输入下面命：
```
pip install pyqpanda
```
> 在linux下若遇到权限问题需要加 `sudo`

## 3. 测试

```python
from pyqpanda import *

def main():
    # 创建一个CPU计算的量子虚拟机
    qvm = CPUQVM()
    # 初始化量子虚拟机
    qvm.init_qvm()
    # 申请量子比特
    qubits = qvm.qAlloc_many(2)
    # 申请经典寄存器
    cbits = qvm.cAlloc_many(2)
    # 初始化一个量子程序
    prog = QProg()
    # 插入量子逻辑门
    prog << H(qubits[0]) \
         << CNOT(qubits[0], qubits[1]) \
         << meas_all(qubits, cbits)
    # 量子程序运行1000次，统计量子程序独立运行的测量结果
    result = qvm.run_with_configuration(prog, cbits, 1000)
    print(result)
    # 释放系统资源
    qvm.finalize()


if __name__ == '__main__':
    main()
```

输出：

```
{'00': 480, '11': 520}
```

> 1. **报错1:**
>
>    ```
>    ModuleNotFoundError: No module named 'requests'
>    ```
>
>    解决办法:
>
>    ```
>    pip install requests
>    ```
>
> 2. **报错2:**
>
>    ```
>    ImportError: cannot import name 'COMMON_SAFE_ASCII_CHARACTERS' from 'charset_normalizer.constant' (/usr/local/anaconda3/envs/originqc/lib/python3.10/site-packages/charset_normalizer/constant.py)
>    ```
>
>    解决办法:
>
>    ```
>    pip install --upgrade charset_normalizer
>    ```

# 三、量子操作

## 1. 量子逻辑门

经典计算中，最基本的单元是比特，而最基本的控制模式是逻辑门。我们可以通过逻辑门的组合来达到我们控制电路的目的。类似地，处理量子比特的方式就是量子逻辑门。 使用量子逻辑门，我们有意识的使量子态发生演化。所以量子逻辑门是构成量子算法的基础。

量子逻辑门由酉矩阵表示。最常见的量子门在一个或两个量子位的空间上工作，就像常见的经典逻辑门在一个或两个位上操作一样。

单比特的门可以用以下形式表达：

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$
其中 $$\alpha$$ 和 $\beta$ 是复数，在测量中，比特出现在$|0\rangle$的概率是$|\alpha|^2$,  $|1\rangle$ 的概率是$|\beta|^2$。作为向量表示为：
$$
|\psi\rangle = (\array{2 \\ 1})
$$


## 2. 量子线路

## 3. 量子程序

## 4. 量子测量

## 5. QIf

## 6. QWhile

## 7. 量子程序统计