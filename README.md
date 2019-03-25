# docker

#### docker-compose.yml

````
version: '3'

volumes:
  projeto-redis-data:
    driver: local

networks:
  backend:
    driver: bridge

services:
  apache-projeto:
    image: spohess/apache
    container_name: apache-projeto-cont
    volumes:
      - .:/var/www/html/
    ports:
      - "8001:8080"
    networks:
      - backend
    depends_on:
      - redis-projeto

  redis-projeto:
    image: spohess/redis
    container_name: redis-projeto-cont
    volumes:
      - projeto-redis-data:/data
    ports:
      - "6381:6379"
    networks:
      - backend

  queue-projeto:
    image: spohess/php
    container_name: queue-projeto-cont
    command: php artisan queue:work redis --tries=3
    volumes:
      - .:/var/www/html
    networks:
      - backend
    depends_on:
      - redis-projeto
````