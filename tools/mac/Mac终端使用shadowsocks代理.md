# Mac终端使用shadowsocks代理(privoxy)


> 浏览器翻墙的方法比较方便, 不过使用的终端默认是不会使用ss的代理的, 导致某些服务无法使用或者使用效果比较差, 需要额外的设置相关的服务才能来正常的使用


## Mac终端默认不使用shadowsocks原因

原因是在我们启动Shadowsocks的客户端的时候,会自动设置了几个类似 **http_proxy** 的系统级别的环境变量,浏览器在发送http请求的时候会去检测这个变量的值是否存在,如果存在的话会把请求的数据包发送到本地的代理服务器中转发到远端的代理服务器,最后再转发到真正的服务器中,流程如下.

![shadowsocks流程](https://update-image.oss-cn-shanghai.aliyuncs.com/pic/20200130154131.png)


## 安装与使用privoxy

1. privoxy安装
    使用brew安装
    ``` shell
    brew install privoxy
    ```
2. privoxy配置
    配置文件在/usr/local/etc/privoxy/config中
    加入以下配置
    ```shell
      listen-address 0.0.0.0:1087
      forward-socks5 / localhost:1080 .
    ```
    其中 第一行是设置privoxy 监听任意ip地址的1087端口,部分软件是8118端口,可以找到类似如下的软件设置查看配置情况
    ![监听](https://update-image.oss-cn-shanghai.aliyuncs.com/pic/20200130180403.png)
    
    第二行是本地代理客户端的设置.
    
 3. 启动privoxy

    ```shell
     sudo /usr/local/sbin/privoxy /usr/local/etc/privoxy/config
     ```
     
 4. 查看是否启动成功
     通过以下指令查看privoxy是否启动成功
     ```shell
     netstat -na | grep 1087
     #如果看到下面类似的输出则启动成功
     tcp4       0      0  127.0.0.1.1087         *.*                    LISTEN
     ```
 5. privoxy使用
     * 当前终端使用
         只需要在当前终端启动的情况下只需要执行以下内容即可
         
         ```shell
         export http_proxy='http://localhost:1087'
         export https_proxy='http://localhost:1087'
         ```
      * 代理持续生效
          上面的方法是只针对执行过相关指令的终端才有效,如果需要长期生效可以将上面的相关指令加入到 **~/.bash_profile** 中  
      * 设置开关方法
          在 **~/.bash_profile ** 文件中加入以下函数来快速开启和关系相关设置
          ```shell
          function proxy_off(){
                unset http_proxy
                unset https_proxy
                echo -e "已关闭代理"
            }

            function proxy_on() {
                export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
                export http_proxy="http://127.0.0.1:8118"
                export https_proxy=$http_proxy
                echo -e "已开启代理"
            }
          ```
      * 关闭
          当不需要使用的时候只需要执行以下指令即可关闭
          ```shell
          unset http_proxy
          unset https_proxy
          ```
      * 查看当前设置情况
          可以通过以下指令来查看设置情况,当关闭时是没有内容输出的
          ```shell
          echo $http_proxy
          #开启输出
          http://localhost:1087
          #关闭输出(没有任何内容)
          ```
          