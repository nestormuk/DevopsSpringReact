# 1. Use an official Maven image to build the project
FROM maven:3.8.6-eclipse-temurin-17 AS builder

# 2. Set working directory
WORKDIR /usr/src/app

# 3. Copy pom.xml first and download dependencies (for better caching)
COPY pom.xml .
RUN mvn dependency:go-offline

# 4. Copy the full source code and build the application
COPY . .
RUN mvn clean package -DskipTests

# 5. Use a lightweight JDK runtime image for running the app
FROM eclipse-temurin:17-jdk

# 6. Set the working directory in the final container
WORKDIR /app

# 7. Copy only the built JAR file from the builder stage
COPY --from=builder /usr/src/app/target/*.jar app.jar

# 8. Expose the application port (8080 is the default for Spring Boot)
EXPOSE 8080

# 9. Start the Spring Boot application
CMD ["java", "-jar", "app.jar"]
