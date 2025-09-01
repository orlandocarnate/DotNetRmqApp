# DotNetRmqApp

A modular ASP.NET Core backend that consumes temperature readings from an ESP32-based outdoor sensor via RabbitMQ. Designed for reliability, scalability, and integration with MQTT-based IoT workflows.

## 📦 Project Overview

This microservice listens to a RabbitMQ queue (`sensor/temperature`) and processes temperature data published by an ESP32 device using the DS18B20 sensor. The ESP32 sends readings via MQTT, and RabbitMQ brokers the messages to this .NET consumer.

## 🧱 Architecture

```text
[ESP32 + DS18B20] → MQTT → RabbitMQ (Docker) → DotNetRmqApp (.NET Consumer)
```

- **ESP32**: Publishes temperature readings to `sensor/temperature` via MQTT
- **RabbitMQ**: Acts as a broker (MQTT plugin enabled)
- **DotNetRmqApp**: Background service consumes messages and logs or processes them

## 🚀 Getting Started

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- Docker (for RabbitMQ)
- ESP32 with DS18B20 sensor
- MQTT client library (`PubSubClient` for Arduino)

### Run RabbitMQ with MQTT Support

```bash
docker run -d --name rabbitmq \
  -p 5672:5672 -p 15672:15672 -p 1883:1883 \
  rabbitmq:3-management

docker exec rabbitmq rabbitmq-plugins enable rabbitmq_mqtt
```

### ESP32 MQTT Configuration

```cpp
client.setServer("YOUR_PC_IP", 1883);
client.publish("sensor/temperature", String(tempC).c_str());
```

### Run the .NET App

```bash
cd Api
dotnet run
```

Or use Docker:

```bash
docker build -t dotnetrmqapp -f Dockerfile .
docker run -p 8080:8080 dotnetrmqapp
```

## 🧪 Folder Structure

```plaintext
Api/
├── Consumers/              # RabbitMQ message listeners
├── Controllers/            # Optional HTTP endpoints
├── Models/                 # Domain models
├── Dtos/                   # Data transfer objects
├── Services/               # Business logic
├── Program.cs              # App entry point
├── appsettings.json        # Configuration
```

## ⚙️ Configuration

Edit `appsettings.json` to match your RabbitMQ setup:

```json
"RabbitMQ": {
  "Host": "localhost",
  "Port": 5672,
  "Username": "guest",
  "Password": "guest",
  "Queue": "sensor/temperature"
}
```

## 📈 Future Enhancements

- Store readings in a database (e.g., SQLite or Postgres)
- Add alerting for extreme temperatures
- Visualize data with Grafana
- Expand to multiple sensor types (humidity, pressure)

## 🧠 Author

Built by Orlando — full-stack developer and 3D visualization specialist, focused on resilient systems and authentic technical storytelling.

---

