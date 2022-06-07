# sysinternals wind工具使用

## AccessChk 

用来检测特定用户或组对资源（包括文件、目录、注册表项、全局对象和Windows服务）拥有哪些类型的访问权限。

| 参数   | 说明                                                         |
| :----- | :----------------------------------------------------------- |
| **-a** | 名称是Windows帐户权限。 指定 `"*"` 为显示分配给用户的所有权限的名称。 请注意，指定特定权限时，仅显示直接分配给右侧的组和帐户。 |
| **-c** | 名称是Windows服务，例如。 `ssdpsrv` 指定 `"*"` 为显示所有服务的名称，并 `scmanager` 检查服务控制管理器的安全性。 |
| **-d** | 仅处理目录或顶级密钥                                         |
| **-e** | 仅显示 (Windows Vista 及更高级别的显式设置完整性级别)        |
| **-f** | 如果如下所示 `-p`，则显示完整的进程令牌信息，包括组和特权。 否则为要从输出中筛选的逗号分隔帐户的列表。 |
| **-h** | 名称是文件或打印机共享。 指定 `"*"` 为显示所有共享的名称。   |
| **-i** | 在转储完全访问控制列表时，仅忽略仅继承 ACE 的对象。          |
| **-k** | 名称是注册表项，例如 `hklm\software`                         |
| **-l** | 显示完整的安全描述符。 添加 `-i` 以忽略继承的 ACE。          |
| **-n** | 仅显示没有访问权限的对象                                     |
| **-o** | 名称是对象管理器命名空间中的对象， (默认值为根) 。 若要查看目录的内容，请使用尾随反斜杠或添加 `-s`指定名称。 添加 `-t` 和对象类型 (，例如节) 仅查看特定类型的对象。 |
| **-p** | 名称是进程名称或 PID，例如 `cmd.exe` ， (指定 `"*"` 为显示所有进程) 的名称。 添加 `-f` 以显示完整的进程令牌信息，包括组和特权。 添加 `-t` 以显示线程。 |
| **-q** | 省略横幅                                                     |
| **-r** | 仅显示具有读取访问权限的对象                                 |
| **-s** | Recurse                                                      |
| **-t** | 对象类型筛选器，例如 `"section"`                             |
| **-u** | 禁止显示错误                                                 |
| **-v** | 详细 (包括Windows Vista 完整性级别)                          |
| **-w** | 仅显示具有写入访问权限的对象                                 |

### 实例

1、检查Power User 账户对 \Windows\system32 的权限

```
accesschk "power users" c:\windows\system32
```

![image-20220530094741396](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q6vembxej21jg0o4n3z.jpg)

2、检查某用户在 HKLM\CurrentUser 注册表哪些是无权问的

```
accesschk -kns hzl hklm
```

![image-20220530095504594](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q731pa8kj21ci0ne7cd.jpg)

## AccessEnum

文件注册表权限检查管理工具

![image-20220530100015059](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q78flv17j21580u047u.jpg)

### 作用

检查指定文件夹或者注册表权限是否异常

## AD Explorer

AD数据库管理工具

![image-20220530100649266](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q7f95jsdj20xl0u0tab.jpg)

## ADInsight 

管理自动登录的开启与关闭

![image-20220530101408296](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q7mve17bj20nw0d2758.jpg)

## Autorunsc 

计算机自启项的管理

查看系统启动和登录时配置为自动启动的程序。 自动运行还显示应用程序可以配置自动启动设置的注册表和文件位置的完整列表。

![image-20220530102308052](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q7wbqhyij210m0u0gtw.jpg)

## BgInfo

在桌面背景，显示系统信息、网络信息等计算机基础信息

![image-20220530102935437](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q82xvgrdj21490u0gs6.jpg)

### 作用

方面在对大量电脑配置时，方便进行信息检查

## CacheSet 

缓存管理器

