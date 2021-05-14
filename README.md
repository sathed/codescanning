# Code Scanning

Instructions for running a simple code scan using SonarQube.

## Instructions

1. Start the SonarQube docker container:
```
docker run --name sonarqube --rm \
  -p 9000:9000 \
  -v sonarqube_extensions:/opt/sonarqube/extensions \
  sonarqube:latest
```
2. Once the application is up, save the IP of your new container to the `SONARQUBE_URL`.
```
SONARQUBE_URL=$(docker inspect -f '{{ .NetworkSettings.Networks.bridge.IPAddress }}' sonarqube)
```
3. Log in to SonarQube at `http://localhost:9000`
    * Username: admin
    * Password: admin
4. Add your project:
    * Click `Add a project`
    * Select `Manually`
    * Provide the `Project key` and the `Display name`, then click `Set Up`.
    * Provide a name for your token and click `Generate`.
    * Click `Continue`
    * Select `Other` and choose `Linux`
5. Start the scan:
```
docker run --rm \
  -e SONAR_HOST_URL="http://${SONARQUBE_URL}:9000" \
  -e SONAR_LOGIN="<token_from_step_4>" \
  -v "/path/to/node/app:/usr/src" \
  sonarsource/sonar-scanner-cli \
  -Dsonar.projectKey=<project_key_from_step_4>
```
6. View the results in your SonarQube dashboard (http://localhost:9000)
