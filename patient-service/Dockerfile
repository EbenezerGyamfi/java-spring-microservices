# build tools to user 
FROM maven:3.9.9-eclipse-temurin-21 AS builder


#working directory
WORKDIR /app

#dependency management
COPY pom.xml .


#go offline caches the dependencies unless a change has been detected
 RUN mvn dependency:go-offline -B

# copy the source code
COPY src ./src

#build the application for the docker image
RUN mvn clean package

# for runnig the code we a jdk

FROM openjdk:21-jdk AS runner 

WORKDIR /app



# copy the jar file from the build stage
# this is the jar file that was created in the build stage
COPY --from=builder ./app/target/patient-service-0.0.1-SNAPSHOT.jar ./app.jar

EXPOSE 4000

# run the application
ENTRYPOINT ["java", "-jar", "app.jar"]






# FROM maven:3.9.9-eclipse-temurin-21 AS builder

# WORKDIR /app

# COPY pom.xml .

# RUN mvn dependency:go-offline -B

# COPY src ./src

# RUN mvn clean package

# FROM openjdk:21-jdk AS runner

# WORKDIR /app

# COPY --from=builder ./app/target/patient-service-0.0.1-SNAPSHOT.jar ./app.jar

# EXPOSE 4000

# ENTRYPOINT ["java", "-jar", "app.jar"]
