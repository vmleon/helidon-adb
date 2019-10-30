# Helidon + ATP Quickstart

This example implements a simple Hello World REST service connecting with Autonomous Database using Helidone MicroProfile.

To connect to ATP we are going to use Oracle JDBC driver and Universal Connection Pool (UCP).

## Requirements

Provision an ATP database and download the wallet.

Configure Maven to use [Oracle Maven Repository](https://maven.oracle.com/).

- The Oracle Maven repository requires user registration and accept the terms and conditions. Go to http://maven.oracle.com.
- Copy `settings.xml` into your `~/.m2`
- Copy `settings-security.xml` into your `~/.m2`
- In your terminal create a master password hash for your `settings-security.xml` with  `mvn -encrypt-master-password`. Use the hash to fill the property on the file.
- In your terminal create a server password hash for your `settings.xml` with  `mvn -encrypt-password`. Use the hash to fill the property on the file.

More information [here](https://blogs.oracle.com/dev2dev/get-oracle-jdbc-drivers-and-ucp-from-oracle-maven-repository-without-ides).

## Build and run

With JDK8+
```bash
mvn package
java -jar target/helidon-quickstart-mp.jar
```

## Exercise the application

```
curl -X GET http://localhost:8080/greet
{"message":"Hello World!"}

curl -X GET http://localhost:8080/greet/Joe
{"message":"Hello Joe!"}

curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting

curl -X GET http://localhost:8080/greet/Jose
{"message":"Hola Jose!"}
```

## Try health and metrics

```
curl -s -X GET http://localhost:8080/health
{"outcome":"UP",...
. . .

# Prometheus Format
curl -s -X GET http://localhost:8080/metrics
# TYPE base:gc_g1_young_generation_count gauge
. . .

# JSON Format
curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
{"base":...
. . .

```

## Build the Docker Image

```
docker build -t helidon-quickstart-mp .
```

## Start the application with Docker

```
docker run --rm -p 8080:8080 helidon-quickstart-mp:latest
```

Exercise the application as described above

## Deploy the application to Kubernetes

```
kubectl cluster-info                         # Verify which cluster
kubectl get pods                             # Verify connectivity to cluster
kubectl create -f app.yaml               # Deploy application
kubectl get service helidon-quickstart-mp  # Verify deployed service
```
