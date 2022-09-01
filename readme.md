# HG本地开发环境搭建

## 目录
- [1. 搭建列表](#1搭建列表)
- [2. 添加到admin用户组权限](#2添加到admin用户组权限)
- [3. 安装Homebrew](#3安装Homebrew)
- [4. 安装php@7.2](#4安装php@7.2)
    - [4.1 安装yaml-2.2.2](#41-安装yaml-2.2.2)
    - [4.2 安装yaf-3.3.5](#42-安装yaf-3.3.5)
    - [4.3 安装memcached-3.2.0](#43-安装memcached-3.2.0)
    - [4.4 安装phalcon-3.4.5](#44-安装phalcon-3.4.5)
- [5. 安装Mongodb](#5安装Mongodb)
- [6. 安装Memcached](#6安装Memcached)
- [7. 安装Nginx](#7安装Nginx)
- [8. 安装Mysql](#8安装Mysql)
- [9. 安装Node-v14.20.0](#9安装Node-v14.20.0)
- [10. 安装Python-2.7.18](#10安装Python-2.7.18)
- [11. 安装composer](#11安装composer)
- [12. 安装git](#12安装git)
- [13. 快捷命令汇总](#13快捷命令汇总)

### 1. 搭建列表</span>
```
├── Homebrew                    
│   ├── PHP=>7.2
│       ├── Mongodb
│       ├── yaml-2.2.2
│           ├── libyaml
│       ├── yaf-3.3.5
│       ├── memcached-3.2.0
│           ├── zlib
│           ├── libmemcached
│           ├── pkg-config
│       ├── phalcon-3.4.5                  
├── Node => v14.20.0       
├── Npm => v6.14.17 (Node自带)
├── Python => 2.7.18  
├── Git                                
├── Composer                        
├── Xcode Command Line Tools（命令行工具）          
├── Memcached
├── Nginx                        
└── Mysql=>5.7                         
```

### 2.添加到admin用户组权限
```
sudo chown -R whoami:admin /usr/local/
```
-------

### 3.安装Homebrew
```
    # 安装Homebrew时需要xcode Command Line Tools，避免homebrew安装时间过长，可先行安装（ps：建议删除重新安装新版本）
    
    # 删除已安装
    rm -rf /Library/Developer/CommandLineTools
    
    # 已安装可忽略
    xcode-select --install
    
    #下载homebrew （ps：安装完之后别忘记切换国内的源）
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    
    # 查看brew版本
    brew --version
    
    # 查看brew源
    brew --config
    # 找个国内源? 或者让自行解决？
```
-------

### 4.安装php@7.2
```
    # 由于PHP 7.2官方已不再维护，需添加第三方仓库
    //将第三方仓库加入brew
    brew tap shivammathur/php
    
    //安装PHP
    brew install shivammathur/php/php@7.2  
    # 按照输出，需要配置环境变量

    # 重要！！！！-将php7.2和fpm配置环境变量（ps:如果不设置会用自带的php）
    echo 'export PATH="/usr/local/opt/php@7.2/bin:$PATH"' >> ~/.zshrc
    echo 'export PATH="/usr/local/opt/php@7.2/sbin:$PATH"' >> ~/.zshrc
    
    # 重要！！！！-将扩展的编译改为php7.2配置环境变量（ps：如果不设置会pecl命令会用不了&编译扩展也不会通过）
    echo 'export LDFLAGS="-L/usr/local/opt/php@7.2/lib"' >> ~/.zshrc
    echo 'export CPPFLAGS="-I/usr/local/opt/php@7.2/include"' >> ~/.zshrc
    
    # 重新加载资源
    source ~/.zshrc
    
    # 重启php-fpm
    brew services start|restart php@7.2
```

### 4.1安装yaml-2.2.2
```
    # 安装libyaml依赖
    brew install libyaml
    
    // 默认的安装路径
    /usr/local/Cellar/libyaml/0.2.5
    
    // 如果找不到可以用以下命令查找路径（ps：推荐用这个，后续小版本升级不会影响到）
    brew --prefix --installed libyaml
    
    # pecl安装 yaml-2.2.2,根据提示输入需要的依赖的路径
    pecl install yaml-2.2.2
    
    # 重启php-fpm
    brew services restart php@7.2
    
    # 查看扩展是否生效（或者 查看phpinfo()）
    php -m
```

### 4.2安装yaf-3.3.5
```
    # yaf-3.3.5
    pecl install yaf-3.3.5 
   
    # php.ini路径
    /usr/lcoal/etc/php/7.2/php.ini
   
    # 在php.ini中添加yaf相关配置
    [yaf]
    yaf.environ = development
    yaf.library = NULL
    yaf.cache_config = 0
    yaf.name_suffix = 1
    yaf.name_separator = ""
    yaf.forward_limit = 5
    yaf.use_namespace = 1
    yaf.use_spl_autoload = 1
    yaf.use_spl_autoload=1
    
    # 重启php-fpm
    brew services restart php@7.2
    
    # 查看扩展是否生效（或者 查看phpinfo()）
    php -m
    
```

### 4.3安装memcached-3.2.0
```
    # 安装libmemcached
    brew install libmemcached
    
    // 默认的安装路径
    /usr/local/Cellar/libmemcached/1.0.18_2
    
    // 如果找不到可以用以下命令查找路径（ps：推荐用这个，后续小版本升级不会影响到）
    brew --prefix --installed libmemcached
    
    # 安装zlib
    brew install zlib
    
    // 默认的安装路径
    /usr/local/Cellar/zlib/1.2.12
    
    // 如果找不到可以用以下命令查找路径（ps：推荐用这个，后续小版本升级不会影响到）
    brew --prefix --installed zlib
    
    # 安装pkg-config
    brew install pkg-config 
    
    # 安装memcached-3.2.0,根据提示输入需要的依赖的路径
    pecl install memcached-3.2.0
    
    # 重启php-fpm
    brew services restart php@7.2
    
    # 查看扩展是否生效（或者 查看phpinfo()）
    php -m
```

### 4.4安装phalcon-3.4.5
```
    # 打开terminal 一行一行的粘贴执行
    PHALCON_VERSION=3.4.5
    curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz
    cd cphalcon-${PHALCON_VERSION}/build
    ./install
    cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}')      
    cd ../../
    rm -r cphalcon-${PHALCON_VERSION}
    
    # 重启php-fpm
    brew services restart php@7.2
    
    # 查看扩展是否生效（或者 查看phpinfo()）
    php -m
    
```
-------

### 5.安装Mongodb（选装）
```
    # 安装
    pecl install mongodb
    
    # 重启php-fpm
    brew services restart php@7.2
    
    # 查看扩展是否生效（或者 查看phpinfo()）
    php -m
```
-------

### 6.安装Memcached
```
    # 安装
    brew install memcached
    
    # 启动
    brew services start memcached
    
    # 测试连接
    telnet 127.0.0.1 11211
    
    # 没有telnet
    brew install telnet
```
-------

### 7.安装Nginx
```
    # Nginx
    brew install nginx
    
    //nginx配置目录
    /usr/local/etc/nginx/nginx.conf
    
    # 启动
    brew services start nginx
    
    #检查nginx是否生效
    localhost:8080 
```
-------

### 8.安装Mysql
```
    # Mysql5.7
    brew install mysql@5.7
    
    # 启动
    brew services start mysql@5.7
    
    # 设置环境变量
    echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.zshrc
    
    echo 'export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"' >> ~/.zshrc
    echo 'export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"' >> ~/.zshrc
    
    # 重新加载资源
    source ~/.zshrc
    
    # 连接
    mysql -u root -p
```
-------

### 9.安装Node v14.20.0
```
    # 安装
    brew install node@14
    
    # 设置环境变量
    echo 'export PATH="/usr/local/opt/node@14/bin:$PATH"' >> ~/.zshrc
    echo 'export LDFLAGS="-L/usr/local/opt/node@14/lib"' >> ~/.zshrc
    echo 'export CPPFLAGS="-phalconI/usr/local/opt/node@14/include"' >> ~/.zshrc
    
    # 重新加载资源
    source ~/.zshrc
```
-------

### 10.安装Python-2.7.18
```
    # 安装 pyenv（ps：由于brew不支持安装旧版本的python，需要通过pyenv安装,或者去官网下载对应版本的pkg，也可）
    brew install pyenv
    
    # 安装python 2.7.18
    pyenv install 2.7.18
    
    # 配置环境变量
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(pyenv init -)"' >> ~/.zshrc
    
    # 重新加载资源
    source ~/.zshrc
    
    # 全局管理python版本
    pyenv global 2.7.18
    
    #检查版本
    python --version
```
-------

### 11.安装composer
```
    # 安装
    brew install composer 
    
    # 检查
    composer
```
-------

## 12.安装git
```
    # 安装
    brew install git
    
    # 检查
    git --version
```
-------

## 13.快捷命令汇总
```
    # 查询brew安装服务
    brew services list
    
    # 启动服务 （ps：不要使用sudo去启动服务，这样会导致用户是root，某些文件会没有权限写入，而报异常，解决方案：将当前用户添加入admin用户组）
    brew service start|stop|resart php@7.2|mysql@5.7|nginx|memcached
   
    # ngxin默认日志
    /usr/local/var/log/nginx/error.log
    
    #nginx站点配置文件
    /usr/local/etc/nginx/nginx.conf
    
    # php-fpm 默认日志
    /usr/local/var/log/php-fpm.log
    
    # php.ini路径
    /usr/lcoal/etc/php/7.2/php.ini
```