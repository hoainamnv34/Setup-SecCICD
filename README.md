# Setup Sec CI/CD tools

1. OWASP ZAP

- Run docker-compose
```yaml
version: "3"
services:
  zap:
    image: hoainamnv34/zap:0.0.4
    container_name: zap
    restart: unless-stopped
    ports:
      - "8083:8083"
```

Service Port: **8083**

2. SonarQube
- set **max_map_count**
```bash
sudo sysctl -w vm.max_map_count=262144
```

- Run docker-compose

```yaml
version: "3"
services:
  sonarqube:
    image: sonarqube:community
    hostname: sonarqube
    container_name: sonarqube
    depends_on:
      - postgresql
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://postgresql:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    ports:
      - "0.0.0.0:20041:9000"
    networks:
      - sonar-net
  postgresql:
    image: postgres:13
    hostname: postgresql
    container_name: postgresql
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
      POSTGRES_DB: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    networks:
      - sonar-net

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql:
  postgresql_data:
networks:
  sonar-net:
    name: sonar-net
```

Service Port: **20041**



3. Defect Dojo
- Clone the project
```bash
git clone git@github.com:hoainamnv34/Django-DefectDojo.git
```
- Building Docker images
```bash
cd django-DefectDojo 
./dc-build.sh 
```
- Run the application 
```bash
./dc-up-d.sh  
```

-  Obtain admin credentials. The initializer can take up to 3 minutes to run.
```bash
docker compose logs initializer | grep "Admin password:" 
```

Service Port: **8080**








