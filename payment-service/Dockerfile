FROM openjdk:11
ARG JAR_FILE=target/*.jar
ENV SPRING_PROFILES_ACTIVE dev
COPY ${JAR_FILE} payment-service.jar
ENTRYPOINT ["java","-jar","-Dspring.profiles.active=${SPRING_PROFILES_ACTIVE}","/payment-service.jar"]
EXPOSE 8400