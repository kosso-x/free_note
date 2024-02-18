# Rds安装使用

- 解压东方通文件夹下的
    1. TongRDS-2.2.1.1_P5.Node.tar.gz 
    
    ```bash
    tar -zxvf /home/aas/东方通/东方通/TongRDS_2.2.1.1_P5/安装文件/绿色版/TongRDS-2.2.1.1_P5.Node.tar.gz
    
    => pmemdb 
    ```
    
    2. TongRDS-2.2.1.1_P5.MC.tar.gz 
    
    ```bash
    tar -zxvf /home/aas/东方通/东方通/TongRDS_2.2.1.1_P5/安装文件/绿色版/TongRDS-2.2.1.1_P5.MC.tar.gz
    
    => pcenter
    ```
    
    3. 两个文件夹移动到目录 /home/aas/
- Rds的配置文件在 pmemdb/etc/cfg.xml
    
    ```bash
    Server.Listen.Secure: 是否启用密码 （2 是启用密码）
    Server.Listen.RedisPassword: 密码
    
    设置密码之后的REDIS_URL设置为：REDIS_URL=redis://:password@localhost:6379/0
    ```
    
- license 安装
    
           标准版license以名为“license.dat”的文件形式由厂家提供，程序的许可证文件“license.dat”放在pmemdb主目录下。
    
- 设置启动命令
    
    ```bash
    ln -s RdsNode.service /home/aas/pmemdb/bin/RdsNode.service
    ```