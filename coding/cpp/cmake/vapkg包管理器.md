# 什么是是vcpkg
vcpkg是c++的一个跨平台的包管理器，可以很方便的管理第三方库。
团队协作：项目开发成员之间的库版本统一（使用 vcpkg.json配置文件 控制第三方库的版本统一）
库管理：方便的安装和卸载第三方库文件（使用vcpkg指令可以快速安装和删除）
管理同个库的不同版本（在项目中，由vcpkg.json决定具体使用的版本）
项目构建时，vcpkg 会自动处理依赖关系，确保所有需要的库都已正确下载和链接

# 重要文件
- vcpkg.json 和 vcpkg-configuration.json文件：项目初始化后，自动生成的项目 第三方库配置文件
- vcpkg_installed 文件夹：保存编译后的第三方库 头文件(include)，静态链接文件(lib)，动态链接文件(dll)。这个文件夹会出现在项目文件夹下，建议不要同步
- vcpkg/downloads 文件夹：保存下载的第三方库源码，补丁等
- vcpkg/ports/：保存有所有第三方库的配置文件：下载网址，所需文件，版本等
- vcpkg/ports/库名/portfile.cmake 文件：第三方库文件的配置：下载网址，所需文件，版本，文件校验

推荐将 vcpkg.json 和 vcpkg-configuration.json文件进行git同步，保证团队成员之间第三方库的版本一致
可以选择 不要同步 vcpkg_installed 文件夹，此文件夹下保存了所有下载的第三方库文件，会大大增加源码包的体积

# 第三方库结构
## 第三方库的一般内容
第三方库一般包含库的头文件(.h)，源文件(.cpp)，链接文件(动态dll 或 静态llb)
部分库只有头文件，或只有链接文件
```bash
minizip/
├── include/
│   ├── minizip.h          # 头文件
│   ├── minizip_utils.h    # 其他头文件

├── lib/
│   ├── minizip.lib        # Windows 静态库
│   ├── minizip.a          # Linux 静态库
│   └── ...                # 其他库文件（如动态库 .dll 或 .so 文件）
├── src/                   # 源代码（可选）
│   ├── minizip.cpp        # 源文件
│   └── ...                # 其他源文件
├── examples/              # 示例代码（可选）
│   ├── example.cpp        # 示例源文件
│   └── ...
└── README.md              # 使用说明文件
```

# 安装vcpkg

## 安装位置
vcpkg 安装位置随意，跨平台时也是
库文件安装在 vcpkg的安装文件夹内，使用相对路径（vcpkg执行文件）进行链接

## windows下安装
### 安装git
请自行解决

### 创建一个文件夹用于安装vcpkg

### 打开cmd，进入此文件夹
在文件管理中打开新建的文件夹，地址栏输入cmd即可

### clone git上的vcpkg源码
```bash
git clone https://github.com/Microsoft/vcpkg
```
下载完成后，会自动建立一个vcpkg文件夹

### 进入vcpkg的源码文件夹，进行安装
执行下面的脚本即可完成安装
```bash
bootstrap-vcpkg.bat
```

### 链接系统内所有的c++编译器
```bash
vcpkg integrate install
```
指令执行结果：
<img width="708" height="170" alt="image" src="https://github.com/user-attachments/assets/118e3aa2-c8d4-4fe2-bccb-51ec100cf6c1" />

### 安装完成后，将 vcpkg 加入系统环境变量中
方便在终端中使用 vcpkg 指令
在path变量中添加 vcpkg的地址
添加 VCPKG_ROOT 变量，变量值位 vcpkg的地址（如果不添加，在安装库后，会在项目文件夹下创建一个 vcpkg_installed文件夹）

## ubuntu下安装
todo

# 卸载 vcpkg
### windows
直接删除安装目录即可

# 修改vcpkg第三方库镜像源
vcpkg默认的镜像源是微软在github上的库。访问速度极慢，挂梯子也不太行，当然不排除我网络有影响
建议修改到国内的镜像，但是国内镜像的库可能不是很全
贴一个原文链接，我这里只改了第三方库的下载地址，可能会不生效
原文链接：https://blog.csdn.net/weixin_41364246/article/details/140123907

