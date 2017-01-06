#mac终端关机命令&开机启动

## 关机

1. 立即关机是

	    sudo halt
	    
	    sudo shutdown -h now


2. 10分钟后关机

    	sudo shutdown -h +10

3. 晚上8点关机

     	   sudo shutdown -h 20:00

4. 立即重启
    
	    sudo reboot 
	    
	    sudo shutdown -r now

## 开机启动

打开系统偏好设置-->用户与群组-->当前用户-登录项  在此添加或删除 开机登录时的自启动项。

比如添加 开机启动自动执行关机 shell脚本。

1. 新建AutoShutDown.sh 

    	#!/bin/sh 
		# echo + | （管道） 解决sudo 自动输入密码
    	echo "password" | sudo -S shutdown -h 19:00


2. 打开系统偏好设置-->用户与群组-->当前用户-登录项 添加AutoShutDown.sh
3. 添加AutoShutDown.sh默认用自己常用的terminal工具打开。
4. 重启就可以看到命令行自动运行啦