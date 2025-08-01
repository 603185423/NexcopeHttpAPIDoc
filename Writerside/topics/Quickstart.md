# Quickstart

> 环境要求: Windows 10及以上的64位系统

## 概述

本文将完成:

* [安装设备驱动](#安装设备驱动)
* [配置监听端口以及程序路径](#修改配置)

## 安装设备驱动 {#安装设备驱动}

:information_source: 仅安装需要使用的设备驱动即可.

### 显微镜串口驱动

1. 安装`VCP_V1.4.0_Setup.exe`文件
2. 连接显微镜到PC上
3. 打开设备管理器, 查看`端口`选项
4. 确保显微镜端口的设备名称以`STMicroelectronics`开头
5. 如果设备名称错误, 需要手动打开`C:\Program Files (x86)\STMicroelectronics\Software\Virtual comport driver\Win8`目录, 安装`dpinst_amd64.exe`文件

### PI驱动

1. 安装`PISoftwareSuite.exe`

## 修改配置 {#修改配置}

打开`NexcopeHttpAPI.exe`, 程序会自动在目录中创建`config.json`文件

程序的默认配置如下

```json
{
  "listen_address": "localhost:26358",
  "programs": [
    {
      "aliases": [
        "program_alias1"
      ],
      "program_path": "/your/path/for/program.exe",
      "startup_delay": 1000
    }
  ]
}
```

`listen_address`为[web接口监听地址](#web接口监听地址)

`programs`记录了[指定外部程序的调用路径](#外部程序调用路径)

### web接口监听地址 {#web接口监听地址}

格式为`{host}:{port}`, 通常不推荐将`host`设置为`0.0.0.0`

### 外部程序调用路径 {#外部程序调用路径}

该值以数组的形式记录了多个外部程序的路径及其别名, 用于提供web接口`/launcher`调用

- `program_path`为程序的路径
- `aliases`为程序的别名, 需要填入Endpoint `/launcher/{program_alias}`中以调用该程序
- `startup_delay`为调用程序前的等待时间, 以避免过快启动程序, 单位为ms, 缺省值1000ms

### 完整示例

```json
{
  "listen_address": "localhost:26358",
  "programs": [
    {
      "aliases": [
        "npd"
      ],
      "program_path": "notepad",
      "startup_delay": 1000
    },
    {
      "aliases": [
        "c"
      ],
      "program_path": "calc",
      "startup_delay": 1000
    }
  ]
}
```

---

## 更多信息

* [如何控制显微镜](How-to-control-microscope.md)
* [如何控制PI设备](How-to-control-PI-device.md)
* [如何使用PI的模拟输入](How-to-use-pi-analog-input.md)
* [API Reference](API_Reference.topic)

