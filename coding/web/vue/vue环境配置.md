# node.js安装和配置
## 下载安装node.js
官网下载最新版本：https://nodejs.org/en/download/
下载zip文件找个目录保存，解压下载的文件，然后配置环境变量，将解压文件所在的路径配置到环境变量中。
<img width="1340" height="660" alt="image" src="https://github.com/user-attachments/assets/d0708be4-c188-424e-b987-db2c4e727138" />

## 安装
下载完成后，双击安装包
<img width="499" height="389" alt="image" src="https://github.com/user-attachments/assets/e914c037-40dc-4bfe-951a-02dd776bea01" />
直接点【Next】按钮，此处可根据个人需求修改安装路径，修改完毕后继续点击【Next】按钮
<img width="499" height="389" alt="image" src="https://github.com/user-attachments/assets/1d2293c8-b2f2-4da1-89b0-ab0a414e3956" />
选择默认安装，继续点击【Next】按钮
<img width="484" height="371" alt="image" src="https://github.com/user-attachments/assets/8d8d080a-133d-4082-b654-4ab838001c1f" />
不选中，直接点击【Next】按钮
<img width="483" height="376" alt="image" src="https://github.com/user-attachments/assets/d049891d-e0b1-4bbe-86cf-636a51d71d5c" />
点击【Install】按钮进行安装
<img width="485" height="373" alt="image" src="https://github.com/user-attachments/assets/56c04947-8041-45e8-ae49-5ae8af4d14a1" />

## 测试是否成功
由于Node.js 中默认安装了 npm，所以不用额外配置就能在全局命令中使用 npm命令，在cmd中测试一下是否安装成功了：输入 node -v 与 npm –v分别查看版本信息
<img width="252" height="89" alt="image" src="https://github.com/user-attachments/assets/ebaf7bb3-aad7-4c22-a7e6-d19a6a9d472b" />

## 配置默认安装目录和缓存日志目录
说明：这里的环境配置主要配置的是npm安装的全局模块所在的路径，以及缓存cache的路径，
之所以要配置，是因为以后在执行类似：npm install express [-g] （后面的可选参数-g，g代表global全局安装的意思）的安装语句时，会将安装的模块安装到【C:\Users\用户名\AppData\Roaming\npm】路径中，占C盘空间。

### 创建默认安装目录和缓存日志目录
比如，我希望将全模块所在路径和缓存路径，放在我node.js安装的文件夹中，
在我安装的文件夹【"D:\Program Files \nodejs】下创建两个文件夹【node_global】及【node_cache】
分别作为默认安装目录和缓存日志目录。
<img width="290" height="174" alt="image" src="https://github.com/user-attachments/assets/28b5c917-e50f-4c9e-be74-65a40e48bce0" />

### 执行命令，将npm的全局模块目录和缓存目录配置到我们刚才创建的那两个目录：
```bash
npm config set prefix "D:\Program Files\nodejs\node_global"
npm config set cache "D:\Program Files\nodejs\node_cache"
```
npm config get prefix查看npm全局安装包保存路径
npm config get cache查看npm装包缓存路径
还可以输入 npm list -global 命令来查看全局安装目录：
npm config list查看所有npm 配置
<img width="819" height="137" alt="image" src="https://github.com/user-attachments/assets/592d7825-4724-447f-b323-61666d518f5e" />

## node.js环境配置
说明：以下D:\Program Files\nodejs为我的node的安装路径，记得改成你们自己的路径
“我的电脑”-右键-“属性”-“高级系统设置”-“高级”-“环境变量”，进入环境变量对话框

### 【系统变量】下新建【NODE_PATH】，此处设置第三方依赖包安装目录
如果跟着第2步修改了全局安装目录，则输入【D:\Program Files\nodejs\node_global\node_modules 】
<img width="621" height="176" alt="image" src="https://github.com/user-attachments/assets/0c8870e9-2cee-4096-a844-8defb1331f08" />

