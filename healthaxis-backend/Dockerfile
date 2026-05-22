# ---- Build Stage ----
FROM eclipse-temurin:21-jdk-alpine AS builder

WORKDIR /app

COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .
COPY src ./src

RUN chmod +x mvnw

RUN ./mvnw clean package -DskipTests

# ---- Runtime Stage ----
FROM eclipse-temurin:21-jre-alpine AS runtime

WORKDIR /app

RUN addgroup -S healthaxis && adduser -S healthaxis -G healthaxis

COPY --from=builder /app/target/*.jar app.jar

RUN chown healthaxis:healthaxis app.jar

USER healthaxis

EXPOSE 8080

ENV JAVA_OPTS="-Xms256m -Xmx512m -XX:+UseG1GC -XX:+UseContainerSupport"

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]