# Real-Time Bus Tracking System User Manual

## Table of Contents
1. Introduction
2. System Requirements
3. Installation
4. System Architecture
5. Using the Application
   5.1 Starting the Backend Services
   5.2 Launching the Frontend
6. Troubleshooting
7. Frequently Asked Questions (FAQ)
8. Contact Support

## 1. Introduction

Welcome to the Real-Time Bus Tracking System! This application allows you to monitor bus locations and speeds in real-time. The system consists of a backend that simulates bus movements and processes data, and a frontend that displays this information on an interactive map.

## 2. System Requirements

- Operating System: RedHat9 Linux Server (or compatible)
- Docker
- Python 3.8+
- Node.js 14+
- Apache Kafka 3.8.0
- Zookeeper 6.2.3
- Microsoft SQL Server 2022
- Web browser with WebSocket support

## 3. Installation

1. Clone the repository:
   ```
   git clone https://github.com/SWE-Capstone-GCPS/gcps-react.git
   cd gcps-react
   ```

2. Install Python dependencies:
   ```
   pip install confluent-kafka pyodbc websockets
   ```

3. Install JavaScript dependencies:
   ```
   npm install
   ```

4. Set up your SQL Server database and update the connection string in `main.py`.

5. Set up environment variables for database and WebSocket server configuration.

6. Ensure you have a MySQL database set up and running with the correct schema for Event_Instances and Asset_Telemetry tables.

7. Ensure Kafka and Zookeeper are running on your system.

## 4. System Architecture

The system consists of the following components:

- Producer (`producer.py`): Simulates bus movements and sends events to Kafka.
- Consumer (`consumer.py`): Receives events from Kafka and forwards them to the frontend via WebSocket.
- Processor (`processor.py`): Processes raw events into a format suitable for storage and display.
- Main Application (`main.py`): Coordinates the consumer, processor, and database operations.
- Database Manager (`database_manager.py`): Handles database connections and operations for storing and retrieving bus data.
- WebSocket Server (`websocket_server.py`): Manages real-time communication between the backend and frontend.
- Frontend (`App.jsx` and `App.css`): Displays real-time bus locations on an interactive map with a styled interface.

### Database Manager (`database_manager.py`)

This component manages the connection to the MySQL database and provides methods for inserting events and retrieving the latest vehicle positions. It uses environment variables for database configuration and includes error handling and logging.

Key features:
- Inserts events into the Event and Vehicle tables
- Retrieves the latest positions for all vehicles
- Uses connection pooling for efficient database operations

### WebSocket Server (`websocket_server.py`)

The WebSocket server facilitates real-time communication between the backend and frontend. It periodically fetches the latest vehicle positions from the database and sends them to connected clients.

Key features:
- Establishes a WebSocket server
- Retrieves latest vehicle positions from the database
- Sends vehicle updates to connected clients every second
- Uses environment variables for configuration

### Frontend Styling (`App.css`)

The `App.css` file provides styles for the frontend application, including:
- Layout for the sidebar and map container
- Styling for bus details in the sidebar
- Responsive design for various screen sizes
- Styling for the reset button and bus markers on the map

These components work together to create a robust, real-time bus tracking system with a user-friendly interface.

## 5. Using the Application

### 5.1 Starting the Backend Services

1. Start the Zookeeper:
   ```
   cd kafka
   bin/zookeeper-server-start.sh config/zookeeper.properties
   ```
3. Start Kafka:
    ```
   cd kafka
   bin/kafka-server-start.sh config/server.properties
   ```
5. Start the main application (consumer, processor, and database manager):
   ```
   python main.py
   ```

### 5.2 Launching the Frontend

1. Start the React development server:
   ```
   npm run dev
   ```
Open your web browser and navigate to `http://localhost:3000`.

- The map will display bus locations as markers.
- Bus positions and speeds will update in real-time.
- Use the mouse to pan and zoom the map.
- Click the "Reset" button to return to the initial view.

## 6. Troubleshooting

- If the map doesn't load, check your internet connection and ensure you have a valid Mapbox API key.
- If bus positions aren't updating, verify that the WebSocket connection is established (check browser console for messages).
- For database connection issues, double-check your MySQL connection string in `main.py`.

## 7. Frequently Asked Questions (FAQ)

Q: How often do bus positions update?
A: Bus positions update in real-time as events are received from the Kafka stream.

Q: Can I track multiple buses simultaneously?
A: Yes, the system is designed to handle multiple assets (buses) concurrently.

Q: Is historical data available?
A: The current version focuses on real-time data. Historical data features may be added in future updates.

## 8. Contact Support

For additional assistance, please contact our support team at support@realtimebustracker.com or visit our support portal at https://support.realtimebustracker.com.