### 【系统变量】下的【Path】添加上node的路径【D:\Program Files\nodejs\】

### 如果设置了全局安装目录
【用户变量】下的【Path】将默认的 C 盘下 APPData/Roaming\npm 修改为【D:\Program Files\nodejs\node_global】，【D:\Program Files\nodejs\node_cache】，这是nodejs默认的模块调用路径

## 切换国内下载源
### 配置淘宝镜像源
查看npm下载源 npm config get registry
将npm的模块下载仓库从默认的国外站点改为国内的站点，这样下载模块的速度才能比较快，现在用的都是淘宝镜像源（https://registry.npm.taobao.org），使用淘宝镜像源有两种方式：

#### 临时使用
```bash
npm --registry https://registry.npm.taobao.org install cluster
```
这个代码就是只在安装cluster的使用淘宝镜像下载，每次安装一个模块都用挺长的代码，比较繁琐，所以推荐第二种方式。

#### 永久使用
这里有也两种配置选择，一是直接修改npm命令的仓库地址为淘宝镜像源，另一种是安装cnpm命令。
第一种:直接修改npm的默认配置
```bash
npm config set registry https://registry.npm.taobao.org
```
验证：配置后可以根据 npm config get registry或 npm config list 命令查看npm下载源是否配置成功，如图所示配置前为"https://registry.npmjs.org/"
<img width="730" height="280" alt="image" src="https://github.com/user-attachments/assets/531d2032-7635-40c1-ad21-8504ac6bce33" />
配置后为淘宝镜像，表示配置成功
<img width="735" height="432" alt="image" src="https://github.com/user-attachments/assets/8ba01055-425b-4445-b65e-2d386fdedac2" />

### 第二种：安装cnpm
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
验证方式变成了cnpm config get registry 或 cnpm config list如图所示
<img width="567" height="357" alt="image" src="https://github.com/user-attachments/assets/30238918-3dfe-4d01-8acb-8a99a19d4240" />

# 安装vue及脚手架
### 安装vue.js
```bash
npm install vue -g
或
cnpm install vue -g
```
根据自己的淘宝镜像源设置选择命令，其中-g是全局安装，指安装到global全局目录去
<img width="498" height="83" alt="image" src="https://github.com/user-attachments/assets/31ce3035-58bd-4ec5-b1a2-af0c21060d49" />
查看安装的vue信息：
```bash
npm info vue 
或者
cnpm info vue
```
<img width="628" height="314" alt="image" src="https://github.com/user-attachments/assets/4efbf5ea-ea60-4bc9-9798-4d0444fc1d2b" />
查看安装的vue版本
```bash
npm list vue
```
<img width="377" height="77" alt="image" src="https://github.com/user-attachments/assets/7da29f27-ed8a-4c30-9e49-5b9bb01674b1" />

### 安装webpack模板
在命令行中运行命令 npm install webpack -g ，然后等待安装完成。
webpack 4x以上，webpack将命令相关的内容都放到了webpack-cli，
所以还需要安装webpack-cli：npm install --global webpack-cli，
安装成功后可使用webpack -v查看版本号。

### 安装脚手架vue-cli 2.x
```bash
npm install vue-cli -g

# 检查版本
vue --version或vue -V
```
<img width="816" height="223" alt="image" src="https://github.com/user-attachments/assets/0983f8ee-b1d3-4333-b626-69defcd00bb7" />

### 这里顺手安装上vue-router
```bash
npm install -g vue-router
```
可以看到我的安装目录如下
<img width="360" height="126" alt="image" src="https://github.com/user-attachments/assets/5653d85e-b479-46d3-af86-3c4dafc70bcd" />

### vue-cli2创建vue项目