## windows 修改第三方库下载地址
修改下载脚本----- vcpkg_download_distfile.cmake (在 vcpkg/scripts/cmake 文件夹下)
找到 vcpkg_download_distfile函数 ，在 vcpkg_list(SET urls_param) 后修改（大概在233行附近）
修改前：
<img width="1655" height="743" alt="image" src="https://github.com/user-attachments/assets/712a2fb9-eb92-4af4-a50b-b32d9437f3e9" />
修改后：
<img width="2008" height="1171" alt="image" src="https://github.com/user-attachments/assets/5e365f72-3495-47a4-bab5-7dbeb6f92a82" />

### 执行bootstrap-vcpkg.bat脚本(前面安装vcpkg的指令)
修改完成后，如果下载时出现网络代理问题，可以考虑是第三方库导致的
可以将对应第三方库修改为原来的库

## linux环境下修改vcpkg第三方库镜像源
vcpkg工具的路径，我的是/app/vcpkg/xxx，需要替换成你自己的

```bash
sed -i 's#https://github.com/#https://mirror.ghproxy.com/https://github.com/#g' /app/vcpkg/scripts/bootstrap.sh \
    && sed -i 's#https://github.com/#https://mirror.ghproxy.com/https://github.com/#g' /app/vcpkg/scripts/vcpkgTools.xml \
    && sed -z -i 's|    vcpkg_list(SET urls_param)\n    foreach(url IN LISTS arg_URLS)\n        vcpkg_list(APPEND urls_param "--url=${url}")\n    endforeach()\n    if(NOT vcpkg_download_distfile_QUIET)\n        message(STATUS "Downloading ${arg_URLS} -> ${arg_FILENAME}...")\n    endif()|    vcpkg_list(SET urls_param)\n    vcpkg_list(SET arg_URLS_Real)\n    foreach(url IN LISTS arg_URLS)\n        string(REPLACE "http://download.savannah.nongnu.org/releases/gta/" "https://marlam.de/gta/releases/" url "${url}")\n        string(REPLACE "https://github.com/" "https://mirror.ghproxy.com/https://github.com/" url "${url}")\n        string(REPLACE "https://ftp.gnu.org/" "https://mirrors.aliyun.com/" url "${url}")\n        string(REPLACE "https://raw.githubusercontent.com/" "https://mirror.ghproxy.com/https://raw.githubusercontent.com/" url "${url}")\n        string(REPLACE "http://ftp.gnu.org/pub/gnu/" "https://mirrors.aliyun.com/gnu/" url "${url}")\n        string(REPLACE "https://ftp.postgresql.org/pub/" "https://mirrors.tuna.tsinghua.edu.cn/postgresql/" url "${url}")\n        string(REPLACE "https://support.hdfgroup.org/ftp/lib-external/szip/2.1.1/src/" "https://distfiles.macports.org/szip/" url "${url}")\n        vcpkg_list(APPEND urls_param "--url=${url}")\n        vcpkg_list(APPEND arg_URLS_Real "${url}")\n    endforeach()\n    if(NOT vcpkg_download_distfile_QUIET)\n        message(STATUS "Downloading ${arg_URLS_Real} -> ${arg_FILENAME}...")\n    endif()|g' /app/vcpkg/scripts/cmake/vcpkg_download_distfile.cmake

```

# 使用vcpkg

## 安装和引入第三方库

### 初始化项目包管理
项目已有vcpkg.json文件时，跳过这一步，直接安装库文件
在项目根文件夹下，执行：
```bash
vcpkg new --application # 初始化
```
这会生成一个 vcpkg.json 和 vcpkg-configuration.json文件，包含库文件的清单
vcpkg.json是vcpkg 的配置文件，包含项目中使用的第三方库的列表，库名，版本等
vcpkg-configuration.json 引入了基线约束，指定项目应使用的依赖项的最低版本，提供了项目级别的配置选项，可以为每个项目单独定义包的版本、使用的注册表和其他依赖包管理策略
团队开发时推荐将 两个文件 上传到 git进行同步，以同步团队中第三方库的版本

