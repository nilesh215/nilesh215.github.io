---
layout: post
title: Spring boot applications run as service
---

[code]
readonly location=/home/user/project/application-name
readonly servicename=AppNameService
readonly port=8080
start() {
        echo "Starting" ${servicename}
    chdir ${location}    
    mvn clean
    mvn install
    nohup mvn spring-boot:run &
    #!/bin/bash
}

stop() {
    echo "Stopping" ${servicename}
    kill $(sudo lsof -t -i:${port})
}

restart() {
        stop
        start
}

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
esac
echo $"Usage: $0 {start|stop|restart}"

exit
[/code]