![image-20220530102920913](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q82plyvtj20q60hymys.jpg)

## ClockRes

查看系统时钟的分辨率

![image-20220530103132226](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q84yyot6j20x60dm40d.jpg)

## Contig

```
\src\Contig\Release\Contig.exe [-a] [-s] [-q] [-v] [现有文件]
```

| 参数   | 说明                                              |
| :----- | :------------------------------------------------ |
| **-a** | 分析碎片                                          |
| **-f** | 分析可用空间碎片                                  |
| **-l** | 设置有效的数据长度以快速创建文件 (需要管理员权限) |
| **-q** | 静默模式                                          |
| **-s** | Recurse 子目录                                    |
| **-v** | “详细”                                            |

## Coreinfo

查看系统各部件缓存信息

```
用法：coreinfo [-c][-f][-g][-l][-n][-s][-m][-v]
```

| 参数                             | 说明                                             |
| :------------------------------- | :----------------------------------------------- |
| **-c**                           | 有关核心的转储信息。                             |
| **-f**                           | 转储核心功能信息。                               |
| **-g**                           | 有关组的转储信息。                               |
| **-l**                           | 有关缓存的转储信息。                             |
| **-n**                           | NUMA 节点上的转储信息。                          |
| **-s**                           | 有关套接字的转储信息。                           |
| **-m**                           | 转储 NUMA 访问成本。                             |
| **-v**                           | 仅转储与虚拟化相关的功能，包括支持二级地址转换。 |
| (需要 Intel 系统上的管理权限) 。 |                                                  |

![image-20220530103605081](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q89pewhnj20u00wpjxi.jpg)

## Dbgview

用于监视本地系统上的调试输出

![image-20220530104430434](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8igwubsj21880ng75y.jpg)

## Desktops

桌面允许在最多四个虚拟桌面上组织应用程序。

![image-20220530104517678](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8ja754fj20ns0l6wgc.jpg)

## Disk2vhd 

Disk2vhd 是一个实用工具，用于创建 VHD (虚拟硬盘 - Microsoft 虚拟机磁盘格式) 版本的物理磁盘

![image-20220530104808449](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8m93jccj212k0rgtd4.jpg)

## DiskExt

显示卷磁盘映射。

![image-20220530104943348](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8nxc55oj210c0u0aet.jpg)

## DiskMon

用于记录和显示Windows系统上的所有硬盘活动

![image-20220530105058243](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8p6tik6j20u014x0z9.jpg)

## DiskView

DiskView 显示磁盘的图形映射，使你能够确定文件所在的位置，或者通过单击群集，查看哪些文件占用了它。 双击以获取有关分配群集的文件的详细信息。

![image-20220530105423149](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8sqnizjj212a0u0gpj.jpg)

## du

报告指定的目录的磁盘空间使用情况。 默认情况下，它会递归目录以显示目录及其子目录的总大小。

```
用法：du [-c[t]] [-l <levels> | -n | -v] [-u] [-q] <directory>
```



