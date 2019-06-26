---
title: "Dockerizing Aspnet Core App"
date: 2019-06-26T06:56:18-04:00
draft: true
---

Assuming that you have the correct docker environment, asp.net core app can run in a container using the following:

{{< highlight docker >}}
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
WORKDIR /app
COPY --from=build-env /app/out .
# RUN ["ls -alt"]
ENTRYPOINT ["dotnet", "DemoApp.dll"]%
{{< /highlight >}}

You might have noticed that the docker file in the previous example uses multi-stage builds in docker.  

```Docker
# Stage #1
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
...

# Stage #2
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
```

In case, Spring Boot is being used:

{{< highlight docker >}}
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG JAR_FILE
# COPY ./target/gs-spring-boot-docker-0.1.0.jar app.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
{{< /highlight >}}

The following java `pom.xml` file can be used:

{{< highlight xml >}}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot-docker</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
        <docker.image.prefix>springio</docker.image.prefix>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.4.9</version>
            <configuration>
                <repository>${docker.image.prefix}/${project.artifactId}</repository>
                <buildArgs>
                        <JAR_FILE>${project.build.finalName}.jar</JAR_FILE>
                </buildArgs>
            </configuration>
        </plugin>
        <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>unpack</id>
            <phase>package</phase>
            <goals>
                <goal>unpack</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>${project.groupId}</groupId>
                        <artifactId>${project.artifactId}</artifactId>
                        <version>${project.version}</version>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
        </plugins>
    </build>

</project>

{{< /highlight >}}

Golang can be easily containerized using the following:

{{< highlight docker >}}
FROM golang:latest                                                                                  
RUN mkdir /app                                                                                      
ADD . /app                                                                                          
WORKDIR /app                                                                                        
RUN go build -o main .                                                                              
CMD ["/app/main"] 
{{< /highlight >}}