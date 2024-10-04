
# SpringAI Elasticsearch Vector Store Demo

This project is a demo for setting up and using Elasticsearch as a vector store in a Spring Boot application. It demonstrates how to index and search vectors using cosine similarity with Elasticsearch.

## Features
- Vector search using Elasticsearch.
- Integration with Spring Boot for easy management.
- Support for cosine similarity.
- Demonstrates integration of vector storage into an AI system using Spring AI.

## Prerequisites
Before running the application, ensure you have the following installed:
- [Docker](https://www.docker.com/get-started)
- JDK 17 or higher
- Maven
- OpenSSL (for generating certificates)

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/LegPro/springai-elasticsearch-vector-demo.git
cd springai-elasticsearch-vector-demo
```

### 2. Running Elasticsearch in Docker
First, create a Docker network:
```bash
docker network create somenetwork
```

Then, start Elasticsearch container:
```bash
docker run -it --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:8.15.1
```

### 3. Import Elasticsearch Certificate

#### Step 1: Retrieve Elasticsearch Certificate
Run the following command to extract the certificate from Elasticsearch:
```bash
# In C:/temp
openssl s_client -showcerts -connect localhost:9200 </dev/null | sed -n -e '/-.BEGIN/,/-.END/ p' > certifs.cer
```

#### Step 2: Import Certificate into JDK
Navigate to your JDK's `bin` directory and import the certificate using `keytool`:
```bash
# In jdk/bin
keytool -import -alias elasticsearchcert -file "C:\temp\certifs.cer" -keystore "C:\Users\vhinu\.jdks\openjdk-23\lib\security\cacerts"
```
This step is necessary for establishing a secure connection between the Spring Boot application and Elasticsearch.

### 4. Run the Application
After setting up Elasticsearch, you can start the Spring Boot application using Maven:

```bash
mvn spring-boot:run
```

### 5. Application Properties Configuration

The important configuration for Elasticsearch is defined in the `application.properties` file:

```properties
spring.application.name=springai-elasticsearch-vector-demo

# OpenSearch Vector Store Settings
spring.ai.vectorstore.elasticsearch.initialize-schema=true
spring.ai.vectorstore.elasticsearch.index-name=spring-ai-document-index
spring.ai.vectorstore.elasticsearch.dimensions=1536
spring.ai.vectorstore.elasticsearch.similarity=cosine

# OpenSearch Connection Settings
spring.elasticsearch.connection-timeout=1s
spring.elasticsearch.uris=https://localhost:9200
spring.elasticsearch.restclient.sniffer.delay-after-failure=1m
spring.elasticsearch.restclient.sniffer.interval=5m
spring.elasticsearch.socket-keep-alive=true
spring.elasticsearch.socket-timeout=10000

# OpenSearch Credentials
spring.elasticsearch.password=your_password_here
spring.elasticsearch.username=elastic
```

Make sure to update the password and username for the Elasticsearch connection if necessary.

### 6. Testing the Application
Once everything is set up, you can interact with the vector store via the exposed APIs. Use Postman or curl to send requests for indexing and searching vectors.
