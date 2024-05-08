#!/bin/bash

# 备份mysql数据库，并删除10天前备份的旧版本数据。

echo "====================开始备份=========================="

# 主机、用户、密码
HOST=127.0.0.1
USER=root
PWD=123456

# 数据库
DB_NAME=vczs

# 备份时间
DATETIME=$(date +%Y_%m_%d_%H_%M_%S)
echo "备份时间:$DATETIME"

# 备份路径
BACKUP_PATH=/home/backup/db
echo "备份路径:$BACKUP_PATH"

# 备份路径不存在就创建
if [ ! -d "$BACKUP_PATH/$DATETIME" ]; then
     mkdir -p "$BACKUP_PATH/$DATETIME"
fi

# 备份数据
mysqldump -h$HOST -u$USER -p$PWD $DB_NAME | gzip > "$BACKUP_PATH/$DATETIME/$DATETIME.sql.gz"

# 打包备份文件
cd $BACKUP_PATH
tar -zcvf "$DATETIME.tar.gz" "$DATETIME"
# 删除已打包的临时数据
rm -rf "$BACKUP_PATH/$DATETIME"

# 删除10天前备份文件
find $BACKUP_PATH -mtime +10 -name "*.tar.gz" -exec rm -rf {} \;

echo "======================备份完成==========================="
