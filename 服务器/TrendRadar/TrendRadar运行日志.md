# 查看运行状态
docker exec -it trendradar python manage.py status

# 手动执行一次爬虫
docker exec -it trendradar python manage.py run
#手动执行一次并推送
sudo docker exec -it trendradar python -m trendradar
# 查看实时日志
docker exec -it trendradar python manage.py logs

# 显示当前配置
docker exec -it trendradar python manage.py config

# 显示输出文件
docker exec -it trendradar python manage.py files

# Web 服务器管理（用于浏览器访问生成的报告）
docker exec -it trendradar python manage.py start_webserver   # 启动 Web 服务器
docker exec -it trendradar python manage.py stop_webserver    # 停止 Web 服务器
docker exec -it trendradar python manage.py webserver_status  # 查看 Web 服务器状

# web地址
https://ecs-workbench-disposable.aliyun.com:8080/index.html

# 查看帮助信息
docker exec -it trendradar python manage.py help

# 重启容器
docker restart trendradar

# 停止容器
docker stop trendradar

# 删除容器（保留数据）
docker rm trendradar