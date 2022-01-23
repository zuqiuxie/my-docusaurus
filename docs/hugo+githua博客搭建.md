## 准备

### 安装brew

> 打开终端，执行下面的命令。

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```



---



## hugo

### 步骤1:安装hugo

```
$ brew install hugo
```

#### 验证安装

```
$ hugo version
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8c5tw9gqj30vo0kajsf.jpg" alt="image-20200427150228739"  />



### 步骤2:建立新网站

> 新建网站文件 blog

```
$ hugo new site blog
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cev22wuj30vo0kaqbf.jpg" alt="image-20200427151113426"  />

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cfsraevj30me046aby.jpg" alt="image-20200427151209200"  />



### 步骤3：添加主题[ ](https://gohugo.io/getting-started/quick-start/#step-3-add-a-theme)

> 请参阅[themes.gohugo.io](https://themes.gohugo.io/)以获取要考虑的主题列表。本快速入门使用了精美的[Ananke主题](https://themes.gohugo.io/gohugo-theme-ananke/)。

#### 例如：m10c主题

![image-20200427151453707](https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cinn96bj31gb0u0qh8.jpg)

> 将此存储库克隆到`themes/`目录中：

```
$ git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cn17ahtj30vo0ka0z2.jpg" alt="image-20200427151905845"  />



### 步骤4：添加一些内容

>  您可以手动创建内容文件（例如作为`content//.`）并在其中提供元数据，但是您可以使用该`new`命令为您做一些事情（例如添加一篇 blog1.md文件）：

```
hugo new posts/blog1.md
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cqtu3l0j30vo0kawlt.jpg" alt="image-20200427152245539"  />

> 在目录 `blog/content/posts`中生成 `bolg1.md`文件

#### 编辑`bolg1.md`文件，添加第一篇博客

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8cw3l7ofj30u00xjgse.jpg" alt="image-20200427152748227"  />



### 步骤5：启动Hugo服务器

>  现在，在本地以m10c主题启动Hugo服务器：

```
hugo server -t m10c --buildDrafts
```

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8czgrpoqj30vo0ka7cv.jpg" alt="image-20200427153103751"  />

> 浏览器输入网址
>
> ​      显示成功

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8d0l5anaj31gb0u0wn3.jpg" alt="image-20200427153205341"  />

### 步骤6：将网站推上远端(githua)

> 根据githua库设置地址
>
> `hugo --theme=<主题》 --baseUrl="<库地址>" --buildDrafts`

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1ge8d7vu6vvj31gb0u07p0.jpg" alt="image-20200427153908190"  />

```
hugo --theme=m10c --baseUrl="https://tjx158589.github.io/" --buildDrafts
```



> 主目录生成需上传远端文件夹`public`

> git 上传操作

```
cd public

git init

git add .

git commit -m "我的 hugo 博客第一次提交"

git remote add origin https://github.com/tjx158589/tjx158589.github.io.git


git push -u origin master
```

### 步骤7：完成打开网站

