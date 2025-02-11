



















# 一、搭建本地hugo站点



1、首先要创建一个站点

hugo new site quickstart 



2、将该目录（quickstart）作为项目的根目录,

cd quickstart



3、引入主题

把下载好的主题放到quickstart\themes目录中



4、运行服务

hugo server





# 二、在github page上托管hugo站点



1、创建github仓库

命名为 username/github.io	Public



2、根据提示初始化仓库

**…or create a new repository on the command line**

```
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/fangdabai/test.git
git push -u origin main
```

**…or push an existing repository from the command line**

```
git remote add origin https://github.com/fangdabai/test.git
git branch -M main
git push -u origin main
```

推送站点到远程

~~~
git remote -v
git add .
git commit -m "推送站点文件"
git push origin main 
~~~



3、settings->  Pages -> Github Actions

然后根据提示 configure hugo站点，会自动生成\\.github\workflows\hugo.yml的配置文件

远程分支拉取到本地，保持同步

```
git pull origin main
```



此时直接访问

xxx.github.io即可



# 三、hugo配置个人域名



我采用的方式比较简单，通过修改Github Pages上的Custom domain 直接保存即可
通过dns把blog.heibai.store 解析到fangdabai.github.io上



在hugo配置文件中把baseurl设置成域名blog.heibai.store























# 其他



### powshell走v2rayN代理

PS C:\Users\fxb\Desktop\HugoProject> $env:HTTP_PROXY = "socks5://127.0.0.1:10808"
PS C:\Users\fxb\Desktop\HugoProject> $env:HTTPS_PROXY = "socks5://127.0.0.1:10808"
PS C:\Users\fxb\Desktop\HugoProject> curl www.google.com





### 网页图标

SVG Icon: https://fontawesome.com/search

[图片转 ico 图标 - 锤子在线工具](https://www.toolhelper.cn/Image/ImageToIco)





### http资源无法加载问题

[解决https页面请求http出现Mixed Content.This request has been blocked； the content must be served over HTTPS._mixed content this request has been blocked; the c-CSDN博客](https://blog.csdn.net/z_johnny/article/details/143636824)





### google_analytics_async模板问题

你的 Hugo 项目在构建时出现了多个错误，主要是因为 `_internal/google_analytics_async.html` 模板已被移除，但你的主题或模板仍在引用它。以下是解决这些错误的步骤：

------

#### **1. 问题分析**

- `_internal/google_analytics_async.html` 是 Hugo 的内置模板，但它在最近版本中被移除。
- 你的主题 `JuiceBar` 中的 `partials/head.html` 文件仍然在引用这个已移除的模板。

关键错误信息：

```
error calling partial: execute of template failed: html/template:partials/head.html:16:16: no such template "_internal/google_analytics_async.html"
```

#### **2. 解决方法**

**方法 1：移除对 `_internal/google_analytics_async.html` 的引用**

1. 打开主题目录：

   ```
   themes/JuiceBar/layouts/partials/head.html
   ```

2. 查找以下代码（大致在第 16 行）：

   ```
   {{ template "_internal/google_analytics_async.html" . }}
   ```

3. **注释掉或删除这行代码**。

   例如：

   ```
   {{/* template "_internal/google_analytics_async.html" . */}}
   ```

4. 保存文件并重新构建站点：

   ```
   hugo
   ```

**方法 2：手动添加 Google Analytics 脚本**

如果你需要继续使用 Google Analytics，替换已移除的 `_internal/google_analytics_async.html` 模板。

1. 在 `head.html` 中手动添加 Google Analytics 的跟踪代码：

   ```
   html复制代码<script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXX-X"></script>
   <script>
     window.dataLayer = window.dataLayer || [];
     function gtag(){dataLayer.push(arguments);}
     gtag('js', new Date());
     gtag('config', 'UA-XXXXXXX-X');
   </script>
   ```

   其中，将 `UA-XXXXXXX-X` 替换为你的 Google Analytics 跟踪 ID。





### hugo更新问题

暂时未更新





### hugo配置个人域名

> 参考：https://zhuanlan.zhihu.com/p/422859066
> 			https://cloud.tencent.com/developer/article/1834163
>
> NameCheap域名解析：https://zhuanlan.zhihu.com/p/33261777

1、当你拥有域名后，首先是用 ping 命令找到存放你的 GitHubPages 的主机的IP地址，在终端里面用命令 ping yourname.github.io 便可完成，回复地址即我们要找的IP地址

2、在购买域名的提供商为域名添加解析，添加两条记录

+ 记录类型：CNAME 将一个域名指向另一个域名，需要添加 CNAME 记录（yourname.github.io）。 主机记录：www 表示访问域名的时候以 www 开头为一级域名。如果是二级域名的话就在前面加上自己想要的参数，访问的时候也是以二级域名的形式访问

+ 记录类型：A 将域名指向一个IPv4地址，如果需要将域名指向一个 IP 地址（刚才我们 ping 获得的IP地址），就需要添加 A 记录。 
  主机记录：@ 表示访问的时候直接访问，前面不加任何参数

3、在本地仓库根目录中创建 CNAME 文件，内容是你的域名（不要加http头），并修改hugo配置文件

~~~bash
touch CNAME
echo "gaiety.store" > CNAME
~~~

修改Hugo 根目录中的配置文件config.toml

~~~bash
# baseURL = 'https://fangdabai.github.io/'
baseURL = 'https://gaiety.store/'
~~~

在 Hugo 根目录中的配置文件中填写 `baseURL` 时，应该选择适合你网站部署情况的那一个，即根据你的网站是部署在 GitHub Pages 还是自己的服务器上。

- 如果你的网站部署在 GitHub Pages 上，则 `baseURL` 应该填写为 `'https://fangdabai.github.io/'`，因为 GitHub Pages 使用的是以用户名为子域名的地址，所以你的网站将通过这个地址访问。
- 如果你使用了自定义域名（比如 `gaiety.store`），并且你的网站是部署在 GitHub Pages 上并绑定了该自定义域名，则 `baseURL` 应该填写为 `'https://gaiety.store/'`，因为你希望网站被访问时使用自定义域名而不是 GitHub Pages 的默认域名。

填写不同的 `baseURL` 会影响 Hugo 生成网站时生成的链接地址和资源引用的路径，确保填写正确的 `baseURL` 可以保证你的网站能够正常访问和显示。

4、同步到远程仓库

~~~bash
git add .
git commit -m "message"
git push
~~~

5、打开仓库 Repository Settings 的 Pages ，我们发现 Custom domain 变为我们的域名，若没有自行更改即可，选择 Enforce HTTPS 为你的网站添加小绿锁，配置后需要等一会才可以生效~

6、设置完成后就可以通过你的域名访问部署在GitHub上的Hugo的网站啦
