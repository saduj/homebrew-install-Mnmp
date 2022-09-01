# homebrew安装本地开发环境搭建
## 搭建List
### 系统方面
- [x] Homebrew
- [x] Node => v14.20.0
- [x] Npm => v6.14.17 (Node自带)
- [x] Python => 2.7.18
- [x] Git
- [x] Composer
- [x] Xcode Command Line Tools（命令行工具）

### 安装Nginx+Mysql+PHP+Memcached
- [x] Memcached
- [x] Nginx
- [x] Mysql=>5.7
- [x] PHP => 7.2
    - [x] Mongodb（没用到，是否删除）
    - [x] yaml-2.2.2
        - [x] libyaml
    - [x] yaf-3.3.5
    - [x] memcached-3.2.0
        - [x] zlib
        - [x] libmemcached
        - [x] pkg-config
    - [x] phalcon-3.4.5

## 添加到admin用户组权限
```
sudo chown -R whoami:admin /usr/local/
```

## 安装Homebrew
```
    # 安装Homebrew时需要xcode Command Line Tools，避免homebrew安装时间过长，可先行安装（ps：建议删除重新安装新版本）
    
    # 删除已安装
    rm -rf /Library/Developer/CommandLineTools
    
    # 已安装可忽略
    xcode-select --install
    
    #下载homebrew （ps：安装完之后别忘记切换国内的源）
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    # 找个国内源? 或者让自行解决？
```
-------

## 安装PHP环境&扩展
#### brew安装php@7.2
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
```
-------

#### 安装yaml-2.2.2
```
    # 安装libyaml依赖
    brew install libyaml
    
    // 默认的安装路径
    /usr/local/Cellar/libyaml/0.2.5
    
    // 如果找不到可以用以下命令查找路径（ps：推荐用这个，后续小版本升级不会影响到）
    brew --prefix --installed libyaml
    
    # pecl安装 yaml-2.2.2
    pecl install yaml-2.2.2   
```
-------

#### 安装yaf-3.3.5
```
    # yaf-3.3.5
    pecl install yaf-3.3.5 
   
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
```
-------

#### 安装memcached-3.2.0
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
    
    # 安装memcached-3.2.0
    pecl install memcached-3.2.0
    
```
-------

#### 安装phalcon-3.4.5
```
    # 打开terminal 一行一行的粘贴执行
    PHALCON_VERSION=3.4.5
    curl -sSL "https://codeload.github.com/phalcon/cphalcon/tar.gz/v${PHALCON_VERSION}" | tar -xz
    cd cphalcon-${PHALCON_VERSION}/build
    ./install
    cp ../tests/_ci/phalcon.ini $(php-config --configure-options | grep -o "with-config-file-scan-dir=\([^ ]*\)" | awk -F'=' '{print $2}')      
    cd ../../
    rm -r cphalcon-${PHALCON_VERSION}
    
    # 添加.so文件到php.ini
    extension=phalcon.so
```
#### 安装Mongodb
```
    # 安装
    pecl install mongodb
```
-------

## 安装Memcached
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

## 安装Nginx
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
## 安装Mysql
```
    # Mysql5.7
    brew install mysql@5.7
    
    # 启动
    brew services start mysql@5.7
    
    # 设置环境变量
    echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.zshrc
    
    echo 'export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"' >> ~/.zshrc
    echo 'export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"' >> ~/.zshrc
    
    # 连接
    mysql -u root -p
```
-------

## 安装Node v14.20.0
```
    # 安装
    brew install node@14
    
    # 设置环境变量
    echo 'export PATH="/usr/local/opt/node@14/bin:$PATH"' >> ~/.zshrc
    echo 'export LDFLAGS="-L/usr/local/opt/node@14/lib"' >> ~/.zshrc
    echo 'export CPPFLAGS="-phalconI/usr/local/opt/node@14/include"' >> ~/.zshrc
```
-------

## 安装Python-2.7.18
```
    # 安装 pyenv（ps：由于brew不支持安装旧版本的python，需要通过pyenv安装,或者去官网下载对应版本的pkg，也可）
    brew install pyenv
    
    # 安装python 2.7.18
    pyenv install 2.7.18
    
    # 配置环境变量
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
    echo 'eval "$(pyenv init -)"' >> ~/.zshrc
    
    # 全局管理python版本
    pyenv global 2.7.18
    
    #检查版本
    python --version
```
-------

## 安装composer
```
    # 安装
    brew install composer 
```
-------

## 安装git
```
    # 安装
    brew install git
```
## 快捷命令汇总
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
```



