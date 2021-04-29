# drone-sonar-plugin
The plugin of Drone CI to integrate with SonarQube (previously called Sonar), which is an open source code quality management platform.

### Notice
This branch adds a new environment parameter `JAVA_BINARIES` to fix the following problem:  
```
Please provide compiled classes of your project with sonar.java.binaries property
```

Find the details at "Pipeline example"

Detail tutorials: [DOCS.md](DOCS.md).

### Build process
build go binary file: 
`GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o drone-sonar`

build docker image
`docker build -t mailbyms/drone-sonar-plugin .`

### Testing the docker image:
```commandline
docker run --rm \
  -e DRONE_REPO=test \
  -e PLUGIN_SOURCES=. \
  -e SONAR_HOST=http://localhost:9000 \
  -e SONAR_TOKEN=60878847cea1a31d817f0deee3daa7868c431433 \
  -e JAVA_BINARIES=target/classes
  mailbyms/drone-sonar-plugin
```

### Pipeline example
```yaml
steps
- name: code-analysis
  image: mailbyms/drone-sonar-plugin
  settings:
      sonar_host:
        from_secret: sonar_host
      sonar_token:
        from_secret: sonar_token
      JAVA_BINARIES: target/classes
```