| 参数   | 说明                                         |
| :----- | :------------------------------------------- |
| **-c** | 将输出打印为 CSV。 使用 -ct 进行制表符分隔。 |
| **-l** | 指定信息 (默认为所有级别) 的子目录深度。     |
| **-n** | 不要递归。                                   |
| **-v** | 在中间目录的 KB) 中显示大小 (。              |
| **-u** | 计算硬链接文件的每个实例。                   |
| **-q** | 安静 (没有横幅) 。                           |

### 实例

```
显示c盘使用情况
du.exe c:/ 
```

![image-20220530105620885](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8usbwh0j20x40fctar.jpg)

## EFSDump

让你能够查看谁有权访问加密文件。

| 参数   | 说明         |
| :----- | :----------- |
| **-s** | 递归子目录。 |

```
efsdump *.txt
```

![image-20220530110013149](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q8yu7cdgj21d80hg41s.jpg)

## FindLinks

报告文件索引和任何硬链接， (指定文件存在的同一卷上的备用文件路径) 。 只要文件至少有一个文件名引用它，文件的数据仍会保持分配状态。

### 实例

```
FindLinks.exe c:\windows\notepad.exe
```

![image-20220530110236830](https://tva1.sinaimg.cn/large/e6c9d24ely1h2q91b4n3pj21yc0hydjg.jpg)

## handle

这个方便的命令行实用工具将显示哪些文件由哪些进程打开，等等。

```
usage： handle [[-a] [-u] |[-c <句柄> [-l] [-y]] |[-s]][-p <processname>|<pid>> [name]
```



| 参数     | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| **-a**   | 转储有关所有类型的句柄的信息，而不仅仅是引用文件的句柄。 其他类型的包括端口、注册表项、同步基元、线程和进程。 |
| **-c**   | 关闭指定句柄 (解释为十六进制数) 。 必须按其 PID 指定进程。 **警告：** 关闭句柄可能会导致应用程序或系统不稳定。 |
| **-l**   | 转储页文件支持的分区的大小。                                 |
| **-y**   | 不要提示关闭句柄确认。                                       |
| **-s**   | 打开每种类型的句柄的打印计数。                               |
| **-u**   | 搜索句柄时显示拥有的用户名。                                 |
| **-p**   | 此参数不会检查系统中的所有句柄，而是将 Handle 的扫描范围缩小到以名称进程开头的进程。 因此： **handle -p exp** 将转储以“exp”开头的所有进程的打开文件，其中包括 Explorer。 |
| **name** | 此参数存在，以便你可以定向 Handle 以搜索对具有特定名称的对象的引用。 例如，如果想要知道 (是否有任何) 打开“c：\windows\system32”的进程，则可以键入： **处理 windows\system** 名称匹配不区分大小写，指定的片段可以是你感兴趣的路径中的任何位置。 |

## Hex2dec

十六进制与十进制转换工具

### 实例

```
hex2dec.exe 0x123
hex2dec.exe 123
```

![image-20220530153059259](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qgsk03d0j214i0loq61.jpg)

## LDMDump

可用于检查存储在系统 LDM 数据库的磁盘副本中的确切内容

**用法：ldmdump [- ] [-d#]**

| 参数    | 说明                                                         |
| :------ | :----------------------------------------------------------- |
| **-**   | 显示支持的选项和用于输出值的度量单位。                       |
| **-d#** | 指定要检查 *的 LDMDump* 磁盘数。 例如，“ldmdump /d0”具有 *LDMDump* 显示存储在磁盘 0 上的 LDM 数据库信息。 |

### 案例

![image-20220530153553659](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qgxnl16lj20ym0ayq4a.jpg)

## ListDLL

加载到进程的 DLL 的实用工具

### 用法

```
listdlls [-r] [-v | -u] [processname|pid]
listdlls [-r] [-v] [-d dllname]
```

| 参数            | 说明                                         |
| :-------------- | :------------------------------------------- |
| **processname** | 进程加载的转储 DLL (接受) 部分名称。         |
| pid             | 转储 DLL 与指定的进程 ID 相关联。            |
| **dllname**     | 仅显示已加载指定 DLL 的进程。                |
| **-r**          | 由于它们未在其基址加载而重新定位的标志 DLL。 |
| **-u**          | 仅列出未签名的 DLL。                         |
| **-v**          | 显示 DLL 版本信息。                          |

### 示例

列出加载到Outlook.exe的 DLL，包括其版本信息：

**listdlls -v outlook**

列出加载到任何进程的未签名 DLL：

**listdlls -u**

![image-20220530153746322](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qgzlulu8j20wf0u0n16.jpg)

显示已加载MSO.DLL的进程：

**listdlls -d mso.dll**

![image-20220530153829612](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qh0d0rotj21470u0juc.jpg)

## LoadOrder

查看在 WinNT/2K 系统上加载设备的顺序。

![image-20220530154024142](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qh2cc8odj211s0u0gtk.jpg)

## LogonSessions

列出系统上的活动登录会话。

**用法：logonsessions [-c[t]] [-p]**

| 参数    | 说明                         |
| :------ | :--------------------------- |
| **-c**  | 将输出打印为 CSV。           |
| **-ct** | 将输出打印为制表符分隔的值。 |
| **-p**  | 列出在登录会话中运行的进程。 |

### 案例

```
logonsessions.exe -p
```

![image-20220530154141138](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qh3oyif1j20u00xigpr.jpg)

## PendMoves 和 MoveFile

允许计划下一次重新启动的移动和删除命令。

### PendMoves 用法

此小程序会转储挂起的重命名/删除值的内容，并在源文件不可访问时报告错误。

![image-20220530154356972](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qh616xccj20zy0u079w.jpg)

### MoveFile 用法

包含的 MoveFile utililty 允许计划下一次重新启动的移动和删除命令 **：用法：movefile [source] [dest]**
指定空目标 (“”) 在启动时删除源。 删除test.exe的示例如下：

```Shell
movefile test.exe ""
```

## NotMyFault

Notmyfault 是一种工具，可用于崩溃、挂起并导致Windows系统上的内核内存泄漏。它可用于了解如何识别和诊断设备驱动程序和硬件问题，还可以使用它在行为不当的系统上生成蓝屏转储文件。

![image-20220530154949951](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhc5vz0kj20u00zjwiv.jpg)

点击后会蓝屏

## NTFSInfo

显示有关 NTFS 卷的信息

```
ntfsinfo.exe 0
```

![image-20220530155340029](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhg4sr94j20xs0asabd.jpg)

## PipeList 

是否知道实现命名管道的设备驱动程序实际上是文件系统驱动程序？

![image-20220530155620067](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhixa2n4j20wy0u0wim.jpg)

## PortMon

使用此高级监视工具监视串行和并行端口活动。 它了解所有标准串行和并行 IOCTL，甚至显示要发送和接收的数据的一部分。 版本 3.x 具有强大的新 UI 增强功能和高级筛选功能

## ProcDump

ProcDump 是一个命令行实用工具，其主要用途是在管理员或开发人员可用于确定峰值原因的峰值期间监视 CPU 峰值和生成故障转储的应用程序。

## procexp

进程资源管理器

![image-20220530160157522](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhos7nk6j20y50u0jz5.jpg)

## Procmon

进程监视器是用于Windows的高级监视工具，用于显示实时文件系统、注册表和进程/线程活动。

![image-20220530160418712](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhr83kfej20w50u0n9h.jpg)

## PsExec

Telnet 等实用工具和远程控制程序（如 Symantec 的 PC Anywhere）可让你在远程系统上执行程序，但它们可能很难设置并要求你在要访问的远程系统上安装客户端软件。

**用法：**

cmd复制

```cmd
psexec [\\computer[,computer2[,...] | @file]][-u user [-p psswd][-n s][-r servicename][-h][-l][-s|-e][-x][-i [session]][-c executable [-f|-v]][-w directory][-d][-<priority>][-a n,n,...] cmd [arguments]
```

| 参数            | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| **-a**          | 使用逗号运行应用程序的单独处理器，其中 1 是最小编号的 CPU。 例如，若要在 CPU 2 和 CPU 4 上运行应用程序，请输入：“-a2，4” |
| **-c**          | 将指定的可执行文件复制到远程系统以供执行。 如果省略此选项，则应用程序必须位于远程系统上的系统路径中。 |
| **-d**          | 不要等待进程终止 (非交互式) 。                               |
| **-e**          | 不加载指定帐户的配置文件。                                   |
| **-f**          | 即使文件已存在于远程系统上，也复制指定的程序。               |
| **-i**          | 运行程序，使其与远程系统上指定会话的桌面进行交互。 如果未指定会话，进程将在控制台会话中运行。 尝试使用重定向的标准 IO) 以交互方式 (运行控制台应用程序时 **，需要** 此标志。 |
| **-h**          | 如果目标系统为 Vista 或更高版本，则进程使用帐户提升的令牌运行（如果可用）。 |
| **-l**          | 作为受限用户 (运行进程会去除管理员组，并且仅允许分配给用户组的权限) 。 在 Windows Vista 上，进程以低完整性运行。 |
| **-n**          | 指定连接到远程计算机的超时（以秒为单位）。                   |
| **-p**          | 指定用户名的可选密码。 如果省略此内容，系统会提示输入隐藏的密码。 |
| **-r**          | 指定要创建或与之交互的远程服务的名称。                       |
| **-s**          | 在系统帐户中运行远程进程。                                   |
| **-u**          | 指定用于登录到远程计算机的可选用户名。                       |
| **-v**          | 仅当指定文件具有更高的版本号或高于远程系统上的版本号时，才复制指定的文件。 |
| **-w**          | 设置进程的工作目录 (相对于远程计算机) 。                     |
| **-x**          | 仅在本地系统上显示 Winlogon 安全桌面上的 UI， (本地系统) 。  |
| **-priority**   | 指定 -low、-belownormal、-abovenormal、-high 或 -realtime，以不同的优先级运行进程。 使用 -background 在 Vista 上以低内存和 I/O 优先级运行。 |
| **计算机**      | 指示 PsExec 在指定的远程计算机或计算机上运行应用程序。 如果省略计算机名称，PsExec 在本地系统上运行应用程序，如果指定通配符 (\\*) ，PsExec 将在当前域中的所有计算机上运行该命令。 |
| **@file**       | PsExec 将在文件中列出的每台计算机上执行该命令。              |
| **cmd**         | 要执行的应用程序的名称。                                     |
| **参数**        | 要传递 (的参数请注意，文件路径必须是目标系统上的绝对路径) 。 |
| **-accepteula** | 此标志禁止显示许可证对话框。                                 |

![image-20220530160651966](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhtvqywjj21e60u0wme.jpg)

## PsFile

显示其他计算机在执行该命令的系统上打开的文件列表

## PsGetSid

PsGetsid 允许将 SID 转换为其显示名称，反之亦然。 它适用于内置帐户、域帐户和本地帐户。

**用法：psgetsid [\\computer[，computer[,...] | @file\][-u 用户名 [-p password]]][account|SID]**

| 参数       | 说明                                                         |
| :--------- | :----------------------------------------------------------- |
| **-u**     | 指定用于登录到远程计算机的可选用户名。                       |
| **-p**     | 指定用户名的可选密码。 如果省略此项，系统会提示输入隐藏的密码。 |
| **帐户**   | PsGetSid 将报告指定用户帐户的 SID，而不是计算机。            |
| **SID**    | PsGetSid 将报告指定 SID 的帐户。                             |
| **计算机** | 指示 PsGetSid 在指定的远程计算机或计算机上执行命令。 如果省略计算机名称 PsGetSid 在本地系统上运行命令，并且指定通配符 (\\*) ，则 PsGetSid 将在当前域中的所有计算机上运行该命令。 |
| **@file**  | PsGetSid 将在文件中列出的每台计算机上执行该命令。            |

![image-20220530161157209](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qhzsf9hcj213a0cctap.jpg)

## PsInfo

*PsInfo* 显示本地系统的信息。

**用法：psinfo [[\\computer[，computer[,..] | @file [-u 用户
[-p psswd]]][-h][-s][-d][-c [-t 分隔符]][filter]**

| 参数           | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| **\\computer** | 对指定的远程计算机或计算机执行命令。 如果省略该命令在本地系统上运行的计算机名称，并且如果指定通配符 (\\*) ，该命令将在当前域中的所有计算机上运行。 |
| **@file**      | 在指定的文本文件中列出的每台计算机上运行该命令。             |
| **-u**         | 指定用于登录到远程计算机的可选用户名。                       |
| **-p**         | 指定用户名的可选密码。 如果省略此内容，系统会提示输入隐藏的密码。 |
| **-h**         | 显示已安装修补程序的列表。                                   |
| **-s**         | 显示已安装应用程序的列表。                                   |
| **-d**         | 显示磁盘卷信息。                                             |
| **-c**         | 以 CSV 格式打印。                                            |
| **-t**         | -c 选项的默认分隔符是逗号，但可以使用指定的字符替代。        |
| **filter**     | Psinfo 仅显示与筛选器匹配的字段的数据。 例如，“psinfo 服务”仅列出 Service Pack 字段。 |

### 案例

显示补丁列表和磁盘信息

```
PsInfo -h -d
```

![image-20220530161408819](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qi1gqe2aj218j0u0wjo.jpg)

## PsKill 

终止本地或远程进程

## PsPing

sPing 实现 Ping 功能、TCP ping、延迟和带宽度量

![image-20220530161734827](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qi510r4bj21dg0mwdkd.jpg)

## PsList

## 简介

| 参数           | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| **pslist exp** | 显示以“exp”开头的所有进程的统计信息，其中包括资源管理器。    |
| **-d**         | 显示线程详细信息。                                           |
| **-m**         | 显示内存详细信息。                                           |
| **-x**         | 显示进程、内存信息和线程。                                   |
| **-t**         | 显示进程树。                                                 |
| **-s [n]**     | 在任务管理器模式下运行，指定可选秒。 按 Escape 中止。        |
| **-r n**       | 任务管理器模式刷新速率（秒） (默认值为 1) 。                 |
| **\\computer** | *PsList* 不会显示本地系统的进程信息，而是显示指定的 NT/Win2K 系统的信息。 如果安全凭据不允许从远程系统获取性能计数器信息，请包含用户名和密码的 -u 开关以登录到远程系统。 |
| **-u**         | 指定用于登录到远程计算机的可选用户名。                       |
| **-p**         | 使用此选项可以在命令行上指定登录密码，以便可以从批处理文件使用 *PsList* 。 如果指定帐户名称并省略 -p 选项 *PsList* 会以交互方式提示输入密码。 |
| **name**       | 显示以指定名称开头的进程的信息。                             |
| **-e**         | 与进程名称完全匹配。                                         |
| pid            | 此参数不会列出系统中所有正在运行的进程，而是将 *PsList 的* 扫描范围缩小到具有指定 PID 的进程。 因此： **pslist 53** 使用 PID 53 转储进程的统计信息。 |

## PsLoggedOn

对本地登录用户的定义是，其配置文件已加载到注册表中，因此 *PsLoggedOn* 通过扫描HKEY_USERS密钥下的密钥来确定谁登录。

![image-20220530161854065](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qi6ejjoej21i40rewje.jpg)

## PsLogList

显示系统事件日志的内容

![image-20220530161942754](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qi793ovvj217q0u0wis.jpg)

## PsPasswd

可以使用 *PsPasswd* 在本地或远程计算机上更改本地帐户或域帐户的密码。

**usage： pspasswd [[\\computer[，computer[,..] | @file [-u 用户 [-p psswd]]]用户名 [NewPassword]**

| 参数            | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| **计算机**      | 对指定的远程计算机或计算机执行命令。 如果省略计算机名称，该命令在本地系统上运行，并且指定通配符 (\\*) ，该命令将在当前域中的所有计算机上运行。 |
| **@file**       | 在指定的文本文件中列出的每台计算机上运行该命令。             |
| **-u**          | 指定用于登录到远程计算机的可选用户名。                       |
| **-p**          | 指定用户名的可选密码。 如果省略此项，系统会提示输入隐藏的密码。 |
| **用户名**      | 指定密码更改的帐户名称。                                     |
| **NewPassword** | 新密码。 如果应用了 NULL 密码。                              |

![image-20220530162121520](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qi8yjgjzj21hl0u0dnl.jpg)

## PsService

在本地系统上显示 (运行和停止) 配置的服务。

```
用法：psservice [\\computer [-u username] [-p password]] <commandoptions><>
```



| 参数           | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| **query**      | 显示服务的状态。                                             |
| **config**     | 显示服务的配置。                                             |
| **setconfig**  | 设置服务 (禁用、自动、需求) 的启动类型。                     |
| **start**      | 启动服务。                                                   |
| **stop**       | 停止服务。                                                   |
| **restart**    | 停止，然后重启服务。                                         |
| **pause**      | 暂停服务                                                     |
| **续**         | 恢复暂停的服务。                                             |
| **依赖**       | 列出依赖于指定的服务。                                       |
| security       | 转储服务的安全描述符。                                       |
| **find**       | 在网络中搜索指定的服务。                                     |
| **\\computer** | 面向指定的 NT/Win2K 系统。 如果安全凭据不允许从远程系统获取性能计数器信息，请包括具有用户名和密码的 -u 开关以登录到远程系统。 如果指定 -u 选项，但未使用 -p 选项的密码， *PsService* 将提示输入密码，并且不会将其回显到屏幕。 |

![image-20220530162419778](https://tva1.sinaimg.cn/large/e6c9d24ely1h2qic48sdrj217e0u079t.jpg)

##  PsShutdown

PsShutdown启动本地或远程计算机的关闭、注销用户、锁定系统或中止即将关闭。

## pssuspend

暂停本地或远程系统上的进程

![image-20220604144125421](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w7h5x425j218h0u0q9n.jpg)

## rammap

RAMMap 是一种高级物理内存使用情况分析实用工具，适用于 Windows Vista 及更高版本

![image-20220604144321648](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w7ij52qmj212i0u0tdn.jpg)

## rdcman

RDCMan 管理多个远程桌面连接。 它可用于管理服务器实验室，你需要定期访问每台计算机

![image-20220604144358862](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w7j5vom8j215j0u0t9s.jpg)

## regdelnull

此命令行实用工具搜索并允许您删除包含嵌入的空字符的注册表项，否则这些注册表项将无法使用标准注册表编辑工具进行删除

regdelnull <路径> [-s]

-s 递归子项

![image-20220604151544696](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8g8trlij20vo090wfr.jpg)

## ru

Ru（注册表使用情况）报告您指定的注册表项的注册表空间使用情况。默认情况下，它递归子项以显示键及其子项的总大小。

ru [-c[t]] [-l <levels> | -n | -v] [-q] <绝对路径> 

ru [-c[t]] [-l <levels> | -n | -v] [-q] -h <hive 文件> [相对路径] 

| -C   | 将输出打印为 CSV。 指定 -ct 进行制表符分隔。           |
| ---- | ------------------------------------------------------ |
| -H   | 加载指定的 hive 文件，执行大小计算，然后卸载并压缩它。 |
| -l   | 指定信息 (默认值为一级) 的子项深度。                   |
| -n   | 不要递归。                                             |
| -q   | 安静 (没有横幅) 。                                     |
| -z   | 显示所有子项的大小。                                   |

## sdelete

可以使用 *SDelete* 安全地删除现有文件，也可以安全地擦除磁盘未分配部分中存在的任何文件数据

```
sdelete [-p passes] [-r] [-s] [-q] <文件或目录> ... 
sdelete [-p passes] [-z|-c [percent free]] <驱动器号 ...> sdelete [-p passes] [-z|-c] <physical disk number>
```



| -c        | 清理可用空间。指定可用空间量，供正在运行的系统使用。 |
| --------- | ---------------------------------------------------- |
| -p        | 指定默认为 1) (覆盖传递数。                          |
| -r        | 删除Read-Only属性。                                  |
| -s        | 递归子目录。                                         |
| -z        | 零可用空间 (适用于虚拟磁盘优化) 。                   |
| -nobanner | 不显示启动横幅和版权消息。                           |

## ShareEnum

列出可在网络上查看的共享及其安全设置

![image-20220604151829513](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8j2tlznj20sc0jojst.jpg)

## sigcheck

显示文件版本号、时间戳信息和数字签名详细信息，包括证书链。 它还包括一个选项，用于检查 [VirusTotal](https://www.virustotal.com/) 上的文件状态、针对 40 多个防病毒引擎执行自动文件扫描的网站，以及上传文件以供扫描的选项。

![image-20220604152117597](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8m0b966j20vg0u0449.jpg)

## streams

默认情况下，所有数据都存储在文件的主未命名数据流中，但通过使用语法“file：stream”，您可以读取和写入替代项

```
streams [-s] [-d] <file or directory>
```



| -s   | 递归子目录 |
| ---- | ---------- |
| -d   | 删除流     |

![image-20220604152246162](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8nja00qj212s0a4myh.jpg)

## strings

扫描传递的文件，以查找默认长度为 3 个或更多 UNICODE（或 ASCII）字符的 UNICODE（或 ASCII）字符串

![image-20220604152408020](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8oy9uxoj20yy08at9w.jpg)

## sync

确定修改后的文件数据是否安全地存储在硬盘驱动器上时，请使用它。遗憾的是，同步需要管理权限才能运行。此版本还允许您刷新可移动驱动器，如 ZIP 驱动器。

刷新移动驱动器：sync.exe -r

![image-20220604152448962](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8pntawfj20pw09cjsb.jpg)

弹出可移动驱动器：sync -e

## sysmon

 Windows 系统服务和设备驱动程序，一旦安装在系统上，它将在系统重新启动后保持驻留状态，以监视系统活动并将其记录到 Windows 事件日志中。它提供有关进程创建、网络连接和对文件创建时间的更改的详细信息

![image-20220604152606811](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8r0hbatj216c0u0tj8.jpg)

## tcpvcon

显示系统上所有TCP和UDP端点的详细列表，包括TCP连接的本地和远程地址以及状态

![image-20220604152640746](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8rlpfudj20u00unq6q.jpg)

## tcpview

显示系统上所有TCP和UDP端点的详细列表，包括TCP连接的本地和远程地址以及状态

![image-20220604152738430](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8slsaqjj21830u0tfb.jpg)

## vmmap

显示了进程的已提交虚拟内存类型以及操作系统分配给这些类型的工作集) (工作集的物理内存量细目。 除了内存使用情况的图形表示形式外，VMMap 还显示摘要信息和详细的进程内存映射。 借助强大的筛选和刷新功能，可以识别进程内存使用情况的来源和应用程序功能的内存成本。

![image-20220604152831974](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8tj42xqj20u00vx7ak.jpg)

![image-20220604152910220](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8u6s5orj20u00vy7bn.jpg)

## volumeid

VolumeID 允许您更改 FAT 和 NTFS 磁盘（软盘或硬盘驱动器）的 ID

## whois

Whois 会为您指定的域名或 IP 地址执行注册记录。

![image-20220604153006876](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8v6hpeaj21bj0u0doz.jpg)

## winobj

使用本机 Windows NT API（由 NTDLL .DLL 提供）来访问和显示有关 NT 对象管理器命名空间的信息

![image-20220604153034723](https://tva1.sinaimg.cn/large/e6c9d24egy1h2w8vnsxavj21830u0jxv.jpg)

## ZoomIt

ZoomIt是一个屏幕缩放和注释工具，用于包括应用程序演示在内的技术演示