### 编辑vcpkg.json文件
安装时，vcpkg会自动寻找此文件并安装此文件内的库
使用 vcpkg.json文件，会进入清单模式（下面安装和卸载第三方库时会提到）
vcpkg.json的一般结构：
```json
{
  "name": "myproject",
  "version": "1.0.0",
  "dependencies": [
    {
      "name": "nlohmann-json",
      "version>=": "3.11.0"
    },
    {
      "name": "xlnt",
      "version>=": "1.5.0"
    }
  ]
}
```
- name：项目名
- version：项目版本号
- dependencies：第三方库列表
- dependencies / name：第三方库名
- dependencies / version>=：指定第三方库支持的最低版本号

### 安装第三方库

#### 普通模式
```bash
vcpkg search 库名 # 查看商店中是否存在指定库

vcpkg install 库名    # 非清单模式下，通过指定库名进行安装
```

#### 清单模式（项目中有 vcpkg.json 文件时）
在项目中已有 vcpkg.json 时，会遍历配置文件中的列表进行安装
当设备中已有相同库的不同版本时，会优先安装并使用 vcpkg.json 内指定的库版本
```bash
vcpkg search 库名 # 查看商店中是否存在指定库

vcpkg install     # 在清单模式下（使用vcpkg.json文件），安装第三方库文件，不能直接指定库名进行安装，需要在 vcpkg.json文件中添加库名，而不是指定库名安装
```

安装完成会生成一个 vcpkg_installed文件夹，这个文件夹不建议使用git同步（只同步 vcpkg.json 即可实现相同效果）
vcpkg 支持以下多种架构，默认编译成 x64 的 Windows 版本库。
-  arm-uwp
-  arm-windows
-  arm64-uwp
-  arm64-windows
-  x64-uwp
-  x64-windows-static
-  x64-windows
-  x86-uwp
-  x86-windows-static
-  x86-windows

### 在项目中引入第三方库
cmake构建的项目引入
CMakeLists文件：
#### 手动引入库路径
```bash
# 设置vcpkg路径
set(VCPKG_DIR "D:/opt/tools/cpp/vcpkg")  # 替换为实际vcpkg路径
set(VCPKG_INCLUDE "${VCPKG_DIR}/installed/x64-windows/include")    # vcpkg的第三方库 include文件夹
set(VCPKG_LIB "${VCPKG_DIR}/installed/x64-windows/lib")    # vcpkg的第三方库 lib文件夹

# 指定第三方库的头文件所在目录
include_directories(${VCPKG_INCLUDE}/库名)

# 指定库链接文件所在文件夹的路径（lib文件或dll文件）
link_directories(${VCPKG_LIB})

# 链接库（库名）
target_link_libraries(${PROJECT_NAME} PRIVATE 库名1 库名2)
```

#### 自动寻找所需库（可能会找不到）
```bash
if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)    # 检查变量是否在环境中定义
    # 设置 vcpkg 工具链文件路径
    set(CMAKE_TOOLCHAIN_FILE "D:/opt/tools/cpp/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "Vcpkg toolchain file")
endif()
# 设置 vcpkg安装的第三方库目录
set(CMAKE_PREFIX_PATH "${CMAKE_SOURCE_DIR}/vcpkg_installed/x64-windows")

# 在系统中寻找库
find_package(<PackageNam> REQUIRED)  # 替换为实际的库名称

# 需要指定第三方库的头文件所在目录（根目录即可）
include_directories(${vcpkg安装头文件目录} 其他头文件)

# 链接库
target_link_libraries(${PROJECT_NAME} PRIVATE 
    库名1 
    库名2
)

# 需要手动复制dll文件到可执行文件夹:
# 第三方库 dll列表
set(DLL_FILES
    "${VCPKG_DEBUG_BIN}/lua.dll"
)
# 遍历 dll列表，将所有 .dll 文件，逐一复制到可执行文件生成目录
foreach(DLL_FILE ${DLL_FILES})
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${DLL_FILE}"
        $<TARGET_FILE_DIR:${PROJECT_NAME}>  # 复制到可执行文件生成目录
    )
endforeach()
```

### 在项目中使用第三方库
```bash
#include <aaa.h> // 以 /installed/x64-windows/include 为相对路径
```

