
<!-- vim-markdown-toc Marked -->

+ [openOCD](#openocd)
    * [安装编译依赖](#安装编译依赖)
    * [获取`OpenOCD`源代码](#获取`openocd`源代码)
    * [配置编译选项](#配置编译选项)
    * [编译和安装](#编译和安装)
    * [配置 udev 规则（让普通用户能访问 USB 设备）](#配置-udev-规则（让普通用户能访问-usb-设备）)
    * [验证安装和 ST-Link 支持](#验证安装和-st-link-支持)

<!-- vim-markdown-toc -->

# openOCD

## 安装编译依赖

```shell
# 更新系统包列表
sudo apt update

# 安装基本编译工具
sudo apt install -y build-essential autoconf automake texinfo libtool pkg-config libjim0.81 libjaylink-dev

# 安装 USB 和硬件访问库
sudo apt install -y libusb-1.0-0-dev libhidapi-dev libftdi1-dev

# 安装其他可能的依赖
sudo apt install -y libusb-0.1-4 libusb-1.0-0 libhidapi-hidraw0 libftdi1-2
```

## 获取`OpenOCD`源代码

```shell
# 克隆 OpenOCD 官方仓库（推荐使用官方源）
git clone https://git.code.sf.net/p/openocd/code openocd
cd openocd
```

## 配置编译选项

```shell
# 运行 bootstrap 脚本生成配置
./bootstrap

# 配置编译选项，特别启用 ST-Link 相关驱动
./configure \
  --enable-stlink \
  --enable-maintainer-mode \
  --enable-ftdi \
  --enable-jlink \
  --enable-cmsis-dap \
  --enable-usb-blaster-2 \
  --prefix=/usr/local

# 或者使用简化的配置命令，确保包含 ST-Link
./configure --enable-stlink --prefix=/usr/local
```

## 编译和安装

```shell
# 编译（使用多核加速编译）
make -j$(nproc)

# 安装到系统
sudo make install

# 更新动态库缓存
sudo ldconfig
```

## 配置 udev 规则（让普通用户能访问 USB 设备）

```shell
# 创建 udev 规则文件
sudo tee /etc/udev/rules.d/99-openocd.rules > /dev/null << 'EOF'
# ST-Link/V2 and V2-1
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="3748", MODE="666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", MODE="666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374d", MODE="666"

# ST-Link/V3
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374e", MODE="666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374f", MODE="666"

# ST-Link/V3 with DFU
SUBSYSTEM=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE="666"

# 其他常见调试器
SUBSYSTEM=="usb", ATTRS{idVendor}=="1366", MODE="666"  # SEGGER J-Link
SUBSYSTEM=="usb", ATTRS{idVendor}=="0d28", MODE="666"  # ARM CMSIS-DAP
SUBSYSTEM=="usb", ATTRS{idVendor}=="0403", MODE="666"  # FTDI
EOF

# 重新加载 udev 规则
sudo udevadm control --reload-rules
sudo udevadm trigger
```

## 验证安装和 ST-Link 支持

```shell
# 检查 OpenOCD 版本
openocd --version

# 查看支持的接口（确认 stlink 在列表中）
openocd --list-adapters | grep -i stlink

# 或者运行详细帮助查看支持的接口
openocd --help | grep -A 20 "Supported Interfaces"
```





