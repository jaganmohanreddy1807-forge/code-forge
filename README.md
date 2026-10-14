# Code Forge

Code Forge is a React/Vite frontend and Spring Boot API for coding practice,
account management, and AI-assisted coding questions powered by Google Gemini.

## Requirements

- Node.js (v18+) and npm
- Java 17+ (or JDK 21/25)
- MySQL (optional; by default the backend uses an embedded persistent H2 database so it works out of the box)

---

## 1. Configure the Backend

The backend runs on **port 8090** by default, aligning with the frontend's API configuration.

### AI Configuration (Google Gemini)

Code Forge uses Google Gemini (`gemini-1.5-flash` by default) via Google's Generative Language API.

1. Get a Gemini API key from [Google AI Studio](https://aistudio.google.com/).
2. You can either set it in `backend/.env` (or root `.env`), or export it in your terminal:

```sh
# Option A: In backend/.env or root .env:
GEMINI_API_KEY=AIzaSy...

# Option B: In terminal:
export GEMINI_API_KEY="AIzaSy..."
```

*(Optional: If you prefer OpenAI, you can set `AI_PROVIDER=openai` and `OPENAI_API_KEY=sk-...`)*

### Database Configuration

- **Out of the box (zero setup)**: Code Forge automatically creates and persists data in a local file database at `./data/codeforge_db`.
- **MySQL (optional)**: If you prefer MySQL, create a database named `codeforge_db` and uncomment/export:

```sh
export DB_URL="jdbc:mysql://localhost:3306/codeforge_db"
export DB_USERNAME="your_mysql_username"
export DB_PASSWORD="your_mysql_password"
```

### Start the Backend

From the `backend/` directory:

```sh
./mvnw spring-boot:run
```

*(On Windows, run `.\mvnw.cmd spring-boot:run`)*

The backend will start and listen at `http://localhost:8090`.
You can verify the backend health at `http://localhost:8090/api/health`.

---

## 2. Configure and Run the Frontend

The frontend is configured with a Vite development server that communicates directly with the backend at `http://localhost:8090` and proxies `/api` requests.

### Configuration

A `.env` file in the project root sets:
```sh
VITE_API_BASE_URL=http://localhost:8090
```

### Start Vite

From the repository root:

```sh
npm install
npm run dev
```

Open the printed URL (e.g. `http://localhost:5173` or `http://localhost:5175`).

---

## 3. Verify the Integration

1. Start both the backend (`./mvnw spring-boot:run` in `backend/`) and frontend (`npm run dev` in root).
2. Open the frontend in your browser.
3. Click **Register** and create a new account.
4. Log in with your new credentials.
5. Navigate to **Code Forge AI** in the sidebar.
6. Ask a question (e.g., "What is binary search?"). The request will flow:
   `Frontend (React) -> Backend (Spring Boot :8090) -> Google Gemini API -> Backend -> Frontend`.
