# Kafka配置

## app.env

```
zookeeper_ip=公网ip
```

例如 zookeeper_ip=11.11.11.112


## docker-compose-env.yml

```
#运行环境的启动方法 zookeeper
version: '2'
services:
  zookeeper:
    image: zookeeper:3.5.9
    container_name: zookeeper_container
    ports:
      - "2181:2181"
    networks:
      - app_net
  
  kafka01:
    image: wurstmeister/kafka:2.13-2.6.0
    container_name: kafka_container_01
    ports:
      - "9092:9092"
    environment:
      - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_ADVERTISED_HOST_NAME=${zookeeper_ip}
      - KAFKA_ADVERTISED_PORT=9092
    networks:
      - app_net
    depends_on:
      - zookeeper
    
  
  kafka02:
    image: wurstmeister/kafka:2.13-2.6.0
    container_name: kafka_container_02
    ports:
      - "9093:9092"
    environment:
      - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_ADVERTISED_HOST_NAME=${zookeeper_ip}
      - KAFKA_ADVERTISED_PORT=9092
    networks:
      - app_net
    depends_on:
      - zookeeper

  kafka03:
    image: wurstmeister/kafka:2.13-2.6.0
    container_name: kafka_container_03
    ports:
      - "9094:9092"
    environment:
      - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
      - KAFKA_ADVERTISED_HOST_NAME=${zookeeper_ip}
      - KAFKA_ADVERTISED_PORT=9092
    networks:
      - app_net
    depends_on:
      - zookeeper
networks:
  app_net:
    driver: bridge
```



## 启动命令

```
docker-compose --env-file app.env -f docker-compose-env.yml up -d 
docker-compose -f docker-compose-env.yml stop
docker-compose -f docker-compose-env.yml down
```



## 检查命令

```
docker-compose -f docker-compose-env.yml logs zookeeper
docker-compose -f docker-compose-env.yml logs kafka01
docker-compose -f docker-compose-env.yml logs kafka02
docker-compose -f docker-compose-env.yml logs kafka03
```

参考

https://docs.docker.com/compose/compose-file/compose-file-v2

