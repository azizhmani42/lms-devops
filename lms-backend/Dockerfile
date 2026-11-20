# ---------- ?tape 1 : build Maven ----------
FROM maven:3.8.6-eclipse-temurin-8 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn -B dependency:go-offline
COPY src ./src
RUN mvn -B clean package -DskipTests

# ---------- ?tape 2 : image d'ex?cution ----------
FROM eclipse-temurin:8-jre-alpine
WORKDIR /app
COPY --from=build /app/target/lms-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
