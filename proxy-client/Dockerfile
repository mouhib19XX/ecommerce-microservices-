
FROM openjdk:11
ARG JAR_FILE=target/*.jar
ENV SPRING_PROFILES_ACTIVE dev
COPY ${JAR_FILE} proxy-client.jar
ENTRYPOINT ["java","-jar","-Dspring.profiles.active=${SPRING_PROFILES_ACTIVE}","/proxy-client.jar"]
EXPOSE 8900