### 移除第三方库
默认移除 x86-winodws 版本库，如需移除其他版本库，需指定。2. 只移除库本身，源码包和解压后的源码并未移除
```bash
vcpkg remove 库名 # 卸载第三方库

# 在清单模式（使用vcpkg.json文件时）只需要在vcpkg.json文件内，删除对应库名，然后执行 vcpkg install
```

## 手动下载第三方库
有时可能会出现无法找到第三方库的情况，使用手动下载作为备用

### 查找第三方库源码的下载网址
查看 vcpkg中保存的 portfile.cmake 文件
```bash
vcpkg安装目录/ports/<库名>/portfile.cmake
```
打开该 portfile.cmake 文件，查找 vcpkg_download_distfile 函数，
URLS就是网址
可能也会有一个检验参数，用来检查下载的文件完整性，手动下载的文件可能无法通过
```bash
vcpkg_download_distfile(    
    PROTOBUF    
    URLS https://github.com/protocolbuffers/protobuf/releases/download/v3.17.3/protobuf-cpp-3.17.3.tar.gz    
    FILENAME protobuf-cpp-3.17.3.tar.gz    
    ...
)
```

### 下载源码
### 编译源码
将下载到的源码压缩包或文件，放入 vcpkg/downloads 文件夹
执行下面的指令
```bash
vcpkg install
```

# vcpkg解释
## vcpkg目录结构
- buildtrees  -  包含从中生成每个库的源的子文件夹。
- docs  -  文档和示例。
- download  -  已下载的工具或源的缓存副本。 运行安装命令时，vcpkg 会首先搜索此处。
- installed  -  包含每个已安装库的标头和库文件。 与 Visual Studio 集成时，相当于告知它将此文件夹添加到其搜索路径。
- packages  -  在不同的安装之间用于暂存的内部文件夹。
- ports  -  用于描述每个库的目录、版本和下载位置的文件。 如有需要，可添加自己的端口。
- scripts  -  由 vcpkg 使用的脚本（CMake、PowerShell）。
- toolsrc  -  vcpkg 和相关组件的 C++ 源代码。
- triplets  -  包含每个受支持目标平台（如 x86-windows 或 x64-uwp）的设置。
- versions

## vcpkg常用指令
```bash
# 常规情况下，我们编译或安装一些第三方库后，需要手动设置 include 目录、lib 目录、环境变量等
# vcpkg 提供了一套机制，可以全自动 VS 的适配目录
集成到环境指令：vcpkg integrate install
移除集成指令：vcpkg integrate remove

集成到工程：vcpkg integrate project（在“\scripts\buildsystems”目录下，生成nuget配置文件）

查看支持的开源库列表：vcpkg search
查看商店中是否存在指定库：vcpkg search 库名
查看支持的架构：vcpkg help triplet

指定编译某种架构的程序库：vcpkg install xxxx:x64-windows（x86-windows）

安装库：vcpkg install xxxx
安装指定版本：vcpkg install 库名:x64-windows
卸载已安装库：vcpkg remove xxxx

查看已经安装的库：vcpkg list
更新已经安装的库：vcpkg update xxx
更新并重新编译所有需要更新的包：vcpkg upgrade xxx

指定卸载平台：vcpkg remove xxxx:x64-windows
移除所有旧版本库：vcpkg remove --outdated

导出已经安装的库：vcpkg export xxxx --7zip（–7zip –raw –nuget –ifw –zip）
# 例如 导出 curl库：vcpkg export curl --7zip
# 默认导出 x86-windows 版本库
# 默认导出 vcpkg 目录下，默认导出包名称 vcpkg-export-日期-时间，如需指定目录和名称，使用 “-output=” 参数
# 导出必须指定包格式：
# --raw：以目录格式导出
# --nuget：以 nuget 格式导出
#　--ifw：未知
#　--zip：以 zip 格式导出
#　--7zip：以 7z 格式导出

导入已导出的开源库：vcpkg import xxx.7z

获取帮助：vcpkg help export
```

