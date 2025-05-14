## Running Ghost CMS in Docker.

#### 2.1 Install docker in EC2 instance.
**Note* Install docker-compose and create a file `docker-compose.yaml` and paste this code.

```bash
version: '3.8'

services:
  ghost:
    image: ghost
    environment:
      database__client: 
      database__connection__host: db
      database__connection__user: root
      database__connection__password: test@123
      NODE_ENV: development
      url: https://22393867f70b-10-244-4-241-3001.papa.r.killercoda.com:3001/      # add you IP
    ports:
      - 3001:2368
    networks:
      - ghost-network
    volumes:
      - ghost-data:/var/lib/ghost/content
    depends_on: 
      - db
  
  db:
    image: mysql:8.0
    ports:
      - 3306:3306
    networks:
      - ghost-network
    volumes:
      - mysql-vol:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: test@123


networks:
  ghost-network:
    driver: bridge

volumes:
  ghost-data:
  mysql-vol:
    
```

run mysql and ghost container using command `docker-compose up -d` 


🏡[Homepage](./README.md)
