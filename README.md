# Docker images

```
version: '3'

volumes:
  redis-data:
    driver: local
  mysql-data:
    driver: local

networks:
  backend:
    driver: bridge

services:
  webserver:
    image: spohess/webserver:1.0.0
    container_name: webserver-cont
    volumes:
      - .:/var/www/html/
    ports:
      - "8080:8080"
    networks:
      - backend
    depends_on:
      - mysql

  mysql:
    image: spohess/mysql8.0.20:1.0.0
    container_name: mysql-cont
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"
    command: --max_allowed_packet=1073741824 --sql_mode="STRICT_TRANS_TABLES"
    environment:
      - MYSQL_DATABASE=laraveladmin
    networks:
      - backend

  redis:
    image: spohess/redis6.0.5:1.0.0
    container_name: redis-cont
    volumes:
      - redis-data:/data
    networks:
      - backend
```