# BH1750FVI 旧版本软件包

## 1 介绍

`BH1750FVI` 旧版本软件包提供了使用光照强度传感器 `BH1750FVI` 基本功能，并且提供了软件平均数滤波器可选功能 。本文介绍该软件包的基本功能，以及 `Finsh/MSH` 测试命令等。

基本功能主要由传感器 `BH1750FVI` 决定：在输入电压为 `2.4v-3.6v` 范围内，测量光照强度的量程如下表所示：

| 功能 | 量程          |
| :------: |:------: |
| 光照强度 | 1lx - 65535lx |

`BH1750FVI` 的分辨率以及测量时间都与测量模式有关，具体如下表所示：

| 测量模式| 测量时间 | 分辨率 |
| :------:|:------: | :------: |
| H-resolution Mode2| Typ. 120ms.|  0.5 lx |
| H-resolution Mode | Typ. 120ms.| 1 lx |
| L-resolution Mode | Typ. 16ms. | 4 lx |

### 1.1 目录结构

| 名称 | 说明 |
| ---- | ---- |
| bh1750.h | 传感器使用头文件 |
| bh1750.c | 传感器使用源代码 |
| SConscript | RT-Thread 默认的构建脚本 |
| README.md | 软件包使用说明 |
| BH1750FVI_datasheet.pdf| 官方数据手册 |

### 1.2 许可证

BH1750FVI 软件包遵循  Apache-2.0 许可，详见 LICENSE 文件。

### 1.3 依赖

依赖 `RT-Thread I2C` 设备驱动框架。

## 2 获取旧版本软件包

使用 `BH1750FVI` 软件包需要在 RT-Thread 的包管理器中选择它，具体路径如下：

```
RT-Thread online packages --->
    peripheral libraries and drivers --->
        sensors drivers --->
              [*]   bh1750 sensor driver package, support: ambient light.  --->
                   	Version (v1.0.0)  --->
        [*] Enable average filter by software                                    
        (10) The number of averaging
        (1000) Peroid of sampling data(unit ms) 
```

每个功能的配置说明如下：

- `Enable average filter by software`：开启采集光照强度软件平均数滤波器功能；
- `The number of averaging`：取平均数的采样数目；
- `Peroid of sampling data(unit ms)`：采集数据的周期，时间单位 `ms`；
- `Version`：配置软件包版本，默认最新版本。

然后让 RT-Thread 的包管理器自动更新，或者使用 `pkgs --update` 命令更新包到 BSP 中。

## 3 使用旧版本BH1750FVI软件包

按照前文介绍，获取 `BH1750FVI` 软件包后，就可以按照 下文提供的 API 使用传感器 `bh1750` 与 `Finsh/MSH` 命令进行测试，详细内容如下。

### 3.1 API

#### 3.1.1  初始化 

`bh1750_device_t bh1750_init(const char *i2c_bus_name)`

根据总线名称，自动初始化对应的 `bh1750` 设备，具体参数与返回说明如下表

| 参数    | 描述                      |
| :----- | :----------------------- |
| name   | i2c 设备名称 |
| **返回** | **描述** |
| != NULL | 将返回 bh1750 设备对象 |
| = NULL | 查找失败 |

#### 3.1.2  反初始化

void bh1750_deinit(bh1750_device_t dev)

如果设备不再使用，反初始化将回收 `bh1750` 设备的相关资源，具体参数说明如下表

| 参数 | 描述           |
| :--- | :------------- |
| dev  | bh1750 设备对象 |

#### 3.1.3 读取光照强度

float bh1750_read_light(bh1750_device_t dev)

通过 `bh1750` 传感器读取光照强度测量值，返回浮点型光照强度值，具体参数与返回说明如下表

| 参数     | 描述           |
| :------- | :------------- |
| dev      | bh1750 设备对象 |
| **返回** | **描述**       |
| != 0.0   | 测量光照强度值     |
| =00000.0     | 测量失败       |


### 3.2 Finsh/MSH 测试命令

BH1750FVI 软件包提供了丰富的测试命令，项目只要在 RT-Thread 上开启 Finsh/MSH 功能即可。在做一些基于 `BH1750FVI` 的应用开发、调试时，这些命令会非常实用，它可以准确的读取传感器测量的光照强度。具体功能可以输入 `bh1750` ，可以查看完整的命令列表

```
msh />bh1750
Usage:
bh1750 probe <dev_name>   - probe sensor by given name
bh1750 read               - read sensor bh1750 data
msh />
```

#### 3.2.1 在指定的 i2c 总线上探测传感器 

当第一次使用 `bh1750` 命令时，直接输入 `bh1750 probe <dev_name>` ，其中 `<dev_name>` 为指定的 i2c 总线，例如：`i2c2`。如果有这个传感器，就不会提示错误；如果总线上没有这个传感器，将会显示提示找不到相关设备，日志如下：

```
msh />bh1750 probe i2c2      #探测成功，没有错误日志
msh />
msh />bh1750 probe i2c66     #探测失败，提示对应的 I2C 设备找不到
[E/bh1750] can't find bh1750 device on 'i2c66'
msh />
```

#### 3.2.2 读取数据

探测成功之后，输入 `bh1750 read` 即可获取光照强度，包括提示信息，日志如下： 

```
msh />bh1750 read
read bh1750 sensor intensity   : 00431.6 lx
msh />
```

## 4 注意事项

暂无。

## 5 联系方式

* 维护：[Sanjay_Wu](https://github.com/sanjaywu)
* 主页：[https://github.com/sanjaywu/bh1750_sensor/tree/bh1750-v1.0.0](https://github.com/sanjaywu/bh1750_sensor/tree/bh1750-v1.0.0 "https://github.com/sanjaywu/bh1750_sensor/tree/bh1750-v1.0.0")
* 邮箱：sanjaywu@yeah.net