#### 创建项目
（最好在cd到D盘的某个位置，即项目的路径，否则项目会新建在C:\Users\Administrator\，也可以直接在想要的项目路径下输入cmd）
```bash
vue init webpack 项目名
```
项目名不要取中文和大写字母。
```bash
进行一些配置：
Project name（工程名）:回车(含大写字母给我报错了，我给改了my-vue)
Project description（工程介绍）：回车
Author：作者名 ：回车
Vue build ==> （是否安装编译器）runtime-compiler、 runtime-only 都是打包方式，第二个效率更高；
Install vue-router ==> 是否要安装 vue-router，项目中肯定要使用到路由，所以Y 回车；
Use ESLint to lint your code ==> 是否需要ESLint检测代码，目前我们不需要所以 n 回车；
Set up unit tests ==> 是否安装 单元测试工具 目前我们不需要 所以 n 回车；
Setup e2e tests with Nightwatch ==>是否需要端到端测试工具目前我们不需要所以n回车；
Should we run npm install for you after the project has been created? (recommended) (Use arrow keys)==> 安装依赖npm install
回车；
```
<img width="918" height="304" alt="image" src="https://github.com/user-attachments/assets/356802ff-be0e-4600-a37b-b61017a52a20" />
此处省略了一部分截图
最后结果如下，项目初始化成功
<img width="733" height="193" alt="image" src="https://github.com/user-attachments/assets/272db8f1-631d-4cb1-b802-a770a850885e" />

#### 按提示代码进入到项目目录下并运行
```bash
npm run dev
```
<img width="767" height="305" alt="image" src="https://github.com/user-attachments/assets/1b9b1015-169c-49a0-a289-6e27cb460f8b" />
按提示打开地址http://localhost:8081， 打开网址如图所示
<img width="1920" height="1051" alt="image" src="https://github.com/user-attachments/assets/0551d778-ec4f-4594-864a-77830abba867" />
结束项目运行：ctrl+c，选择Y即可停止项目的运行。

# 安装vue-cli 3.x
## 卸载旧版本
```bash
卸载2.x版本 npm uninstall vue-cli -g
卸载3.x版本 npm uninstall @vue/cli -g
```

## 安装新版本
```bash
npm install @vue/cli –g
指定版本号 npm install -g @vue/cli@3.12.1 不指定版本号会默认安装最新的版本
```
<img width="885" height="127" alt="image" src="https://github.com/user-attachments/assets/b975af85-8ecc-45d9-87a9-cf33ad3bba20" />

## 新建项目
```bash
vue create 项目名
```
<img width="710" height="199" alt="image" src="https://github.com/user-attachments/assets/a03610fd-65b3-4548-80db-d5ee1d86e204" />
```bash
Please pick a preset=》选择一个配置:default默认有babel、eslint，Manually select features 手动配置。
选择手动配置，根据自己的需要选择，敲空格键配合方向键进行选择。
where do you prefer placing config for …=》配置放在哪里
In dedicated config files =》 每项配置有单独的文件
In package.json =》在package.json 文件中
Save this as a preset for future project? =>是否为保存配置习惯文件，存了后下次新建新项目选择配置时就会有此选项了
Save preset as; =>存个名字
Pick the package …=>运行选择npm或者yarn
```
配置成功
<img width="477" height="165" alt="image" src="https://github.com/user-attachments/assets/98a550f6-92e0-4418-aa5c-dd5aa5958ed2" />

## 运行项目
cd 到我们的项目目录， 然后使用npm run serve可以运行我们的项目
http://localhost:8080/ 打开就可以看到我们的运行的结果了，如图
<img width="731" height="390" alt="image" src="https://github.com/user-attachments/assets/a89dcc95-976d-49a3-8597-02558ac2b059" />


# cli3下拉取2.x模板
```bash
npm install -g @vue/cli-init
```
依然可以新建2.x的项目 vue init webpack my-vue

# 开发工具
## 用VS查看vue代码
最好使用编码工具查看编写代码，我用的vs code ，安装见https://blog.csdn.net/dream_summer/article/details/108872293，下面讲如何使用Visual Studio Code查看vue代码

