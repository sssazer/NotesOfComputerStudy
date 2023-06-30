# 1 基本介绍

Vi是Linux系统中内置的文本编辑器，相当于记事本

Vim是Vi的增强版，具有程序编辑的能力，提供代码补完、编译等功能

# 2. 三种模式

## 2.1 正常模式

通过命令行输入`vi Hello.java` 或 `vim Hello.java` 即可进入要编写的文件，默认是正常模式，只能查看无法编辑

## 2.2 编辑模式

**直接插入**

- i：在光标前面一个位置插入
- a：在光标后面一个位置插入
- I：从当前行 第一个非空字符的前一个位置插入
- A：在当前行尾插入

**插入新行并插入**

- o：在光标所在行的下面新插入一行并进入插入模式
- O：在光标所在行的上面新插入一行并进入插入模式

**删除并插入**

- s：删除光标所在字符并进入插入模式
- S：删除光标所在行，并在行首插入

## 2.3 命令行模式

在编辑模式写完之后，按esc回到正常模式，然后按 : 冒号进入命令行模式，之后输入命令执行保存等操作

wq：保存并退出
q：退出
q！：强制退出，不保存

# 3 vi和vim快捷键

在正常模式下输入才是快捷键

按完快捷键后左下角会有提示

### 3.1 拷贝 yy

可以先按v进入可视模式，然后移动光标选中多个字符，然后按y复制

yy：复制当前行。

先输入数字，再输入yy ，可以拷贝多行字符 

### 3.2 粘贴 p

p：将复制或删除的内容粘贴（插入）到光标所在的下一行中。

如果删除或复制的是不是整行而是字符，就会粘贴（插入）到光标的下一个位置

删除的内容也在剪切板中

### 3.3 删除 dd

- 输入dd删除当前行，输入数字+dd删除当前行开始的多行
- x 删除当前光标所在字符
- d0 删除从行首到光标（不包含）的字符
- d$ 删除从光标（包含）到行尾的字符

所有删除的内容事实上是存放在VIM的一个缓冲区当中，相当于剪切

### 3.4 查找 /

输入 / + 要查找的内容，从光标开始向后搜索

输入 ？ + 要查找的内容，从光标开始向前搜索

比如输入 /Hello 查找Hello

会查找到整个文件中的所有Hello并高亮显示，可以按n来切换到下一个查询结果，按N来切换到上一个查询的结果。输入 ：nohl 取消高亮

### 3.5 显示行号

输入 :set nu 显示行号

输入 :set nonu 取消显示行号

### 3.6 定位光标 g

- 输入 G 定位到末行。行号+G到指定行
- 输入 gg 定位到首行
- 输入 %  定位到当前光标所在括号的另一半括号

### 3.7 撤销 u

- u：撤销最后一次修改
- ctrl + r：恢复撤销的内容

### 3.8 移动光标

**基础移动：**

- h：向左
- l：向右
- j：向下
- k：向上

在移动命令前输入数字，可以移动指定数字的字符

**跳转移动：**

以单词为单位：

- w——word：跳到下一个单词开头

- b——begin：跳到本单词或上一个单词开头

​		光标在单词中间——跳到本单词开头

​		光标在单词开头——跳到上一个单词开头

- e——end：跳到本单词或下一个单词结尾
- ge：跳到上一个单词结尾

以行为单位：

- 0：跳到行首
- ^：跳到本行第一个非空字符
- &：跳到行尾
- gg：跳到第一行
- G：跳到最后一行

查找指定字符并跳转：

- f{char}——find char：光标跳到下个{char}所在位置
- F{char}：光标向前跳到上一个{char}所在位置
- t{char}：光标跳到下一个{char}的前一个字符
- T{char}：光标向前跳到上一个{char}的后一个字符
- `;`：重复上次的字符查找操作
- `,`：反向查找上次的查找命令

### 3.9 替换字符 :s

- 将光标移动至要替换的字符处，输入r，然后输入新的字符。只能替换单个字符

先输入数字，可以将光标后多个字符替换为同一个字符

- 按R可以进入替换模式

- `:s/old/new` 将光标所在行的第一个old替换为new
- `:s/old/new/g` 将光标所在行的所有old替换为new
- `:%s/old/new/g` 将文件中所有old替换为new

### 3.10 缩进 >>

- \>\>：缩进（相当于插入模式的TAB）
- <<：反缩进

### 3.11 执行shell命令（Linux命令行命令）

`:!要执行的命令`

