# Cloud-Native IoT Air Quality Monitoring System

End-to-end IoT platform for real-time air quality monitoring, integrating ESP32-based sensor nodes, a Docker-based local data stack, AWS cloud services, and a machine learning model for air quality classification.

## 1. Overview

This project collects temperature, humidity, and air quality data from an ESP32 node, processes it through a local development stack and a cloud-native architecture, and serves real-time analytics, alerts, and ML-powered predictions.

The system demonstrates:
- Full IoT pipeline: device → local stack → cloud → ML → dashboards → alerts  
- Hybrid development: Docker-based local environment plus AWS production deployment  
- Enterprise-grade security: MQTT over TLS 1.2 with X.509 certificates  
- Data engineering best practices: ETL, data quality, orchestration, and observability  

## 2. Features

- ESP32 node with DHT22 and MQ sensors, plus SSD1306 OLED display  
- Secure MQTT communication (TLS 1.2, X.509 mutual authentication)  
- Local stack with Docker Compose: Mosquitto, InfluxDB, MariaDB, Airflow, Node-RED, Grafana  
- AWS cloud architecture: IoT Core, Lambda, S3, DynamoDB, Glue, Athena, CloudWatch, API Gateway  
- XGBoost ML model (4-class air quality prediction) deployed to AWS Lambda  
- Automated ETL workflows and daily WhatsApp reports via Twilio  
- Real-time dashboards and historical analytics  

## 3. Architecture

### Local (Development) Architecture

```mermaid
graph LR
    subgraph "Docker Compose Environment"
        subgraph "Message Broker"
            MQTT[Mosquitto<br/>Port 1883]
        end
        
        subgraph "Time-Series DB"
            Influx[InfluxDB<br/>Port 8086]
        end
        
        subgraph "Relational DB"
            Maria[MariaDB<br/>Port 3306]
        end
        
        subgraph "Orchestration"
            AF1[Airflow Webserver<br/>Port 8080]
            AF2[Airflow Scheduler]
            DAG[WhatsApp DAG<br/>Daily Summary]
        end
        
        subgraph "Visual Programming"
            NR[Node-RED<br/>Port 1880]
        end
        
        subgraph "Visualization"
            Graf[Grafana<br/>Port 3000]
        end
    end
    
    subgraph "External Services"
        Twilio[Twilio API<br/>WhatsApp]
        Phone[WhatsApp on Phone]
    end
    
    ESP32[ESP32] -->|MQTT| MQTT
    MQTT --> NR
    NR --> Influx
    Influx --> AF2
    AF2 --> Maria
    AF2 --> DAG
    DAG --> Twilio
    Twilio --> Phone
    Influx --> Graf
    Maria --> Graf
    AF1 -.-> AF2
# spotifydataazureproject
