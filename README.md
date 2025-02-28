# gitbucket-docker-postgres

1. add folder **gitbucket_build**
2. add file **Dockerfile**
3. add script 
```text
FROM openjdk:17

MAINTAINER malexple

ADD https://github.com/gitbucket/gitbucket/releases/download/4.42.1/gitbucket.war /opt/gitbucket.war

RUN ln -s /gitbucket /root/.gitbucket

VOLUME /gitbucket

# Port for web page
EXPOSE 8080

# Port for SSH access to git repository (Optional)
EXPOSE 29418

CMD ["java", "-jar", "/opt/gitbucket.war"]
```

3. add folder **gitbucket_data** 
4. add file config gitbucket connect database **database.conf**
```text
db {
  url = "jdbc:postgresql://postgres:5432/postgres"
  user = "postgres"
  password = "postgres"
}
```
5. add folder **postgres_data**

6. add file docker-compose.yml

```yaml
version: '3'
services:
  gitbucket:
    build: gitbucket_build
    container_name: gitbucket
    volumes:
      - ./gitbucket_data:/gitbucket
    ports:
      - "8080:8080"
      
  postgres:
    image: postgres:15.5
    container_name: postgres
    volumes:
      - ./postgres_data/postgres.conf:/etc/postgresql.conf
    environment:
      POSTGRES_DB: "postgres"
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: "postgres"
    ports:
      - "5432:5432"  
```

7. run command

```bash
1. add folder **gitbucket_build**
2. add file **Dockerfile**
3. add script 
```
FROM openjdk:17

MAINTAINER malexple

ADD https://github.com/gitbucket/gitbucket/releases/download/4.42.1/gitbucket.war /opt/gitbucket.war

RUN ln -s /gitbucket /root/.gitbucket

VOLUME /gitbucket

# Port for web page
EXPOSE 8080

# Port for SSH access to git repository (Optional)
EXPOSE 29418

CMD ["java", "-jar", "/opt/gitbucket.war"]
```

3. add folder **gitbucket_data** 
4. add file config gitbucket connect database **database.conf**
```
db {
  url = "jdbc:postgresql://postgres:5432/postgres"
  user = "postgres"
  password = "postgres"
}
```
5. add folder **postgres_data**

6. add file docker-compose.yml

```yaml
version: '3'
services:
  gitbucket:
    build: gitbucket_build
    container_name: gitbucket
    volumes:
      - ./gitbucket_data:/gitbucket
    ports:
      - "8080:8080"
      
  postgres:
    image: postgres:15.5
    container_name: postgres
    volumes:
      - ./postgres_data/postgres.conf:/etc/postgresql.conf
    environment:
      POSTGRES_DB: "postgres"
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: "postgres"
    ports:
      - "5432:5432"  
```

7. run command

```bash
docker-compose up -d
```