比如：`:!ls /` 列出根目录下所有文件

### 3.12 另存为

- `:w 文件名` 整个文件另存为

- 先按v进入可视模式，选中要另存为的部分，再输入`:w 文件名`即可将选中部分另存为

- `:w! 文件名` 可以另存为并强制覆盖目标文件

### 3.13 合并文件

`:r 文件名` 会将指定文件读入并放在光标所在位置下一行

### 3.14 打开多个文件

`vim -o 文件1 文件2 ...` 将多个文件垂直（上下）并排打开

`vim -O 文件1 文件2 ...` 将多个文件水平（左右）并排打开

用 ctrl+w+w 将光标切换到下一个文件。或者ctrl+w+方向键 切换文件

在原来的退出命令后加一个a即可全部关闭，比如wqa，全部保存并关闭

### 3.15 动作+操作符

**操作符operator：**

- d：delete，删除
- c：change，修改
- y：yank，复制
- v：visual，选中并进入visual模式

**动作motion：**

- i：inner，内部
- a：around，周围

**范围：**

- w：word，光标所在单词
- `b` / `(`：bracket，光标所在小括号
- `B` / `{`：光标所在大括号
- `"`：光标所在双引号
- `'`：光标所在单引号
- `<`：光标所在尖角号

使用操作+动作+范围组合示例：

- `diw`：delete inner word，将光标所在单词删除
- `ciw`：change inner word，将光标所在单词删除并进入INSERT模式
- `yi(`：yank inner ()，复制小括号中的内容，不包括括号
- `ya{`：yank around {}，复制大括号中的内容，包括括号

三者可以随意组合

# 4. 修改配置和下载插件

## 4.1 配置

在`~/.vimrc`文件下添加配置信息

## 4.2 插件下载

插件管理插件：Vim-Plug

1. https://github.com/junegunn/vim-plug 下载plug.vim，并放在`~/.vim/autoload/`下

2. https://vimawesome.com/ 挑选插件

3. 在.vimrc配置文件中进行插件的安装

    ```
    ~/.vim/plugged是自己创建的用于存放安装的插件的目录
    call plug#begin('~/.vim/plugged')
    在这里写要安装的插件
    Plug 'preservim/nerdtree'
    call plug#end()
    ```

    在配置文件中写好要安装的插件后，在普通模式中输入`:PlugInstall`进行插件的安装

4. 插件的卸载：

    在配置文件中删除要卸载的插件 安装的时候写的语句
    
    在普通模式输入`:PlugClean`完成卸载

5. 插件的更新

    在普通模式输入`:PlugUpdate`完成更新

## 4.3 插件推荐

### 4.3.1 nerdtree

**功能：** 在文件左侧打开文件目录

**安装：** `Plug 'preservim/nerdtree'`

### 4.3.2 vim-startify

**功能：** 在命令行输入vim可以快速打开最近访问文件

**安装：** `Plug 'mhinz/vim-startify'`

### 4.3.3 vim-airline

**功能：** 打开文件后下方会显示一条信息栏

**安装：** `Plug 'vim-airline/vim-airline'`

### 4.3.4 coc.nvim

**功能：** 提供代码补全功能

**安装：**

1. 安装nodejs，需要版本14.4以上

    ```
    cd /opt
    wget https://nodejs.org/dist/v15.10.0/node-v15.10.0-linux-x64.tar.gz
    // 解压
    tar -zxvf node-v15.10.0-linux-x64.tar.gz
    // 创建软连接
    ln -s /opt/node-v15.10.0-linux-x64/bin/node /usr/bin/node
    ls -s /opt/node-v15.10.0-linux-x64/bin/npm /usr/bin/npm

    // 检查安装是否成功
    node -v
    npm -v
    ```

2. 安装coc.nvim

    安装前先确保git版本足够，不然下载时无法选择分支

    ```
    Plug 'neoclide/coc.nvim',{'branch':'release'}
    ```

3. 安装C++自动补全服务

    ```
    https://github.com/clangd/clangd/releases/download/15.0.6/clangd-linux-15.0.6.zip 下载文件并复制到/opt 下
    
    unzip clangd-linux-15.0.6.zip
    ln -s /opt/clangd_15.0.6/bin/clangd /usr/bin/langd
    
    打开vim编辑器，输入以下命令
    :CocInstall coc-clangd
    安装完成后就有自动补全了
    ```



# 5. NeoVIM下载

## 5.1 NeoVIM下载