### 在VS code 中启动项目
创建完项目后，首先用VS打开项目所在的文件夹，点击工具栏的终端——新建终端，在下面的终端窗口命令行输入 npm run serve启动，编译成功后会自动打开浏览器
<img width="368" height="385" alt="image" src="https://github.com/user-attachments/assets/3f67dda9-7984-4a18-b5d2-003cac7e3758" />
项目中新建vue.config.js 文件，更改端口号为8089
```bash
module.exports=
{//扩展配置
    configureWebpack:
    {
        devServer:
        {
            port:8089,//更改端口号
            open:true,//自动启动浏览器
            //mock数据
            before(app)
            {
            }
        }
    }
}
```
保存后，在终端启动，端口号变为8089并自动打开了浏览器http://localhost:8089
如图
<img width="480" height="320" alt="image" src="https://github.com/user-attachments/assets/92afd5ff-accf-4da3-9ff8-8f4c54cbaed4" />
<img width="523" height="446" alt="image" src="https://github.com/user-attachments/assets/cfbe912b-9f5f-49a3-bb74-5715df7dd49d" />

## Hbuilder X
### 安装
从HbuildX官网（http://www.dcloud.io/hbuilderx.html）下载hbuildx安装文件（已提供，无需下载），下载后，直接解压就可以使用。
直接解压到软件安装目录

### 新建VUE项目
<img width="771" height="570" alt="image" src="https://github.com/user-attachments/assets/00656a10-5efd-4a9c-8451-85f43dc24da0" />

### Node配置（配置运行许可）
选中新建的项目点击工具栏运行-运行到终端-运行设置
填写最下面的npm、node运行配置，如下图：
<img width="1563" height="711" alt="image" src="https://github.com/user-attachments/assets/3d35a206-6123-42f3-a2a9-6308db8daeb4" />

### 运行工程
选中工程，点击右键-外部命令
```bash
npm run build 编译该工程
npm run serve 启动该工程
```
启动成功之后，打开路径看到如下界面说明成功。
<img width="1920" height="915" alt="image" src="https://github.com/user-attachments/assets/2b2d777c-6bc4-49df-b7db-23becf4db7b5" />

# vue项目结构

## build：构建脚本目录
1. build.js ==> 生产环境构建脚本；
2. check-versions.js ==> 检查npm，node.js版本；
3. utils.js ==> 构建相关工具方法；
4. vue-loader.conf.js ==> 配置了css加载器以及编译css之后自动添加 前缀；
5. webpack.base.conf.js ==> webpack基本配置；
6. webpack.dev.conf.js ==> webpack开发环境配置；
7. webpack.prod.conf.js ==> webpack生产环境配置；

## config：项目配置
1. dev.env.js ==> 开发环境变量；
2. index.js ==> 项目配置文件；
3. prod.env.js ==> 生产环境变量；

## node_modules：npm 加载的项目依赖模块

## src：这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：
1. assets：资源目录，放置一些图片或者公共js、公共css。但是因为它们属于代码目录下，所以可以用 webpack 来操作和处理。意思就是你可以使用一些预处理比如 Sass/SCSS 或者 Stylus。
2. components：用来存放自定义组件的目录，目前里面会有一个示例组件。
3. router：前端路由目录，我们需要配置的路由路径写在index.js里面；
4. App.vue：根组件；这是 Vue 应用的根节点组件，往下看可以了解更多关注 Vue 组件的信息。
5. main.js：应用的入口文件。主要是引入vue框架，根组件及路由设置，并且定义vue实例，即初始化 Vue 应用并且制定将应用挂载到index.html 文件中的哪个 HTML 元素上。通常还会做一些注册全局组件或者添额外的 Vue 库的操作。

## static：静态资源目录，如图片、字体等。不会被webpack构建

