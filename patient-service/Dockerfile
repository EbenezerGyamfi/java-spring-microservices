# Stage 1: Build the application
FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

# Copy Maven configuration and resolve dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy the rest of the source
COPY src ./src

# Build the project (skip tests to speed up CI builds, optional)
RUN mvn clean package

# Stage 2: Run the application
# Use a smaller JDK or even JRE-based image
FROM eclipse-temurin:21-jre-alpine AS runner

# Create app directory
WORKDIR /app

# Create non-root user
RUN adduser -D -u 1000 appuser

# Copy only the final JAR (not the entire /target)
COPY --from=builder /app/target/*.jar app.jar

# Make sure the JAR is readable
RUN chmod 644 app.jar && chown -R appuser /app

# Switch to non-root user
USER appuser

# Expose port
EXPOSE 4000

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
