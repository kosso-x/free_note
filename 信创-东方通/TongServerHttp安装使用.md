# TongServerHttp安装使用

- 解压文件
    
    ```bash
    # 压缩包
    /home/aas/东方通/东方通/TongHttpServer/TongHttpServer_6.0.0.0_x86_64.tar.gz
    
    # 解压
    tar -zxvf TongHttpServer_6.0.0.0_x86_64.tar.gz
    => TongHttpServer_6.0.0.0_x86_64
    
    # 复制到 /home/aas/ 
    mv /home/aas/东方通/东方通/TongHttpServer/TongHttpServer_6.0.0.0_x86_64 /home/aas/
    ```
    
- 启动文件
    
    ```bash
    # 进入目录
    cd /home/aas/TongHttpServer_6.0.0.0_x86_64/THS/bin
    
    # 目录结构
    -rwxr-xr-x 1 root root 5.1M 12月 21 13:04 httpserver
    -rwxr-xr-x 1 root root 216K 10月  9 2021 httpserverHA
    -rwxr-xr-x 1 root root  114 10月  9 2021 monitor.sh
    -rwxrwxr-x 1 root root 1.9K 6月   9 10:48 startConsole.sh
    -rwxr-xr-x 1 root root 1.9K 10月  9 2021 startHA.sh
    -rwxr-xr-x 1 root root 1.4K 10月  9 2021 start.sh
    -rw-rw-r-- 1 root root  39M 12月 23 14:08 thsconsole-6.0.0.0.jar
    
    # 启动
    ./start.sh 
    
    # 停止
    ./start.sh stop
    
    # 热加载
    ./start.sh reload
    ```
    
- 配置文件
    
    ```bash
    # 地址
    /home/aas/TongHttpServer_6.0.0.0_x86_64/THS/conf/httpserver.conf
    ```
    
- 代理 aas dev 的配置
    
    ```bash
    user  root;
    worker_processes  4;
    
    error_log  logs/error.log  error;
    pid        logs/httpserver.pid;
    
    events {
        worker_connections  1024;
        use epoll;
    }
    
    http {
        include       mime.types;
        default_type  application/octet-stream;
    
        upstream app {
          ip_hash;
          server unix:///opt/projects/account_audit_system/tmp/pids/aas.sock;
        }
    
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
    
        access_log  logs/access.log  main;
    
        status_zone;
    
        sendfile        on;
        keepalive_timeout  60;
    
        server {
            listen      80;
            server_name 192.168.100.201;
    
            access_log  logs/host.access.log  main;
    
            location / {
              proxy_pass http://localhost:3000;
            }
        }
    }
    ```