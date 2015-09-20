---
layout: post
title: Spring boot applications run as service
---

Deploying spring boot applications can be made as easy as starting a service in linux. Follow the steps below and you can simply execute service appname start/stop/restart in your command prompt.

> 1. Create a file by the name of the application in folder /etc/init.d/ 
  eg: login, email, authentication etc.(extension not required)
  

> 2. Copy paste this code below and make changes for location, service name and port on which your application runs


> 3. Make the file executable by using command chmod +x appname 


> 4. Now simply execute service appname start/stop/restart


Additionally we can easily add more output to verify the process executed. 
eg: echo "Service Restarted Successfully"

```
# Location of the project
readonly location=/home/user/project/application-name
# Name of the project
readonly servicename=AppNameService
# Port on which the application runs
readonly port=8080

# Steps to start the application
start() {
        # Print the starting of application
        echo "Starting" ${servicename}
        # Navigate the location of the project
        chdir ${location}    
        #Perform processes to start the application
        mvn clean
        mvn install
        # Here the & depicts to start the process as independent process and append the logs to file name nohup at 
        # project location
        nohup mvn spring-boot:run &
}

# Steps to stop the application
stop() {
    echo "Stopping" ${servicename}
    kill $(sudo lsof -t -i:${port})
}

# Steps to restart the application
restart() {
        stop
        start
}

# Check the first string from input line and execute the appropriate method
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
```

Simple usage of shell script can help a lot in automating the repetative process for development, devops, changing properties from staging to development. 
