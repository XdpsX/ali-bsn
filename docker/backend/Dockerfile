# Build stage
FROM maven:3.8.7-openjdk-18 AS build
WORKDIR /build
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM amazoncorretto:17
ARG PROFILE=dev
ARG APP_VERSION=1.0.2

WORKDIR /app
COPY --from=build /build/target/book-network-*.jar /app/

EXPOSE 8088

# postgres-sql-bsn is container name
ENV DB_URL=jdbc:postgresql://postgres-sql-bsn:5432/book_social_network
ENV ACTIVE_PROFILE=${PROFILE}
ENV JAR_VERSION=${APP_VERSION}

CMD java -jar -Dspring.profiles.active=${ACTIVE_PROFILE} -Dspring.datasource.url=${DB_URL}  book-network-${JAR_VERSION}.jar

#Run in book-network folder
# docker build -t bsn/bsn:1.0.0 -f ../docker/backend/Dockerfile .
# docker build -t bsn/bsn:1.0.1 -f ../docker/backend/Dockerfile .