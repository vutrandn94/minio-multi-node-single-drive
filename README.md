# minio-multi-node-single-drive
Deploy MinIO: Multi-Node Single-Drive

## Requirement
- Format disk XFS for high performance
- Minimum: 4 nodes (4 servers) <=> Default server failures tolerance: 2 server failures in total (If reached 2 server failures, MinIO can switch to read-only mode or stop working to ensure data security)
- Deployment environment: Docker
- Deployment tools: Docker Compose

## Information about the servers deploying the lab

| Hostname | IP Address |
| :--- | :--- |
| minio01 | 172.31.40.231 |
| minio02 | 172.31.44.99 |
| minio03 | 172.31.36.91 |
| minio04 | 172.31.40.139 |

## Deploy
**Default minio admin user & password (Change if necessary)** 
| Default Root User | Default Root Password |
| :--- | :--- |
| root | Enjoyd@y |

**Default docker volume mount path**
| Default volume mount path |
| :--- |
| /mnt/data-0 | 

**Default MinIO docker image**
| Default MinIO docker image |
| :--- |
| quay.io/minio/minio:RELEASE.2025-01-20T14-49-07Z |

**Setting hosts file (Set on all nodes)**
```
## MINIO
172.31.40.231 minio01 
172.31.44.99 minio02 
172.31.36.91 minio03 
172.31.40.139 minio04 
```

**docker-compose.yml in minio1 (Config in node "minio1")**
```
services:
  minio01:
    image: 'quay.io/minio/minio:RELEASE.2025-01-20T14-49-07Z'
    restart: always
    environment:
      MINIO_ROOT_USER: "root"
      MINIO_ROOT_PASSWORD: "Enjoyd@y"
      TZ: "Asia/Ho_Chi_Minh"
    command: server --console-address ":9001" http://minio0{1...4}/mnt/data-0
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/mnt/data-0
    networks:
      - minio-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 1m
networks:
  minio-net:
    driver: bridge
```

**docker-compose.yml in minio2 (Config in node "minio2")**
```
services:
  minio02:
    image: 'quay.io/minio/minio:RELEASE.2025-01-20T14-49-07Z'
    restart: always
    environment:
      MINIO_ROOT_USER: "root"
      MINIO_ROOT_PASSWORD: "Enjoyd@y"
      TZ: "Asia/Ho_Chi_Minh"
    command: server --console-address ":9001" http://minio0{1...4}/mnt/data-0
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/mnt/data-0
    networks:
      - minio-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 1m
networks:
  minio-net:
    driver: bridge
```

**docker-compose.yml in minio3 (Config in node "minio3")**
```
services:
  minio03:
    image: 'quay.io/minio/minio:RELEASE.2025-01-20T14-49-07Z'
    restart: always
    environment:
      MINIO_ROOT_USER: "root"
      MINIO_ROOT_PASSWORD: "Enjoyd@y"
      TZ: "Asia/Ho_Chi_Minh"
    command: server --console-address ":9001" http://minio0{1...4}/mnt/data-0
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/mnt/data-0
    networks:
      - minio-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 1m
networks:
  minio-net:
    driver: bridge
```

**docker-compose.yml in minio4 (Config in node "minio4")**
```
services:
  minio04:
    image: 'quay.io/minio/minio:RELEASE.2025-01-20T14-49-07Z'
    restart: always
    environment:
      MINIO_ROOT_USER: "root"
      MINIO_ROOT_PASSWORD: "Enjoyd@y"
      TZ: "Asia/Ho_Chi_Minh"
    command: server --console-address ":9001" http://minio0{1...4}/mnt/data-0
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/mnt/data-0
    networks:
      - minio-net
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 1m
      timeout: 10s
      retries: 3
      start_period: 1m
networks:
  minio-net:
    driver: bridge
```

**Deploy service (Execute on all nodes)**
```
# docker-compose up -d
```

**Access MinioUI http://<MINIO_SERVER_IP>:9001 or config Nginx / HAProxy to loadbalance for MinIO nodes**