## vcpkg-configuration.json 文件解读
```json
{
  "registries": [
    {
      "kind": "git",
      "repository": "https://github.com/microsoft/vcpkg",
      "baseline": "abc123def456",  // 可以锁定某个提交或分支
      "packages": [ "nlohmann-json", "xlnt" ]
    }
  ],
  "default-registry": {
    "kind": "git",
    "baseline": "797ae22b76f0e78c3aeb03d625722a0cbfe9f52e",
    "repository": "https://github.com/microsoft/vcpkg"
  },
  "overlay-ports": [ "custom_ports/" ],
  "overlay-triplets": [ "custom_triplets/" ],
  "registries-cache-path": "path/to/cache"
}
```
- registries：定义自定义的包注册表，可以从其他 git 仓库拉取包。
- default-registry：定义默认使用的 vcpkg 注册表，通常是 vcpkg 的官方 GitHub 仓库。
- overlay-ports：指定自定义的 ports 目录，允许用户提供自己修改的包配置。
- overlay-triplets：自定义构建目标配置的目录。
- registries-cache-path：指定缓存预编译包的位置。

# vcpkg报错
## 项目中有vcpkg.json文件时，不能直接指定包名进行安装
### 报错内容：
```bash
error: In manifest mode, 
vcpkg install does not support individual package arguments.
    To install additional packages, edit vcpkg.json and then run 
vcpkg install without any package arguments.
    See https://learn.microsoft.com/vcpkg/users/manifests?WT.mc_id=vcpkg_inproduct_cli for more information.
```

### 报错原因：
由于使用了 vcpkg 的 Manifest Mode，这是 vcpkg 的一种管理依赖的模式，它通过项目中的 vcpkg.json 文件来管理所有包依赖，而不是通过命令行逐个手动安装包。
在 Manifest Mode 下，不能通过命令行直接指定要安装的单个包，如你通过 vcpkg install 命令带包名的方式会导致报错。相反，你需要将要安装的包添加到 vcpkg.json 文件中，然后运行 vcpkg install 来安装所有依赖包

### 解决：
#### 方法一：通过vcpkg.json进行安装
通过编辑vcpkg.json文件进行库文件安装，文件例子如下（安装 aaa 库）
```json
{
  "name": "my_project",
  "version": "1.0.0",
  "dependencies": [
    "aaa"
  ]
}
```
dependencies 数组中添加所有你需要的依赖包
然后执行指令进行安装：
```bash
vcpkg install
```

#### 方法二：退出 Manifest Mode 模式
删除项目中的 vcpkg.json文件即可退出此模式，但是不建议使用此方法，除非你的项目是刚刚创建的新项目

## 安装或查看镜像库时报错
### 报错内容
```bash
error: failed to fetch ref HEAD from repository https://github.com/microsoft/vcpkgfailed to execute: "D:\opt\IDE\Git\cmd\git.exe" "--git-dir=C:\Users\lhw\AppData\Local\vcpkg\registries\git\.git" "--work-tree=C:\Users\lhw\AppData\Local\vcpkg\registries\git" -c core.autocrlf=false fetch --update-shallow -- https://github.com/microsoft/vcpkg HEAD
error: git failed with exit code: (128).fatal: unable to access 'https://github.com/microsoft/vcpkg/': 
Failed to connect to github.com port 443 after 21113 ms: Couldn't connect to server
```

### 报错原因
网络异常，连接不到github服务器

### 解决
更换网络，梯子，或者使用国内的镜像源

## 编辑vcpkg.json后安装报错
### 报错内容
```bash
D:\Project\cpp\tool\excel_to_json_code\excel_to_json_code\vcpkg.json: 
error: $.name (a package name): "excel_to_json_code" is not a valid package name. 
Package names must be lowercase alphanumeric+hyphens and not reserved (see https://learn.microsoft.com/vcpkg/users/manifests?WT.mc_id=vcpkg_inproduct_cli for more information).note: 
Extended documentation available at 'https://learn.microsoft.com/vcpkg/users/manifests?WT.mc_id=vcpkg_inproduct_cli'.
```

### 报错原因
vcpkg.json配置的包名错误
包名格式要求：
- 小写字母。
- 数字。
- 连字符 -。
- 不能包含特殊字符（如下划线 _、空格等）。
- 不能包含大写字母。

