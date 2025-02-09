# Spring Boot build stage
FROM maven:3.8.6-eclipse-temurin-17 AS builder

WORKDIR /usr/src/app

# Copy pom.xml and install dependencies (for better caching)
COPY pom.xml . 
RUN mvn dependency:resolve

# Copy the full source and build the app
COPY . .
RUN mvn clean package -DskipTests

# Final runtime image with JDK
FROM eclipse-temurin:17-jdk

WORKDIR /app

# Copy only the built JAR file from the builder
COPY --from=builder /usr/src/app/target/*.jar app.jar

EXPOSE 8080

# Run the Spring Boot application
CMD ["java", "-jar", "app.jar"]