## index.html：
首页入口文件，可以添加一些 meta 信息等。 这是应用的模板文件，Vue 应用会通过这个 HTML 页面来运行，也可以通过 lodash 这种模板语法在这个文件里插值。
注意：这个不是负责管理页面最终展示的模板，而是管理 Vue 应用之外的静态 HTML 文件，一般只有在用到一些高级功能的时候才会修改这个文件。

## package.json：npm包配置文件，定义了项目的npm脚本，依赖包等信息

## README.md：项目的说明文档，markdown 格式
## .xxxx文件：这些是一些配置文件，包括语法配置，git配置等
1. .babelrc:babel编译参数
2. .editorconfig:编辑器相关的配置，代码格式
3. .eslintignore : 配置需要或略的路径，一般build、config、dist、test等目录都会配置忽略
4. .eslintrc.js : 配置代码格式风格检查规则
5. .gitignore:git上传需要忽略的文件配置
6. .postcssrc.js ：css转换工具

# 报错
## npm install vue -g 报错
内容：
```bash
npm ERR! code EPERM
npm ERR! syscall mkdir
npm ERR! path C:\Program Files\nodejs\node_cache\_cacache
npm ERR! errno -4048
npm ERR! Error: EPERM: operation not permitted, mkdir 'C:\Program Files\nodejs\node_cache\_cacache'
npm ERR!  [OperationalError: EPERM: operation not permitted, mkdir 'C:\Program Files\nodejs\node_cache\_cacache'] {
npm ERR!   cause: [Error: EPERM: operation not permitted, mkdir 'C:\Program Files\nodejs\node_cache\_cacache'] {
npm ERR!     errno: -4048,
npm ERR!     code: 'EPERM',
npm ERR!     syscall: 'mkdir',
npm ERR!     path: 'C:\\Program Files\\nodejs\\node_cache\\_cacache'
npm ERR!   },
npm ERR!   isOperational: true,
npm ERR!   errno: -4048,
npm ERR!   code: 'EPERM',
npm ERR!   syscall: 'mkdir',
npm ERR!   path: 'C:\\Program Files\\nodejs\\node_cache\\_cacache'
npm ERR! }
npm ERR!
npm ERR! The operation was rejected by your operating system.
npm ERR! It's possible that the file was already in use (by a text editor or antivirus),
npm ERR! or that you lack permissions to access it.
npm ERR!
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.
```
解决方法：
1. 删除.npmrc文件
```bash
该文件在：C:\Users{账户}\下的.npmrc文件，
一般这种类型的都是默认被隐藏，一定要选择将隐藏取消掉
```
2. 或者直接用命令清理就行，控制台输入：
```bash
npm cache clean --force
```

## vue-cli2创建vue项目报错：vue init webpack 项目名
报错内容：
```bash
vue : 无法加载文件 C:\Users\hp\AppData\Roaming\npm\vue.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中

的 about_Execution_Policies。

所在位置 行:1 字符: 1

+ vue init webpack demo2

+ ~~~

    + CategoryInfo          : SecurityError: (:) []，PSSecurityException

    + FullyQualifiedErrorId : UnauthorizedAccess 作者：丘奇小怪 https://www.bilibili.com/read/cv15758057 出处：bilibili
```
解决方法：
```bash
以管理员权限打开windows powershell，
输入set-ExecutionPolicy RemoteSigned，
选择y或者a即可
```

## npm install @vue/cli –g 报错
报错内容
```bash
npm ERR! arg Argument starts with non-ascii dash, this is probably invalid: –g
npm ERR! code EINVALIDTAGNAME
npm ERR! Invalid tag name "–g" of package "–g": Tags may not have any characters that encodeURIComponent encodes.

npm ERR! A complete log of this run can be found in: C:\Users\lhw\AppData\Local\npm-cache\_logs\2023-08-26T01_50_15_506Z-debug-0.log
```
报错原因：参数使用了非ASCII字符
解决方法：
指令手动输入一遍，可能是 -g 的 g符号错误



























