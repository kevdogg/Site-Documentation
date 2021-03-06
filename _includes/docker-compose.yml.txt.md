```bash
---
version: '3.7'

#networks:
#  docker_net:
#    ipam:
#      driver: default
#      config:
#        - subnet: 172.28.1.0/24

networks:
  net:
    driver: bridge

#volumes:
#  authelia-data:
#    external: true


services:
  
  redis:
    container_name: redis
    restart: unless-stopped
#    image: redis:4.0-alpine
#    image: 'bitnami/redis:latest'
    image: ${REDIS_CONTAINER}
#    command: redis-server --requirepass authelia_redis_pw
#    ports: 
#      - 6379:6379
    expose:
      - ${REDIS_PORT}
    networks:
      - net
#      docker_net:
#        ipv4_address: 172.28.1.3
    environment:
      - TZ
      - REDIS_PASSWORD=${AUTHELIA_SESSION_REDIS_PASSWORD}
#      TZ: 'America/Chicago'
#      REDIS_PASSWORD: 'authelia_redis_pw'
 
  db:
    container_name: mariadb
#    image: linuxserver/mariadb
    image: ${MYSQL_CONTAINER}
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=${AUTHELIA_STORAGE_MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${AUTHELIA_STORAGE_MYSQL_DATABASE}
      - MYSQL_USER=${AUTHELIA_STORAGE_MYSQL_USER}
      - MYSQL_PASSWORD=${AUTHELIA_STORAGE_MYSQL_PASSWORD}
      - TZ
    networks:
      - net
#      docker_net:
#        ipv4_address: 172.28.1.2
#    ports:
#      - 3306:3306
    expose:
      - ${MYSQL_PORT}
    volumes:
#      - /var/data/db:/config
      - ${MYSQL_DATA_VOLUME}:/config

  authelia:
    container_name: authelia
    hostname: authelia
#    image: authelia/authelia:latest
    image: ${AUTHELIA_CONTAINER}
    restart: unless-stopped
    depends_on:
      - db
    networks:
      - net
#      docker_net:
#        ipv4_address: 172.28.1.4
    ports:
      - ${AUTHELIA_PORT}:${AUTHELIA_PORT}
#    expose:
#      - 8080
    volumes:
      - ${AUTHELIA_CONFIG_VOLUME}:/etc/authelia/configuration.yml
      - ${AUTHELIA_USERS_VOLUME}:/etc/authelia/users.yml
      - ${AUTHELIA_USER_DB_VOLUME}:/var/lib/authelia/db.sqlite3
#      - /etc/authelia/config/configuration.yml:/etc/authelia/configuration.yml
#      - authelia_config:/etc/authelia/configuration.yml
#      - /etc/authelia/config/users.yml:/etc/authelia/users.yml

#      - authelia-data:/var/lib/authelia
#      - /var/data/authelia/db.sqlite3:/var/lib/authelia/db.sqlite3
    environment:
      - TZ
      - NODE_TLS_REJECT_UNAUTHORIZED=0
      - AUTHELIA_JWT_SECRET
      - AUTHELIA_DUO_API_SECRET_KEY
      - AUTHELIA_SESSION_SECRET
      - AUTHELIA_NOTIFIER_SMTP_PASSWORD
      - AUTHELIA_SESSION_REDIS_PASSWORD
      - AUTHELIA_STORAGE_MYSQL_PASSWORD

```
