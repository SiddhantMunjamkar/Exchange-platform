# ⚡ Real-Time Crypto Exchange System

A fully modular and event-driven cryptocurrency exchange system built using **Node.js**, **Redis**, **Docker**, and **WebSockets**. The architecture is designed for real-time matching, low latency, and scalable microservices.

> 💼 This project demonstrates practical skills in building distributed systems, real-time data processing, queue-based pipelines, and frontend-backend integration — suitable for production-grade crypto exchanges or high-frequency systems.

---

## 🧠 System Architecture

Below is a high-level view of the exchange system pipeline:

![Exchange Architecture](./docs/exchange_architecture.png)

### 🔁 Data Flow:

1. **Browser (Client UI)** sends an order using `POST /api/v1/order`.
2. **API Service**:
   - Validates and parses the order.
   - Pushes the order into a **Redis queue**.
   - Publishes the order event via **Redis Pub/Sub** to the **Engine**.
3. **Engine** (Matching Engine):
   - Consumes the order.
   - Executes matching logic.
   - Emits:
     - `trade_created` event with trade details.
     - Market updates like `ticker@SOL_USDC`, `depth@SOL_USDC`.
   - Publishes these updates to Redis Pub/Sub channels.
4. **WebSocket Service**:
   - Listens to Redis Pub/Sub updates.
   - Forwards real-time trade, ticker, and depth updates to all connected clients.
5. **Database Processor**:
   - Listens to trade-related events from Redis.
   - Persists price/time/volume data into a **Time Series Database** (TSDB), e.g., TimescaleDB or InfluxDB.

---

## 📁 Project Structure
├── api # REST API for placing orders (HTTP server)
├── db # Database processor to store trades in TSDB
├── docker # Docker and Docker Compose configs
├── engine # Matching engine that executes trades
├── frontend # Web frontend client (React or similar)
├── mm # Market Maker bot (optional, generates liquidity)
├── node_modules # Installed node dependencies
├── ws # WebSocket server for live data feed
├── .gitignore
├── README.md
├── yarn.lock