[NeoVIM的github下载页面](https://github.com/neovim/neovim/releases)

下载.tar.gz文件之后解压，解压完成后即可进入 `~/nvim-linux64/bin`目录下运行nvim

但是会提示GLIBC2.28和GLIBC2.29 NOT FOUND

## 5.2 安装GLIBC2.28

GLIBC是C语言运行库，很多软件的运行依赖于GLIBC

1. 升级GCC编译器

    ```shell
    yum -y install cenos-release-scl
    yum -y install devtoolset-8-gcc devtoolset-8-gcc-c++ devtoolset-8-binutils
    scl enable devtoolset-8 bash
    echo "source /opt/rh/devtoolset-8/enable" >> /etc/profile
    # 在/etc/profile中添加一行
    ```

2. 升级make

    ```shell
    wget http://ftp.gnu.org/gnu/make/make-4.2/tar/gz
    tar -xzvf make-4.2/tar/gz
    cd make-4.2
    sudo ./configure
    sudo make 
    sudo make install
    sudo rm -rf /usr/bin/make
    sudo cp ./make /usr/bin
    make -v # 查看make版本，检查是否升级成功
    ```

3. 下载安装GLIBC2.28

    ```shell
    cd /usr/local
    wget https://mirror.bjtu.edu.cn/gnu/libc/glibc-2.28.tar.xz --no-check-certificate
    tar -xf glibc-2.28.tar.xz
    cd glibc-2.28
    mkdir build
    cd build
    yum install -y bison
    sudo ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
    make #运行时间较长
    make install
    ```

4. 验证GLIBC是否安装成功

    ```shell
    strings /lib64/libc.so.6 | grep GLIBC
    ```

5. 解决更新GLIBC后的中文乱码问题

    ```shell
    # 在刚刚的build目录下执行
    make localedata/install-locales
    ```

## 5.3 安装GLIBC2.29

1. 更新make

2. 安装python3

    ```shell
    whereis python # 查看系统中python的位置
    cd /usr/bin/
    ll python* # 查看当前的软链接
    # python -> python2
    # python2 ->python2.7
    安装完后可以让python指向python3，这样两个版本的python就可以共存了

    # 安装相关包
    yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make 
    # 安装pip
    yum -y install epel-release
    yum -y install libffi-devel
    yum install python-pip
    # 安装wget
    pip install wget
    #用wget下载python3的源码包
    wget http://npm.taobao.org/mirrors/python/3.7.5/Python-3.7.5.tar.xz
    xz -d Python-3.7.5.tar.xz
    tar -xf Python-3.7.5.tar
    cd Python-3.7.5
    ./configure prefix=/usr/local/python3
    make
    make install

    # 添加软连接
    # 备份原来的链接
    mv /usr/bin/python /usr/bin/python.bak 
    # 添加python3的软链接
    ln -s /usr/local/python3/bin/python3.7 /usr/bin/python 
    # 测试是否安装成功
    python -V

    #更改yum配置，否则会导致yum不能正常使用
    vi /usr/bin/yum
    # 把#! /usr/bin/python 改为 #! /usr/bin/python2
    vi /usr/libexec/urlgrabber-ext-down
    # 修改同上
    ```

3. 下载安装GLIBC2.29

    ```shell
    wget http://ftp.gnu.org/gnu/glibc/glibc-2.29.tar.gz
    tar -zxvf glibc-2.29.tar.gz
    cd glibc-2.29
    mkdir build
    cd build
    ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
    make # 执行时间较长
    make install
    strings /lib64/libc.so.6 | grep GLIBC
    ```

# 6. VSCode设置

## 6.1 要安装的插件：

## 6.2 设置字体：

### 6.2.1 下载字体

1. 在https://www.jetbrains.com/lp/mono/#intro

   中下载JetBrains Mono字体，是一个压缩包

2. 解压压缩包

3. Windows系统需要进入解压后的文件夹 -- fonts -- ttf 

   全选其中所有字体，右键选择安装

4. Linux系统需要将解压后的文件夹放入`~/.local/share/fonts`（本用户）或`/usr/share/fonts`（全系统）

   之后执行`fc-cache -f -v`

### 6.2.2 在VSCode中设置字体

1. 上方工具栏 -- 文件 -- 首选项 -- 设置

2. 在Editor：Font Family一栏中将字体名称添加至第一项

   例如：`'JetBrains Mono'`

3. 或者在字体下载的地方找到How To Install，根据要求操作