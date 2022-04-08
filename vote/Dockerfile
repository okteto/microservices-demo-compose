FROM openjdk:11

WORKDIR /app
COPY . ./ 
RUN sed -i 's/\r$//' ./mvnw && chmod 777 ./mvnw && ./mvnw clean package
RUN cp ./target/*.jar sanity.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/sanity.jar"]