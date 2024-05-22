FROM openjdk:11
ARG JAR_FILE=target/*.jar
ENV SPRING_PROFILES_ACTIVE dev
COPY ${JAR_FILE} favourite-service.jar
ENTRYPOINT ["java","-jar","-Dspring.profiles.active=${SPRING_PROFILES_ACTIVE}","/favourite-service.jar"]
EXPOSE 8800