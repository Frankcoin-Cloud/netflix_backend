#=======stage 1: =======build stage===
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copy only pom.xml first to leverage Docker layer caching
COPY pom.xml .
RUN mvn dependency:go-offline -B


# Now copy source code and build
COPY src ./src
RUN mvn clean package -DskipTests


# ===== Stage 2: Runtime =====
FROM eclipse-temurin-17-jre-jammy

WORKDIR /app

# Copy only the built JAR from the build stage
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]