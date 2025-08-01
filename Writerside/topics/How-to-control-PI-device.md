# 如何控制PI设备

本文以`PowerShell - RestMethod`和`Python - Requests`为例, 简单控制PI设备的xyz运动

>
> 请先阅读[Quickstart](Quickstart.md)并安装完成PI驱动.
>
{style="note"}

## 概述

控制PI的平台需要以下步骤:
1. [枚举所有PI设备](#枚举所有PI设备)
2. [连接PI设备](#连接PI设备)
3. (可选)[Auto Zero](#Auto-Zero)
4. [开启伺服模式](#开启伺服模式)
5. (可选)[获取当前XYZ轴坐标](#获取当前XYZ轴坐标)
6. [移动XYZ轴到新的坐标](#移动XYZ轴到新的坐标)
7. [关闭PI设备的连接](#关闭PI设备的连接)


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

## (可选)Auto Zero {#Auto-Zero}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-Auto-Zero" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"x`": true,
    `"y`": true,
    `"z`": true
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/auto_zero' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-Auto-Zero" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/auto_zero"

payload = "{\r\n    \"x\": true,\r\n    \"y\": true,\r\n    \"z\": true\r\n}"
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

## 开启伺服模式 {#开启伺服模式}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-开启伺服模式" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"x`": true,
    `"y`": true,
    `"z`": true
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/servo_state' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-开启伺服模式" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/servo_state"

payload = "{\r\n    \"x\": true,\r\n    \"y\": true,\r\n    \"z\": true\r\n}"
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

## (可选)获取当前XYZ轴坐标 {#获取当前XYZ轴坐标}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-获取当前XYZ轴坐标" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/move' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-获取当前XYZ轴坐标" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/move"

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
    "result": true,
    "x": -0.1098079681,
    "y": -0.1136112213,
    "z": -0.0341091156
}
```

## 移动XYZ轴到新的坐标 {#移动XYZ轴到新的坐标}

>
> 移动平台前务必保证对应的轴的[servo模式](#开启伺服模式)处于开启状态.
>
{style="note"}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-移动XYZ轴到新的坐标" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"
{
    `"x`": 1.2,
    `"y`": 2.3,
    `"z`": 0.1
}
"@

$response = Invoke-RestMethod 'http://{{serverIP}}:26358/v1/device/pi/E727/1/move' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-移动XYZ轴到新的坐标" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://{{serverIP}}:26358/v1/device/pi/E727/1/move"

payload = "{\r\n    \"x\": 1.2,\r\n    \"y\": 2.3,\r\n    \"z\": 0.1\r\n}"
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

## 关闭PI设备的连接 {#关闭PI设备的连接}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-关闭PI设备的连接" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"

"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/pi/E727/1/disconnect_device' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-关闭PI设备的连接" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/pi/E727/1/disconnect_device"

payload = ""
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
</code-block>
</tab>
</tabs>








