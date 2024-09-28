# Stock Trading
## Overview
This document provides a detailed overview of a stock trading system that allows clients to interact with external market exchanges, create and manage stock orders, and authenticate user activities. The system architecture is modeled using the C4 approach, which is broken down into Context, Container, Component, and Class diagrams.Each section provides insights into how different elements of the system interact and operate.

## Installation Instructions
1. Clone the repository from GitHub:
   ```bash
   git clone https://github.com/your-repo/stock-trading-system.git
   ```
2. Navigate to the project directory:
   ```bash
   cd stock-trading-system
   ```
3. Install the necessary dependencies using .NET CLI:
   ```bash
   dotnet restore
   ```
4. Run the application:
   ```bash
   dotnet run
   ```

## Architecture
### Context Diagram
![alt text](image.png)
The context diagram provides a high-level overview of the system and its primary external interactions.
#### System Overview:
The stock trading system enables clients to interact with market exchanges and manage stock trading operations, such as creating or updating orders. It relies on various services such as authentication, order management, and streaming to execute the trading functions.
#### Key Actors:
- Client: A user or an automated system that interacts with the stock trading platform to place or modify stock orders.
- Market Exchange: An external stock exchange where the client’s orders are executed.

#### Key Services:
- Authentication Service: Ensures that the client is authenticated before interacting with the system or placing orders.
- Order Service : Store Order In Database and send Order to Market Exchange Service.
- Streamer Service : Stream Order Details With Clients.
- Order Updeter : Update Order Status.
- Market Exchange Service: Facilitates interaction between the e-commerce system and external stock markets.

### Container Diagram
![alt text](image-1.png)
The container diagram focuses on the core system components, describing how the services interact within the system.
Core Containers:
1. Client: 
- The client is responsible for initiating requests to the stock trading system.These requests include actions such as placing stock orders, querying order statuses, or interacting with the external market.
2. Order Service: 
- The Order Service is a core part of the system, responsible for processing and managing stock orders.
    - Actions include:
        - Create/Update Orders: Clients can create new stock orders or update existing ones.
        - Interaction with SQL Database: The order service stores and retrieves order information from a SQL database.
        - Order Updater: This component is responsible for updating order statuses based on external market interactions or client actions.
3. SQL Database:
    - Stores and retrieves order-related data. All order information (e.g., timestamps, stock types, order status) is persisted in the SQL database for reliability and consistency.
<!-- 4. Redis (Cache):
        ◦ Redis is used to cache frequently accessed data, such as order statuses or recent transactions, allowing for faster responses to clients. -->
4. RabbitMQ (RMQ):
    - The system uses RMQ as a messaging queue to facilitate asynchronous communication between different services, such as the Order Updater and other system components.
6. Streaming Service:
    - Responsible for streaming real-time updates about the orders to the client. The client can subscribe to real-time order feeds, improving the trading experience.
7. Authentication Service:
    - Validates client credentials. Every interaction that requires a client to make an authenticated action (like placing an order) goes through the Authentication Service.
8. Market Exchange:
    - An external entity that the system interacts with to execute the client’s stock orders. The Market Exchange service fetches market data and transmits stock orders for execution.

### Component Diagram
![alt text](image-2.png)
The component diagram focuses on the Order Service, which is central to the system's operation.
Components of Order Service:
1. Order Processor:
    - The Order Processor handles the creation and updating of stock orders.
    - It is responsible for ensuring that all orders are processed efficiently and passed to the external market exchange.
2. Order Validator:
    - This component ensures that any order placed by the client adheres to the rules of the system and the market exchange.
    - Examples of validations include checking for sufficient funds, stock availability, and correct order formats.
3. Order Repository:
    - Interacts with the SQL database to persist order information.
    - Responsible for saving new orders and fetching existing order data when needed.
- **Flow**:
    - When a client places an order, the request passes through the Order Processor.
    - The Order Validator checks the order for compliance with system and market rules.
    - Once validated, the Order Repository stores the order in the SQL database, and the client is notified of the order's success or failure.

### Class Diagram
![alt text](class.jpg)
The class diagram outlines the structure of the Order Processor and related components in more technical detail.
Main Classes:
1. Order Processor:
    - Methods:
        - createOrder(): Initiates the creation of a new stock order.
        - updateOrder(): Updates the details of an existing order.
    - The Order Processor interacts with other classes, including the Order Validator and Order Repository, to ensure that each order is handled correctly.
2. Order Validator:
    - Methods:
        - validateOrder(): Verifies that the stock order is compliant with system rules, including limits, stock type, and user authentication.
        - Ensures that invalid orders are rejected before they are processed by the system.
3. Order Repository:
    - Methods:
        - saveOrder(): Persists the order details to the SQL database.
        - fetchOrder(): Retrieves order information from the SQL database.
    - Ensures data consistency and order traceability by interacting with the backend database. -->

#### System Interaction Summary
1. Client Interaction:
    - The client interacts with the system primarily through the Order Service.
    - Every order is validated, processed, and either streamed in real-time or stored for future access.
2. Order Processing:
    - Once an order is created, it follows a pipeline:
        - The Order Processor validates and stores the order.
        - If needed, the Order Updater will modify the order’s status based on market execution results or client modifications.
        - Updates are streamed to the client via the Streaming Service.
3. Market Interaction:
    - Once the order is validated, it is sent to the Market Exchange for execution.
    - The Order Updater monitors this process and updates the client on the order's success or failure.

#### Technologies
- **C#**
- **SQL Database**
    - **Definition**: A relational database used for persistent data storage.
    - **Usage**: It stores user details, order histories, stock information, and audit logs.
- **Entity FrameWork Core**
- **Dotnet 8**
- **ASP.NET Core**

- **SignalR**
    - **Definition**: RealTime Connection.
    - **Usage**: Stream Orders Ftom Streamer Service TO Client.
- **RabbitMQ**
    - **Definition**: RabbitMQ is the message broker facilitating communication between different services.
    - **Usage**: RMQ is used to manage queues, handle order processing requests, and communicate updates between the order service, market exchange, and order updater.
- **Redis**
    - **Definition**: Redis is an in-memory data store.
    - **Usage**: Redis is employed for caching frequently accessed data to improve response times. It stores temporary data like session tokens or recently accessed stock prices.

### Conclusion
This document presents a comprehensive overview of the stock trading system, highlighting its various services and components. The system is designed to efficiently handle stock orders, communicate with external market exchanges, and provide real-time updates to clients. The use of Redis for caching, RabbitMQ for messaging, and streaming services ensures that the system is both scalable and responsive.
