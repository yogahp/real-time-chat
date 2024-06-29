# Real-Time Chat Application

This is a real-time chat application built with Vue.js for the frontend and Node.js for the backend. The application uses WebSockets for real-time communication and Docker for containerization.

## Features

- Real-time messaging
- User name and avatar selection
- Responsive design
- Dockerized for easy setup and deployment

## Project Structure

```plaintext
real-time-chat/
├── node_modules/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── ChatWindow.vue
│   │   └── EnterName.vue
│   ├── views/
│   ├── App.vue
│   ├── main.ts
│   ├── router/
│   │   └── index.ts
├── server.js
├── Dockerfile.frontend
├── Dockerfile.backend
├── docker-compose.yml
├── package.json
├── package-lock.json
├── tsconfig.json
├── vue.config.js
└── README.md
```

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Installation

1. **Clone the repository:**

    ```sh
    git clone https://github.com/yogahp/real-time-chat.git
    cd real-time-chat
    ```

2. **Build and start the Docker containers:**

    ```sh
    docker-compose up --build
    ```

3. **Access the application:**

    - Frontend: `http://localhost:8080`
    - Backend: `http://localhost:3000`

## Docker Setup

### Dockerfile for Vue.js Application (Frontend)

**Dockerfile.frontend**

```Dockerfile
# Dockerfile for Vue.js Application (Development)
# Use the official Node.js image as the base image
FROM node:16

# Set the working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose port 8080
EXPOSE 8080

# Start the development server
CMD ["npm", "run", "serve"]
```

### Dockerfile for Node.js Server (Backend)

**Dockerfile.backend**

```Dockerfile
# Dockerfile for Node.js Server (Development)
# Use the official Node.js image as the base image
FROM node:16

# Set the working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the server code
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the development server with nodemon for live reloading
CMD ["npx", "nodemon", "server.js"]
```

### Docker Compose Configuration

**docker-compose.yml**

```yaml
version: '3'

services:
  frontend:
    build:
      context: .
      dockerfile: Dockerfile.frontend
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "8080:8080"
    environment:
      - NODE_ENV=development
    command: npm run serve

  backend:
    build:
      context: .
      dockerfile: Dockerfile.backend
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    command: npx nodemon server.js
```

## Usage

1. **Open your browser and navigate to `http://localhost:8080` to access the Vue.js application.**
2. **The backend server should be running at `http://localhost:3000`.**

### Additional Commands

- **Stop Docker containers:**

    ```sh
    docker-compose down
    ```

- **Rebuild Docker containers:**

    ```sh
    docker-compose up --build
    ```

## License

This project is licensed under the MIT License.