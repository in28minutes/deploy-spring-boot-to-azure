# Todo and Hello World Rest APIs Connecting to MySQL

- Image URL - https://hub.docker.com/r/in28min/todo-rest-api-mysql
- Image - in28min/todo-rest-api-mysql:1.0.0.RELEASE
- Runs on port 5000
- Run com.in28minutes.rest.webservices.restfulwebservices.RestfulWebServicesApplication as a Java Application.


## Containerization

### Troubleshooting

- Problem - Caused by: com.spotify.docker.client.shaded.javax.ws.rs.ProcessingException: java.io.IOException: No such file or directory
- Solution - Check if docker is up and running!
- Problem - Error creating the Docker image on MacOS - java.io.IOException: Cannot run program “docker-credential-osxkeychain”: error=2, No such file or directory
- Solution - https://medium.com/@dakshika/error-creating-the-docker-image-on-macos-wso2-enterprise-integrator-tooling-dfb5b537b44e

### Creating Containers and Running Locally

https://hub.docker.com/r/in28min/todo-rest-api-mysql

### Run MySQL  

```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 mysql:5.7
```

### Run REST API Container

```
mvn clean package
docker run --name todos-api --publish 5000:5000 --link mysql:mysql in28min/todo-rest-api-mysql:1.0.0.RELEASE
```

### Test

Run http://localhost:5000/users/in28minutes/todos


### Push Your REST API Container to Docker Repo

```
docker login

docker push in28min/todo-rest-api-mysql:0.0.1-SNAPSHOT
```


## Hello World URLS

- http://localhost:5000/hello-world

```txt
Hello World
```

- http://localhost:5000/hello-world-bean

```json
{"message":"Hello World - Changed"}
```

- http://localhost:5000/hello-world/path-variable/in28minutes

```json
{"message":"Hello World, in28minutes"}
```



## Todo JPA Resource URLs

- GET - http://localhost:5000/jpa/users/in28minutes/todos

```
[
  {
    "id": 10001,
    "username": "in28minutes",
    "description": "Learn JPA",
    "targetDate": "2019-06-27T06:30:30.696+0000",
    "done": false
  },
  {
    "id": 10002,
    "username": "in28minutes",
    "description": "Learn Data JPA",
    "targetDate": "2019-06-27T06:30:30.700+0000",
    "done": false
  },
  {
    "id": 10003,
    "username": "in28minutes",
    "description": "Learn Microservices",
    "targetDate": "2019-06-27T06:30:30.701+0000",
    "done": false
  }
]
```

#### Retrieve a specific todo

- GET - http://localhost:5000/jpa/users/in28minutes/todos/10001

```
{
  "id": 10001,
  "username": "in28minutes",
  "description": "Learn JPA",
  "targetDate": "2019-06-27T06:30:30.696+0000",
  "done": false
}
```

#### Creating a new todo

- POST to http://localhost:5000/jpa/users/in28minutes/todos with BODY of Request given below

```
{
  "username": "in28minutes",
  "description": "Learn to Drive a Car",
  "targetDate": "2030-11-09T10:49:23.566+0000",
  "done": false
}
```

#### Updating a new todo

- http://localhost:5000/jpa/users/in28minutes/todos/10001 with BODY of Request given below

```
{
  "id": 10001,
  "username": "in28minutes",
  "description": "Learn to Drive a Car",
  "targetDate": "2045-11-09T10:49:23.566+0000",
  "done": false
}
```

#### Delete todo

- DELETE to http://localhost:5000/jpa/users/in28minutes/todos/10001


## H2 Console

- http://localhost:5000/h2-console
- Use `jdbc:h2:mem:testdb` as JDBC URL 

## Azure Resource Commands

```
az group create --name docker-compose-resource-group --location westeurope
az appservice plan create --name docker-compose-service-plan --sku P1v2 --resource-group docker-compose-resource-group --is-linux 
az webapp create --resource-group docker-compose-resource-group --plan docker-compose-service-plan --name todo-rest-api-mysql-docker-compose --multicontainer-config-type compose --multicontainer-config-file docker-compose.yaml
```