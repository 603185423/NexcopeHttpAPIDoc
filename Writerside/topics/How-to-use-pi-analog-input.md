# 如何使用PI的模拟输入

本文以`PowerShell - RestMethod`和`Python - Requests`为例, 使用模拟输入的通道7控制PI设备的z轴运动

>
> 请先阅读[Quickstart](Quickstart.md)并安装完成PI驱动.
>
{style="note"}

## 概述

控制PI的平台需要以下步骤:
1. [枚举所有PI设备](#枚举所有PI设备)
2. [连接PI设备](#连接PI设备)
3. [设置命令等级](#设置命令等级)
4. [设置模拟输入的sensor range factor](#设置模拟输入的sensor-range-factor)
5. [设置模拟输入的Gain和Offset](#设置模拟输入的Gain和Offset)
6. [设置z轴的模拟输入通道](#设置z轴的模拟输入通道)


## 枚举所有PI设备 {#枚举所有PI设备}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-枚举所有PI设备" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/enumerate_devices' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-枚举所有PI设备" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/enumerate_devices"

payload = {}
headers = {}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
    "DeviceCount": 1,
    "DeviceNames": [
        "PI E-727 Controller SN 0124009586"
    ]
}
```

获得设备名称`PI E-727 Controller SN 0124009586`用于下一步的连接

## 连接PI设备 {#连接PI设备}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-连接PI设备" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"DeviceName`": `"PI E-727 Controller SN 0124009586`"
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/connect_device' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-连接PI设备" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/connect_device"

payload = "{\r\n    \"DeviceName\": \"PI E-727 Controller SN 0124009586\"\r\n}"
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
    "DeviceID": 1
}
```

获得设备ID`1`用于后续的操作

## 设置命令等级 {#设置命令等级}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-设置命令等级" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"Level`": 1,
    `"Password`": `"advanced`"
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/command_level' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-设置命令等级" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/command_level"

payload = "{\r\n    \"Level\": 1,\r\n    \"Password\": \"advanced\"\r\n}"
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
    "result": true
}
```

## 设置模拟输入的sensor range factor {#设置模拟输入的sensor-range-factor}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-设置模拟输入的sensor-range-factor" title="powershell" group-key="powershell">
<code-block lang="powershell">
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")

$body = @"
{
`"CH4`": 2,
`"CH5`": 2,
`"CH6`": 2,
`"CH7`": 2
}
"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/pi/E727/1/analog_input_sensor_range_factor' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-设置模拟输入的sensor-range-factor" title="python" group-key="python">
<code-block lang="python">
import requests
import json

url = "http://localhost:26358/v1/device/pi/E727/1/analog_input_sensor_range_factor"

payload = json.dumps({
"CH4": 2,
"CH5": 2,
"CH6": 2,
"CH7": 2
})
headers = {
'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
    "result": true
}
```

## 设置模拟输入的Gain和Offset {#设置模拟输入的Gain和Offset}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-设置模拟输入的Gain和Offset" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"CH7`": {`"Gain`": 1.4, `"Offset`": -20}
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/analog_input_gain_offset' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-设置模拟输入的Gain和Offset" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/analog_input_gain_offset"

payload = "{\r\n    \"CH7\": {\"Gain\": 1.4, \"Offset\": -20}\r\n}"
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
  "result": true
}
```

## 设置z轴的模拟输入通道 {#设置z轴的模拟输入通道}

>
> 为了避免未使用的通道对xy轴干扰, 需要将xy的通道设置为0以关闭xy的模拟输入
>
{style="note"}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-设置z轴的模拟输入通道" title="powershell" group-key="powershell">
<code-block lang="powershell">
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")

$body = @"
{
`"x`": 0,
`"y`": 0,
`"z`": 7
}
"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/pi/E727/1/analog_input_channel' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-设置z轴的模拟输入通道" title="python" group-key="python">
<code-block lang="python">
import requests
import json

url = "http://localhost:26358/v1/device/pi/E727/1/analog_input_channel"

payload = json.dumps({
"x": 0,
"y": 0,
"z": 7
})
headers = {
'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

</code-block>
</tab>
</tabs>

### 接口返回

```json
{
  "result": true
}
```

现在可以通过CH7的差分信号对z轴进行控制了

