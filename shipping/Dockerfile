# Build stage: use Maven with Eclipse Temurin JDK 11
FROM maven:3.9.10-eclipse-temurin-11 AS build-stage

WORKDIR /opt/shipping

# Copy only the pom.xml initially to cache dependencies
COPY pom.xml .

RUN mvn dependency:resolve

# Copy source code and package
COPY src ./src

RUN mvn package -DskipTests

# Runtime stage: use Eclipse Temurin JRE 11 for smaller image
FROM eclipse-temurin:11-jre

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy the built jar from build stage
COPY --from=build-stage /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
