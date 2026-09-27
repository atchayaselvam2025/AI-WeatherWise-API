AI-WeatherWise-API

AI WeatherWise API is a RESTful backend system developed using Node.js, Express.js, MongoDB, and Mongoose. The application provides secure user authentication through JWT and bcrypt password encryption, along with functionality for managing favorite locations. It also integrates Google Gemini Generative AI to provide intelligent weather information, summaries, and personalized recommendations using real-time weather data.

Features

- User Authentication: Provides secure user registration, login, and profile access using bcrypt for password hashing and JWT for authentication and authorization.
- Favorite Locations Management: Allows authenticated users to add, view, modify, and remove their preferred or frequently used locations.
- Current Weather Retrieval: Obtains important weather information such as temperature, humidity, wind speed, and current weather conditions through OpenWeatherMap.
- AI Weather Insights: Uses the Google Gemini API to create meaningful weather summaries and personalized recommendations, including suitable clothing, hydration, and outdoor activities.
- Resilient Fallback Mode: When OpenWeatherMap or Gemini API keys are not available, the system uses local rule-based mock generators. This allows the API to remain operational and testable without requiring external API credentials.

---

Technology Stack

- Runtime Environment: Node.js
- Web Framework: Express.js
- Database Wrapper: Mongoose ODM
- Database: MongoDB Atlas or Local MongoDB
- Security: JWT (jsonwebtoken), bcryptjs
- AI Integration: "@google/generative-ai" (Google Gemini API)

---

Project Structure (MVC)

src/
├── config/
│   └── db.js               # MongoDB connection setup
├── models/
│   ├── User.js             # User Schema (name, email, password)
│   └── Location.js         # Location Schema (city, country, user, createdAt)
├── middleware/
│   └── authMiddleware.js   # JWT Auth Guard middleware
├── controllers/
│   ├── authController.js   # Handlers for auth endpoints
│   ├── locationController.js # Handlers for favorites CRUD
│   ├── weatherController.js # Fetch weather data
│   └── aiController.js     # Orchestrate Gemini AI generations
├── routes/
│   ├── authRoutes.js
│   ├── locationRoutes.js
│   ├── weatherRoutes.js
│   └── aiRoutes.js
├── services/
│   ├── weatherService.js   # Fetch API query + mock fallback logic
│   └── aiService.js        # Gemini SDK client + local fallback logic
├── app.js                  # App middlewares and routes setup
└── server.js               # Entry point - starts server & DB

---

Quick Start Guide

1. Prerequisites

Before running the application, ensure that Node.js (v18+) and npm are installed on the system.

2. Install Dependencies

Open the project directory in a terminal and execute:

npm install

3. Environment Setup

Create a ".env" file in the root directory using ".env.example" as a reference:

PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/weatherwise
JWT_SECRET=your_super_secret_jwt_key_12345!
OPENWEATHER_API_KEY=your_openweathermap_api_key
GEMINI_API_KEY=your_gemini_api_key

«Note on API Keys: If the "OPENWEATHER_API_KEY" and "GEMINI_API_KEY" contain placeholder values or are not configured, the application automatically uses stable, locally generated mock response objects. This makes it possible to test and verify the available endpoints without immediately configuring external API credentials.»

4. Run the Server

For development mode with automatic file reloading, use:

npm run dev

For starting the application in production mode:

npm start

After starting successfully, the server will be available at:

http://localhost:5000

---

API Documentation

1. User Authentication

Endpoint| Method| Access| Request Body| Description
"/api/auth/register"| "POST"| Public| "{ "name", "email", "password" }"| Creates a new user account and returns a JWT token.
"/api/auth/login"| "POST"| Public| "{ "email", "password" }"| Authenticates the user and provides a JWT token.
"/api/auth/profile"| "GET"| Private (JWT)| None| Retrieves the profile information of the authenticated user.

2. Location Management (Private / JWT required)

Endpoint| Method| Access| Request Body| Description
"/api/locations"| "POST"| Private| "{ "city", "country" }"| Adds a new city to the user's favorite locations.
"/api/locations"| "GET"| Private| None| Displays all favorite locations saved by the authenticated user.
"/api/locations/:id"| "PUT"| Private| "{ "city", "country" }"| Modifies an existing favorite location using its ID.
"/api/locations/:id"| "DELETE"| Private| None| Removes a selected favorite location using its ID.

3. Weather Fetching

Endpoint| Method| Access| Description
"/api/weather/:city"| "GET"| Public| Retrieves current weather information including temperature, humidity, wind speed, and weather condition for a specified city.

4. AI Weather Insights (Private / JWT required)

Endpoint| Method| Access| Request Body| Description
"/api/ai/weather-summary"| "POST"| Private| "{ "city", "temperature", "humidity", "condition" }"| Produces an AI-generated natural language summary of the current weather.
"/api/ai/weather-recommendation"| "POST"| Private| "{ "temperature", "condition" }"| Provides personalized recommendations related to clothing and suitable activities.

---

API Testing Using Postman

The API endpoints can be tested conveniently using Postman by following these steps:

1. Import the "postman_collection.json" file available in the project root directory into Postman.
2. The collection contains predefined sample requests and payloads for user registration, login, profile access, location management, weather searches, and AI-based requests.
3. The registration and login requests include test scripts that automatically capture the generated JWT token and store it in the collection variable "token".
4. Once the token has been stored, the authenticated endpoints can be accessed without manually copying and entering the JWT token for every request.
