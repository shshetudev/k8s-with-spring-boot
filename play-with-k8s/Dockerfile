FROM alpine/java:21-jdk
EXPOSE 8080
ARG JAR_FILE=build/libs/play-with-k8s-0.0.1-SNAPSHOT.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT [ "java", "-jar", "/app.jar" ]