### 解决
修改包名：excel-to-json-code
```json
{
  "name": "excel-to-json-code",
  "version": "1.0.0",
  "dependencies": [
    "libxlsxwriter",
    "nlohmann-json"
  ]
}
```

## 下载第三方库时报错：找不到需要的文件或访问不到
### 报错内容：
<img width="1774" height="1030" alt="image" src="https://github.com/user-attachments/assets/c0a7e0cc-28c7-4067-8f01-cb7266859d77" />

### 报错原因
大概意思是可能由于代理服务器或网络原因找不到目标文件，所以下载失败

### 解决方法1. 尝试手动下载目标文件进行安装
文件网址和文件名
文件网址：截图第二行的网址 https://github.com/boostorg/cmake/commit/ae2e6a647187246d6009f80b56ba4c2c8f3a008c.patch?full_index=1
有时候缺失的是整个库，此时可能会是一个压缩包
文件名：截图第一个可以看到 Missing boostorg-cmake-boost-1.85.0-0009-msvc-1940-still-vc143.patch
这个就是需要的文件名

或者在 vcpkg/ports/库名/portfile.cmake 文件中寻找（参考本文-使用vcpkg-手动下载第三方库）

下载
这个网址是一个文件，打开后直接显示文件内容
我们可以复制文件内容和文件名，自行制作一个目标文件
注意：自行安装的文件可能会出现 校验错误
此时可以尝试修改上面查找文件地址的配置文件 vcpkg/ports/库名/portfile.cmake 文件中 地址下面的校验码的部分
注意，可能在C:/user/name/Appdata/vcpkg 下还有一个vcpkg文件夹，需要修改这里的配置文件才能生效
我手动下载文件出现了校验错误，并且无法修改，所以就有了第二种方法：修改镜像库代理

### 解决方法2. 修改镜像库代理
看截图第二行，是因为 https://mirror.ghproxy.com 代理  https://github.com/ 镜像导致的文件缺失
所以我们可以不使用这个代理，直接使用github进行下载
修改镜像库
修改下载脚本----- vcpkg_download_distfile.cmake (位于 vcpkg/scripts/cmake 文件夹下)
找到 vcpkg_download_distfile函数 ，在 vcpkg_list(SET urls_param) 后修改（大概在233行附近）
如果你是看我的教程，修改的镜像库，那么就可以看到下面截图倒数第二行有 github的镜像修改
<img width="1413" height="171" alt="image" src="https://github.com/user-attachments/assets/5666176c-f152-479c-a261-c3774c7a1ab3" />
我们将后面的 https://mirroe.ghproxy.com/删除
表示使用 github 转发 github，这样就可以取消镜像库映射
记得下载后将镜像映射恢复
（我后面有出现了同样的问题，又是 github库，所以我彻底将github库的映射取消掉了）

## code:124 libffi编译错误，提示与mysys2有关
### 报错原因：
1.Windows的 ALSR安全机制。
2.mysys2 未安装（安装时也会因为 ALSR机制 导致安装失败）
<img width="502" height="138" alt="image" src="https://github.com/user-attachments/assets/025b5c8c-4f91-40ee-9e8c-9a3ef54acb05" />

### 解决方法：
在 Windows安全中心 - 应用和浏览器控制 - 攻击防护设置 - 关闭 Force randomization for images  （ASLR）- 重启电脑
<img width="558" height="219" alt="image" src="https://github.com/user-attachments/assets/a7377679-0e36-42ff-b33b-139b8e5847e2" />

## 运行时报错：无法定位程序输入点 于动态库mysqlcppconn8-2-vs14.dll上
### 报错内容：
```bash
弹窗：无法定位程序输入点 于动态库mysqlcppconn8-2-vs14.dll上
```

### 报错原因：
第三方库出错，可能是：
1.第三方库的版本错误（运行代码是x86 debug版本，使用的dll库是relse版本）
2.dll缺失或错误

### 解决：
1.如果当前是debug模式，报错的dll应该也使用debug版本（位于vcpkg_install/x64-windows/debug/bin）
2.手动下载dll文件，或重装第三方库
使用管理员打开cmd，执行 sfc /SCANNOW 检查文件完整性

