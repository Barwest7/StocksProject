Hello!

This is a Stock Portfolio Management System.
It's a microservices based stock portfolio manager built with Docker, Flask, MongoDB, and NGINX. 
This was a project for Cloud Computing course at uni.

What does the project do?
Manages stock portfolios with real time prices, automatic failover, and load balancing. It is a backend system for for investment tracking.


Architecture:
The system runs 5 Docker containers:

MongoDB: stores portfolio data
Stocks1 (2 instances): manages first portfolio with load balancing
Stocks2: manages second portfolio
Capital Gains: calculates profits (or losses) across portfolios
NGINX: handles routing and load balancing

Services communicate with each other through Docker internal network.
NGINX distributes traffic 3:1 between the two Stocks1 instances for load balancing.

Tech Stack:
Python, Flask for the APIs
MongoDB for persistence
NGINX for reverse proxy and load balancing
Docker Compose for orchestration
API Ninjas for real time stock prices

How To Setup:

1. Clone the repo

git clone https://github.com/Barwest7/StocksProject.git
cd StocksProject

2. Get a free API key from API Ninjas

3. Create a .env file:

cp .env.example .env
Then add your API key to the .env file.

4. Run it:

docker-compose up --build
Services will be available on:

Stocks1: http://localhost:5001
Stocks2: http://localhost:5002
Capital Gains: http://localhost:5003
NGINX: http://localhost:80

Youre good to go!

Usage Examples:

Add a stock:
curl -X POST http://localhost:5001/stocks \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "AAPL",
    "name": "Apple Inc.",
    "purchase price": 150.00,
    "purchase date": "17-01-2024",
    "shares": 10
  }'

Get portfolio value:
curl http://localhost:5001/portfolio-value

Calculate capital gains:
Total gains:
curl http://localhost:5003/capital-gains

Filter by portfolio and share count:
curl "http://localhost:5003/capital-gains?portfolio=stocks1&numsharesgt=10"

API Endpoints:
Stocks Service (ports 5001, 5002)

GET /stocks - list all stocks
POST /stocks - add a stock
GET /stocks/{id} - get specific stock
PUT /stocks/{id} - update stock
DELETE /stocks/{id} - delete stock
GET /stock-value/{id} - get current value
GET /portfolio-value - total portfolio value

Capital Gains Service (port 5003)

GET /capital-gains - calculate gains

Query params: portfolio (stocks1/stocks2), numsharesgt, numshareslt


NGINX (port 80)

Limited access: only GET requests to /stocks1, /stocks2, and specific stocks
Load balances between Stocks1 instances (3:1 ratio)

Cool Features:
Auto restart on failure: containers automatically recover from crashes. Try it:
curl http://localhost:5001/kill

You should see in the docker that the service was shut down

If you then wait for a few seconds, and try:
curl http://localhost:5001/stocks

Docker brings it back up automatically!!!

Data persistence: MongoDB keeps your data even after containers restart!

Load balancing: NGINX distributes requests 3:1 between Stocks1 instances using weighted round robin.

Thank you for reading!!!
