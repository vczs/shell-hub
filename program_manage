#!/bin/bash

# 可配置项
APP_NAME="main.go"
APP_PATH="/home/vczs/go/hello"
PID_PATH="$APP_PATH/log/hello.pid"
LOG_PATH="$APP_PATH/log/hello.log"

# 启动应用程序
start() {
    if [ ! -f "$APP_PATH/$APP_NAME" ]; then
        echo "Error: App file $APP_PATH/$APP_NAME does not exist"
        exit 1
    fi

    if [ -f "$PID_PATH" ]; then
        PID=$(cat "$PID_PATH")
        if ps -p "$PID" > /dev/null; then
            echo "Error: $APP_NAME is already running with PID $PID"
            exit 1
        else
            rm "$PID_PATH"
        fi
    fi

    echo "Starting $APP_NAME..."
    nohup "$APP_PATH/$APP_NAME" > "$LOG_PATH" 2>&1 &
    echo $! > "$PID_PATH"
    echo "$APP_NAME started with PID $(cat $PID_PATH)"
}

# 停止应用程序
stop() {
    if [ ! -f "$PID_PATH" ]; then
        echo "Error: $APP_NAME is not running"
        return
    fi

    PID=$(cat "$PID_PATH")
    if ps -p "$PID" > /dev/null; then
        echo "Stopping $APP_NAME..."
        kill -SIGTERM "$PID"
        rm "$PID_PATH"
        echo "$APP_NAME stopped"
    else
        echo "Error: $APP_NAME is not running"
        return
    fi
}

# 重启应用程序
restart() {
    stop
    start
}

# 查看应用程序状态
status() {
    if [ ! -f "$PID_PATH" ]; then
        echo "$APP_NAME is not running"
        exit 1
    fi

    PID=$(cat "$PID_PATH")
    if ps -p "$PID" > /dev/null; then
        echo "$APP_NAME is running with PID $PID"
    else
        echo "$APP_NAME is not running"
    fi
}

# 解析命令行参数并执行对应操作
case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    restart)
        restart
        ;;
    status)
        status
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
        ;;
esac

exit 0
