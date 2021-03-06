### Overview 
Docker containers are created by using [base] images. An image can be basic, with nothing but the operating-system fundamentals, or it can consist of a sophisticated pre-built application stack ready for launch.

When building your images with docker, each action taken (i.e. a command executed such as apt-get install) forms a new layer on top of the previous one. These base images then can be used to create new containers.

In this wiki, we will see about automating this process as much as possible, as well as demonstrate the best practices and methods to make most of docker and containers via Dockerfiles: scripts to build containers, step-by-step, layer-by-layer, automatically from a source (base) image.

#### Docker in Brief 
The docker project offers higher-level tools which work together, built on top of some Linux kernel features. The goal is to help developers and system administrators port applications. - with all of their dependencies conjointly - and get them running across systems and machines headache free.

Docker achieves this by creating safe, LXC (i.e. Linux Containers) based environments for applications called “docker containers”. These containers are created using docker images, which can be built either by executing commands manually or automatically through Dockerfiles.

#### Dockerfiles
Each Dockerfile is a script, composed of various commands (instructions) and arguments listed successively to automatically perform actions on a base image in order to create (or form) a new one. They are used for organizing things and greatly help with deployments by simplifying the process start-to-finish.

Dockerfiles begin with defining an image FROM which the build process starts. Followed by various other methods, commands and arguments (or conditions), in return, provide a new image which is to be used for creating docker containers.

### Dockerfile Commands

Dockerfile consists of two kind of main line blocks: comments and commands + arguments.

#### FROM 
The FROM command initializes a new build stage and sets the Base Image for subsequent commands. Dockerfile must start with a FROM command.
````DockerFile
# Usage: FROM [image name]
FROM ubuntu
````

#### ADD 
ADD Commands Copy a file from the host into the container. 
````DockerFile
# Usage: ADD [source directory or URL] [destination directory]
ADD /my_app_folder /my_app_folder
````

#### CMD 
This command set default commands to be executed, or passed to the ENTRYPOINT 
````DockerFile
# Usage 1: CMD application "argument", "argument", ..
CMD "echo" "Hello docker!"
````

#### RUN
The RUN command will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.
````DockerFile
# Usage: RUN [command]
RUN mvn install
````
#### WORKDIR
The WORKDIR directive is used to set where the command defined with CMD is to be executed.
````DockerFile
# Usage: WORKDIR /path
WORKDIR ~/
````

#### MAINTAINER
The MAINTAINER set the author / owner data of the Dockerfile.
````DockerFile
# Usage: MAINTAINER [name]
MAINTAINER authors_name(admin@email.com)
````
#### EXPOSE
Expose a port to outside.
````DockerFile
# Usage: EXPOSE [port]
EXPOSE 8080
````

#### VOLUME
The VOLUME command is used to enable access from your container to a directory on the host machine (i.e. mounting it).
Example:
````DockerFile
# Usage: VOLUME ["/dir_1", "/dir_2" ..]
VOLUME ["/my_files"]
````

## Multi-stage builds
In Docker, one of the main issues is the size of the final image. It’s not uncommon to end up with images over 1 GB even for simple Java applications. Since version 17.05 of Docker, it’s possible to have multiple builds in a single Dockerfile, and to access the output of the previous build into the current one. Those are called 
[multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).
The final image will be based on the last build stage.

Let’s imagine the code is hosted on GitHub, and that it’s based on Maven. Build stages would be as follows:

*  Clone the code from GitHub.
* Copy the folder from the previous stage; build the app with Maven.
* Copy the JAR/WAR from the previous stage; run it with java -JAR/WAR .

### 1. Create the Dockerfile
This docker build file can be used for building any oasp4j web app with the following 
features:

*   The source code is hosted on GitHub.
*   The build tool is Maven.
*   The resulting output is an executable JAR/WAR file.

File: Dockerfile
```Dockerfile
# Stage 1. Git clone
FROM alpine/git AS clone
ARG url
WORKDIR /app
RUN git clone ${url}

# Stage 2. Maven build
FROM maven:3.5-jdk-8-alpine AS build
ARG project
WORKDIR /app
COPY --from=clone /app/${project} /app
RUN mvn install

# Stage 3. Run Spring Boot
FROM openjdk:8-jre-alpine
ARG artifactId
ARG version
ENV artifact ${artifactId}-server-bootified.war
WORKDIR /app
COPY --from=build /app/server/target/${artifact} /app

EXPOSE 8080

ENTRYPOINT ["sh", "-c"]
CMD ["java -jar ${artifact}"]
```
Note that `url`, `project`, `artifactId` and `version` are arguments that must be 
passed on the command line. 
`artifact` must be set as an environment variable with `ENV`, so it is persisted in the final
app image and can be used at runtime by `java`.

### 2. Build the image
The Spring Boot app image can now be built using the following command-line. 
Please change the parameters as per your project, e.g.:

```
docker build --build-arg url=https://github.com/username/java-getting-started.git --build-arg project=java-getting-started --build-arg artifactId=java-getting-started --build-arg version=1.0 -t java-getting-started .
```
### 3. Run a new container
Run the image built with the previous command:
```
docker run -d -p 8090:8080 java-getting-started
```

## Example

The next example shows how to create a Dockerfile to build and run a container 
running the server from [My Thai Star](https://github.com/oasp/my-thai-star) application.

Rather than using arguments like in the previous example, the data 
(git repo url, project name, ...) is set directly into the Dockerfile.

#### Sample Dockerfile
File Name: Dockerfile 
```Dockerfile
# 1. Clone the project code
FROM alpine/git AS clone
WORKDIR /app
RUN git clone https://github.com/Himanshu122798/mtsj.git

# 2. Copy the project folder from the previous build stage and build the app with maven
FROM maven:3.5-jdk-8-alpine AS build
WORKDIR /app
COPY --from=clone /app/mtsj /app
RUN mvn install

#3. Copy the war file from the previous build stage and run the app with java
FROM openjdk:8-jre-alpine
WORKDIR /app
COPY --from=build /app/server/target/mtsj-server-bootified.war /app

EXPOSE 8080

ENTRYPOINT ["sh", "-c"]  
CMD ["java -jar mtsj-server-bootified.war"]
```

#### Build the Docker image
Build the Docker image from the same folder as Dockerfile using this command (including the dot `.`)

```
docker build -t mtsj .
```

Where the option `-t mtsj` is used to tag the image name.

#### Run the container
Use this command to run the Spring Boot application.
```
docker run --name mtsj0 -p 8090:8080 mtsj
```
Where the options:
* `--name mtsj0` specifies the container name
* `-p 8090:8080` maps the port 8080 of the container to 8090

The command `docker ps` lists all the running containers:
```
λ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
fb0c6836838b        mtsj                "sh -c 'java -jar ..."   44 seconds ago      Up 43 seconds       0.0.0.0:8090->8080/tcp   mtsj0
```

The application is now running on http://localhost:8090/mythaistar/.