FROM registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.12

WORKDIR /usr/app
COPY ./passwordapi.jar .
EXPOSE 8080
CMD ["java", "-jar", "passwordapi.jar"]
