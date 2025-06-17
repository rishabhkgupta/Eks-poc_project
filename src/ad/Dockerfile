# stage 1: Build
FROM eclipse-temurin:21-jdk AS builder

WORKDIR /usr/src/app/

COPY gradlew* settings.gradle build.gradle ./
COPY ./gradle ./gradle

RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

COPY . .
COPY ./pb ./proto
RUN chmod +x ./gradlew
RUN ./gradlew installDist -PprotoSourceDir=./proto

# Stage 2: Run
FROM eclipse-temurin:21-jre AS release

WORKDIR /usr/src/app/

COPY --from=builder /usr/src/app/build/install/opentelemetry-demo-ad/ ./app/

WORKDIR /usr/src/app/app

ENV AD_PORT=9099
EXPOSE 9099

ENTRYPOINT ["./bin/Ad"]
