# nginx常用命令总结

## 查看nginx是否启动

1. ps -ef | grep nginx //用ps -ef列出进程列表，然后通过grep过滤。 
2. ps -C nginx -o pid //直接查看进程id

## nginx的启动，停止，与重载

1. nginx -s reload //修改配置文件后，可以重新热加载 
2. nginx -s stop //快速停止，可能会丢失有关信息 nginx -s quit //有序停止 
3. start nginx 或者 nginx开启nginx
