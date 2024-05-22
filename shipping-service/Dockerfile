FROM openjdk:11
ARG JAR_FILE=target/*.jar
ENV SPRING_PROFILES_ACTIVE dev
COPY ${JAR_FILE} shipping-service.jar
ENTRYPOINT ["java","-jar","-Dspring.profiles.active=${SPRING_PROFILES_ACTIVE}","/shipping-service.jar"]
EXPOSE 8600