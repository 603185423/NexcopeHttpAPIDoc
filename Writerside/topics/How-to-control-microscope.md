# 如何控制显微镜

本文以`PowerShell - RestMethod`和`Python - Requests`为例, 简单控制显微镜平台的xyz运动和物镜转盘的运动

>
> 请先阅读[Quickstart](Quickstart.md)并安装完成显微镜驱动.
>
{style="note"}

## 概述

控制显微镜的平台和物镜需要以下步骤:
1. [获取所有显微镜设备](#获取所有显微镜设备)
2. (可选)[连接显微镜](#连接显微镜)
3. (可选)[获取当前的XYZ轴坐标](#获取当前的XYZ轴坐标)
4. [移动XYZ轴到新的坐标](#移动XYZ轴到新的坐标)
5. (可选)[获取当前物镜转盘的孔位](#获取当前物镜转盘的孔位)
6. [控制物镜转盘运动到新的孔位](#控制物镜转盘运动到新的孔位)
7. [关闭显微镜连接](#关闭显微镜连接)

## 获取所有显微镜设备 {#获取所有显微镜设备}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-获取所有显微镜设备" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-获取所有显微镜设备" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/microscope"

payload = ""
headers = {}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
</code-block>
</tab>
</tabs>

### 接口返回

```json
{
    "devices": [
        {
            "id": 24,
            "name": "STMicroelectronics Virtual COM Port (COM24)",
            "port": 24
        }
    ]
}
```

获得需要使用的显微镜的设备ID`id = 24`

## (可选)连接显微镜 {#连接显微镜}

> 显微镜在发送控制请求时会自动连接未连接的设备, 所以该步骤可选
> {style="note"}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-连接显微镜" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/open' -Method 'POST' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-连接显微镜" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/microscope/24/open"

payload = ""
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
</code-block>
</tab>
</tabs>

## (可选)获取当前的XYZ轴坐标 {#获取当前的XYZ轴坐标}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-获取当前的XYZ轴坐标" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/position' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-获取当前的XYZ轴坐标" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/microscope/24/position"

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
    "Data": {
        "x": 5791430,
        "y": 4940710,
        "z": 49824
    },
    "ErrorCode": null,
    "ErrorReason": null
}
```

## 移动XYZ轴到新的坐标 {#移动XYZ轴到新的坐标}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-移动XYZ轴到新的坐标" title="powershell" group-key="powershell">
<code-block lang="powershell">
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")

$body = @"
{
`"x`": 5000000,
`"y`": 4000000,
`"z`": 30000
}
"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/position' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-移动XYZ轴到新的坐标" title="python" group-key="python">
<code-block lang="python">
import requests
import json

url = "http://localhost:26358/v1/device/microscope/24/position"

payload = json.dumps({
"x": 5000000,
"y": 4000000,
"z": 30000
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
    "ErrorCode": null,
    "ErrorReason": null
}
```

## (可选)获取当前物镜转盘的孔位 {#获取当前物镜转盘的孔位}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-获取当前物镜转盘的孔位" title="powershell" group-key="powershell">
<code-block lang="powershell">
$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/WJ' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-获取当前物镜转盘的孔位" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/microscope/24/WJ"

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
        "Data": {
            "WJ": 2
        },
        "ErrorCode": null,
        "ErrorReason": null
    }
```

## 控制物镜转盘运动到新的孔位 {#控制物镜转盘运动到新的孔位}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-控制物镜转盘运动到新的孔位" title="powershell" group-key="powershell">
<code-block lang="powershell">
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")

$body = @"
{
`"WJ`": 2
}
"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/WJ' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python-控制物镜转盘运动到新的孔位" title="python" group-key="python">
<code-block lang="python">
import requests
import json

url = "http://localhost:26358/v1/device/microscope/24/WJ"

payload = json.dumps({
"WJ": 4
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
    "ErrorCode": null,
    "ErrorReason": null
}
```

## 关闭显微镜连接 {#关闭显微镜连接}

### 发送请求

<tabs group="code-lang">
<tab id="powershell-关闭显微镜连接" title="powershell" group-key="powershell">
<code-block lang="powershell">
$body = @"

"@

$response = Invoke-RestMethod 'http://localhost:26358/v1/device/microscope/24/close' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
</code-block>
</tab>
<tab id="python1-关闭显微镜连接" title="python" group-key="python">
<code-block lang="python">
import requests

url = "http://localhost:26358/v1/device/microscope/24/close"

payload = ""
headers = {}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
</code-block>
</tab>
</tabs>


