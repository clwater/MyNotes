# confluence

Docker 安装


docker run --name mysql -e MYSQL_ROOT_PASSWORD=fqqJI1pVI6 -v /root/data/confluence/mysql-data:/var/lib/mysql  -p 3306:3306 -d mysql/mysql-server:5.7

docker run -d --name confluence -v /root/data/confluence/logs:/opt/atlassian/confluence/logs -v /root/data/confluence/confluence-data:/var/atlassian/confluence  -v /root/data/confluence/server.xml:/opt/atlassian/confluence/conf/server.xml -p 8090:8090  --link mysql:db --user root:root cptactionhank/atlassian-confluence:7.3.1

version: '3.4'
services:
  confluence:
    image: cptactionhank/atlassian-confluence:latest
    container_name: confluence
    ports:
      - "8090:8090"
      - "8091:8091"
    restart: always
    privileged: true
    depends_on:
      - db
    volumes:
      - /root/data/confluence/logs:/opt/atlassian/confluence/logs
      - /root/data/confluence/confluence-data:/var/atlassian/confluence
  db:
    image: mysql/mysql-server:5.7
    container_name: mysql-db
    ports:
      - "3306:3306"
    restart: always
     privileged: true
    environment:
      - MYSQL_ROOT_PASSWORD=fqqJI1pVI6
    volumes:
      - /root/data/confluence/mysql-data:/var/lib/mysql




