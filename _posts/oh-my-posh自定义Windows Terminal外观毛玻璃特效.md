

# oh-my-posh自定义Windows Terminal外观|毛玻璃特效

先放上最终效果图

![最终效果图](https://github.com/238Uranium/tuchuang/blob/main/OMP-WindowsTerminal/Final.png)

oh-my-posh在官网有详细的安装教程，但是无论是按照oh-my-posh官网说明还是微软官网说明均会遇到一些问题，在此记录安装过程。

### 准备工作

#### 字体安装

首先需要安装字体，在这推荐安装 [Nerd Fonts](https://www.nerdfonts.com/font-downloads)。

![Nerd Font官网](https://github.com/238Uranium/tuchuang/blob/main/OMP-WindowsTerminal/NerdFont.png)

在此借用微软官方教程的话：

```Text
自定义命令提示符通常使用字形（图形符号）来设置提示符的样式。 如果你的字体不包含相应字形，则在整个提示符中，你可能会看到若干 Unicode 替换字符“▯”。 若要在终端中查看所有字形，建议安装 Nerd Font。
```

字体安装可以参考[Add a font (microsoft.com)](https://support.microsoft.com/en-us/office/add-a-font-b7c5f17c-4426-4b53-967f-455339c564c1)

#### 字体更改

Windows Terminal更改字体需要打开设置（快捷键`Ctrl+,`），选择需更改的配置文件>外观>字体。

#### 修改Powershell策略

在默认情况下Powershell策略会阻止脚本运行，因此需要更改。在Terminal中输入以下代码

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

### 安装Oh-My-Posh

#### 安装

在此使用 [PowerShell install-module](https://docs.microsoft.com/zh-cn/powershell/module/powershellget/install-module) 安装 Oh My Posh，输入以下命令：

```powershell
Install-Module -Name PSReadLine -Scope AllUsers -Force -SkipPublisherCheck
Install-Module posh-git -Scope AllUsers
Install-Module oh-my-posh -Scope AllUsers
```

> 注1：该命令会为所有用户安装OMP，若不为全体用户安装，后续会出现主题不能配置的问题
>
> 注2：命令需要管理员权限

以上命令会安装oh-my-posh已经最新的主题。安装路径会在`C:\Program Files\WindowsPowerShell\Modules\oh-my-posh`

安装之后建议使用一下命令进行更新：

```powershell
Update-Module oh-my-posh
```

#### 配置环境

安装之后需要使用以下命令设置

```powershell
$DefaultUser = $env:USERNAME
```

### 生成配置文件并配置

```powershell
code $profile
```

> 注：本人使用的编辑器为VS Code，所以在此使用code命令。可将其换成你电脑上的其他编辑器

在编辑器中输入

```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme clean-detailed
```

> 注1：官方文档中使用的命令为Set-Theme，但可能会遇到
>
> ```Text
> Set-Theme : 无法将“Set-Theme”项识别为cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次。
> ```
>
> 的问题，因此改为`Set-PoshPrompt`

可将第三行中`-Theme`后的参数改为你喜欢的主题。[主题列表](https://ohmyposh.dev/docs/themes)

### 查看主题效果

重启Windows Terminal，等待第一次配置读入。

### 配置毛玻璃特效

毛玻璃特效需要在`setting.json`文件中配置。首先需要在`"schemes"`字段添加如下代码

```json
{
    "name" : "Frost",
    "background" : "#FFFFFF",
    "black" : "#3C5712",
    "blue" : "#17b2ff",
    "brightBlack" : "#749B36",
    "brightBlue" : "#27B2F6",
    "brightCyan" : "#13A8C0",
    "brightGreen" : "#89AF50",
    "brightPurple" : "#F2A20A",
    "brightRed" : "#F49B36",
    "brightWhite" : "#741274",
    "brightYellow" : "#991070",
    "cyan" : "#3C96A6",
    "foreground" : "#000000",
    "green" : "#6AAE08",
    "purple" : "#991070",
    "red" : "#8D0C0C",
    "white" : "#6E386E",
    "yellow" : "#991070"
}
```

然后在`"profiles"`字段添加以下字段

```json
"acrylicOpacity": 0.3,
"colorScheme" : "Frost",
"cursorColor" : "#000000",
"fontFace" : "Cascadia Code PL",
"useAcrylic": true
```

此处添加如图

[][tuchuang/setting.png at main · 238Uranium/tuchuang (github.com)]![profiles字段添加](https://github.com/238Uranium/tuchuang/blob/main/OMP-WindowsTerminal/setting.png)

`"acrylicOpacity"`行定义了毛玻璃特效的透明度，可根据喜好自行调节

建议将Terminal主题设置为亮色

```json
"theme": "light"
```

### 关于

#### Powershell策略

PowerShell 的执行策略是一项安全功能，用于控制 PowerShell 加载配置文件和运行脚本的条件，Powershell策略共有七种。

##### AllSigned

- 脚本可以运行。
- 要求所有脚本和配置文件都由受信任的发布者签名，包括在本地计算机上编写的脚本。
- 在从尚未分类为受信任或不受信任的发布者运行脚本之前，提示你。
- 运行已签名但恶意的脚本的风险。

##### Bypass

- 不阻止任何操作，并且没有任何警告或提示。
- 此执行策略适用于将 PowerShell 脚本内置到较大应用程序中的配置，或用于 PowerShell 是具有其自己的安全模型的程序基础的配置。

##### Default

- 设置默认执行策略。
- **Restricted** 对于 Windows 客户端。
- 适用于 Windows 服务器的 RemoteSigned。

##### RemoteSigned

- Windows 服务器计算机的默认执行策略。
- 脚本可以运行。
- 需要受信任的发布者对从 Internet 下载的脚本和配置文件（包括电子邮件和即时消息程序）进行数字签名。
- 对于在本地计算机上编写且未从 Internet 下载的脚本，不需要数字签名。
- 如果脚本未受阻止（例如使用 cmdlet），则运行从 Internet 下载且 `Unblock-File` 未签名的脚本。
- 从 Internet 来源（而不是 Internet）运行未签名脚本的风险，以及可能是恶意的已签名脚本的风险。

##### Restricted

- Windows 客户端计算机的默认执行策略。
- 允许单个命令，但不允许脚本。
- 阻止运行所有脚本文件，包括格式化和配置文件 () 、模块脚本文件 () 和 `.ps1xml` `.psm1` PowerShell 配置文件 `.ps1`  。

##### Undefined

- 当前作用域中未设置执行策略。
- 如果所有作用域中的执行策略都是 ，则有效执行策略 **Undefined** 适用于 **Restricted** Windows 客户端 **，RemoteSigned** 适用于 Windows Server。

##### Unrestricted

- 不能更改非 Windows 计算机的默认执行策略。
- 未签名的脚本可以运行。 存在运行恶意脚本的风险。
- 在运行不是来自本地 Intranet 区域中的脚本和配置文件之前，警告用户。
