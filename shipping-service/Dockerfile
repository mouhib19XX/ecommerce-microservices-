FROM openjdk:11
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} shipping-service.jar
ENTRYPOINT ["java","-jar","/shipping-service.jar"]
EXPOSE 8600