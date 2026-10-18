# Gemini Chatbot

A simple AI chatbot built as a technical challenge.

The project contains a React frontend and a Node.js backend responsible for communicating with the Gemini API.

## Tech Stack

### Frontend

- React
- TypeScript
- Vite
- Tailwind CSS

### Backend

- Node.js
- Express
- TypeScript
- Gemini API
- Vitest
- Supertest

## Project Structure

```text
gemini-chatbot/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── Chat.tsx
│   │   ├── services/
│   │   │   └── chatService.ts
│   │   ├── types/
│   │   │   └── chat.ts
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── index.css
│   ├── package.json
│   ├── tsconfig.json
│   └── vite.config.ts
│
└── backend/
    ├── src/
    │   ├── config/
    │   │   └── env.ts
    │   ├── controllers/
    │   │   └── chatController.ts
    │   ├── routes/
    │   │   └── chatRoutes.ts
    │   ├── services/
    │   │   └── geminiService.ts
    │   ├── tests/
    │   │   ├── chatController.test.ts
    │   │   ├── geminiService.test.ts
    │   │   └── chatRoutes.test.ts
    │   ├── app.ts
    │   └── server.ts
    ├── .env.example
    ├── package.json
    └── tsconfig.json
```

## Requirements

Before running the project, make sure you have installed:

- Node.js
- npm
- A valid Gemini API key

You can create a Gemini API key through Google AI Studio.

## How to Run

### 1. Clone the repository

```bash
git clone <repository-url>
cd gemini-chatbot
```

### 2. Configure the backend

Open the backend folder:

```bash
cd backend
```

Install the dependencies:

```bash
npm install
```

Create a `.env` file based on `.env.example`.

```env
GEMINI_API_KEY=your_api_key_here
PORT=3001
```

The application validates the required environment variables when the backend starts.

Start the backend:

```bash
npm run dev
```

The API will be available at:

```text
http://localhost:3001
```

### 3. Configure the frontend

Open another terminal and access the frontend folder:

```bash
cd frontend
```

Install the dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The frontend will usually be available at:

```text
http://localhost:5173
```

### 4. Open the application

Open the following address in your browser:

```text
http://localhost:5173
```

Type a message in the chat and the frontend will send it to the backend.

The backend processes the request, communicates with the Gemini API and returns the generated response to the frontend.

## API

### Send a message

```http
POST /api/chat
```

Example request:

```json
{
  "message": "Hello"
}
```

Example response:

```json
{
  "reply": "Hello! How can I help you?"
}
```

If the message is empty or missing, the API returns:

```json
{
  "error": "Message is required"
}
```

with HTTP status:

```text
400 Bad Request
```

## Application Flow

The application follows this flow:

```text
React Frontend
      ↓
POST /api/chat
      ↓
Route
      ↓
Controller
      ↓
Gemini Service
      ↓
Gemini API
      ↓
Response
      ↓
React Frontend
```

## Backend Architecture

The backend was separated into small layers with clear responsibilities.

### Routes

Routes define the available HTTP endpoints.

```text
chatRoutes.ts
```

### Controllers

Controllers handle request validation and HTTP responses.

```text
chatController.ts
```

### Services

Services contain external integrations and business logic.

```text
geminiService.ts
```

The Gemini SDK is isolated inside the service so the rest of the application does not depend directly on the external API implementation.

### Config

Environment configuration is centralized inside:

```text
config/env.ts
```

The backend fails during startup if a required environment variable is missing instead of waiting until the first request.

## Tests

The backend uses Vitest for automated tests.

Run the tests with:

```bash
cd backend
npm test
```

The test suite covers:

- Request validation in the chat controller
- Gemini service behavior
- Chat API endpoint behavior

The Gemini API is mocked during automated tests.

This keeps the tests:

- Fast
- Deterministic
- Independent from internet access
- Independent from Gemini API availability
- Free from API quota usage

### Unit Tests

The unit tests validate individual parts of the application in isolation.

```text
chatController.test.ts
geminiService.test.ts
```

### Route Test

The route test uses Supertest to validate the HTTP endpoint.

```text
chatRoutes.test.ts
```

It verifies the flow between the Express route and the application logic while keeping the external Gemini API mocked.

## Build

### Frontend

```bash
cd frontend
npm run build
```

### Backend

```bash
cd backend
npm run build
```

The generated backend files will be available in:

```text
backend/dist
```

## Lint

Frontend linting can be executed with:

```bash
cd frontend
npm run lint
```

## Environment Variables

Example:

```env
GEMINI_API_KEY=
PORT=3001
```

The real `.env` file should never be committed to the repository.

Only `.env.example` should be versioned.

## Error Handling

The application handles common error scenarios such as:

- Empty messages
- Missing environment configuration
- Gemini API failures
- Failed frontend requests

If the Gemini API cannot generate a response, the backend returns an error and the frontend displays a user-friendly message.

## Git

The repository ignores files that should not be versioned, including:

```text
node_modules
.env
dist
coverage
```

## Notes

The project was intentionally kept simple because of the scope of the technical challenge.

The backend uses a Route → Controller → Service structure to separate responsibilities without introducing unnecessary complexity.

The frontend also keeps the state management local to the chat component because the application is small and does not require a global state management library.