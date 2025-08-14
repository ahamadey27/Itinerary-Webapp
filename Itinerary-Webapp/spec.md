# Wander-Portfolio: A Full-Stack Travel Itinerary Application Specification

## Project Vision & Technology Stack
This document outlines the complete specification for building "Wander-Portfolio," a full-stack web application that serves as a cornerstone portfolio project. The application emulates the core functionality of a travel planner, allowing users to create trips, add daily points of interest, and view them on an interactive map.

The technology stack is a modern, MERN-based architecture chosen for its relevance in today's job market, ensuring the skills developed are directly transferable to professional roles.

### Technology Stack Summary

| Category         | Technology                  | Version (LTS/Latest) | Role in Project                               |
| ---------------- | --------------------------- | -------------------- | --------------------------------------------- |
| **Runtime** | Node.js                     | 22.x LTS             | Executes our backend JavaScript code.         |
| **Backend** | Express.js                  | 4.x                  | Framework for building our backend API.       |
| **Frontend** | React                       | 18.x                 | Library for building the user interface.      |
| **Build Tool** | Vite                        | Latest               | Develops and bundles the React application.   |
| **API** | Google Maps JavaScript API  | v3                   | Provides map and location services.           |
| **Frontend Lib** | @vis.gl/react-google-maps   | Latest               | React components for Google Maps.             |
| **Frontend Lib** | react-datepicker            | Latest               | Component for selecting trip dates.           |
| **Testing (BE)** | Jest & Supertest            | Latest               | For testing our Express API endpoints.        |
| **Testing (FE)** | Jest & React Testing Library| Latest               | For testing our React components.             |
| **Deployment (FE)**| Vercel                      | N/A                  | Platform for hosting our live React app.      |
| **Deployment (BE)**| Render                      | N/A                  | Platform for hosting our live Express API.    |

---

## Phase 1: Foundational Setup & Configuration
**Goal:** Establish a robust and well-organized project foundation by setting up the development environment, scaffolding the initial project structures, and securely configuring all necessary services.

### Step 1: Development Environment Setup
- [x] **Install Node.js and npm:** Download and install the latest LTS version of Node.js from the official website. Verify installation by running `node -v` and `npm -v` in your terminal.
- [x] **Configure VS Code:** Install Visual Studio Code and add the following recommended extensions for improved productivity and code quality:
    - [x] ESLint
    - [x] Prettier - Code formatter

### Step 2: Frontend Project Scaffolding with Vite
- [x] **Create the React Project:** Use the Vite CLI to scaffold the frontend application.
    ```bash
    npm create vite@latest
    ```
    - Follow the prompts, naming the project `wander-portfolio-client` and selecting the `React` framework with the `JavaScript` variant.
- [x] **Install Dependencies and Start Dev Server:** Navigate into the new project directory, install the required packages, and start the development server.
    ```bash
    cd wander-portfolio-client
    npm install
    npm run dev
    ```
- [x] **Review Project Structure:** Familiarize yourself with the generated file structure, particularly `package.json`, `vite.config.js`, and the `src/` directory containing `main.jsx` and `App.jsx`.

### Step 3: Backend Project Scaffolding with Node.js & Express
- [x] **Create the Server Directory:** In the project root (parallel to `wander-portfolio-client`), create the backend directory.
    ```bash
    mkdir wander-portfolio-server
    cd wander-portfolio-server
    ```
- [x] **Initialize the Node.js Project:** Create a `package.json` file using npm.
    ```bash
    npm init -y
    ```
- [x] **Install Express:** Add Express.js as a project dependency.
    ```bash
    npm install express
    ```
- [x] **Create a "Hello World" Server:** Create an `app.js` file, add the basic server code to listen on port `3001`, and verify it runs correctly by executing `node app.js`.

### Step 4: Acquire and Secure API Credentials
- [ ] **Create a Google Cloud Platform (GCP) Project:** Navigate to the Google Cloud Console, create a new project, and enable billing.
- [ ] **Enable Required APIs:** In the API Library, search for and enable the following three APIs:
    - [ ] Maps JavaScript API
    - [ ] Places API
    - [ ] Geocoding API
- [ ] **Create and Restrict the API Key:** In "APIs & Services" > "Credentials", create a new API key. **CRITICALLY**, apply restrictions immediately:
    - [ ] **Application restrictions:** Select "HTTP referrers" and add `localhost:*/*` for development.
    - [ ] **API restrictions:** Select "Restrict key" and choose the three APIs you enabled.
- [ ] **Store the Key Securely:**
    - [ ] Create a `.env` file in the root of the `wander-portfolio-client` directory.
    - [ ] Add the key to the file, prefixed with `VITE_`: `VITE_Google Maps_API_KEY=YourApiKeyHere`.
    - [ ] Add `.env` to your `.gitignore` file to prevent the key from being committed to version control.

---

## Phase 2: Backend API Development
**Goal:** Build the server-side API to handle data storage and retrieval, using an in-memory data store to focus on core API design and logic.

### Step 1: Define API Architecture and Data Models
- [ ] **Adopt RESTful Principles:** Design the API around resources (`trips`, `events`) using standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`).
- [ ] **Define Data Models:** Create the C# classes that will represent the application's data structure.
    - [ ] **Trip Model:** `id`, `city`, `startDate`, `endDate`, `events` (array).
    - [ ] **Event Model:** `id`, `name`, `address`, `lat`, `lng`, `day`.
- [ ] **Create In-Memory "Database":** In the server project, create a `db.js` file that exports an empty array (`trips`) to act as the in-memory data store.

### Step 2: Build Express API Endpoints (CRUD)
- [ ] **Implement Endpoints:** Build out the API routes according to the specification table below. Ensure the Express app is configured to parse JSON request bodies using `express.json()`.

| Method | Route                             | Description                         | Request Body (JSON)                                                  | Success Response (2xx)                            |
| ------ | --------------------------------- | ----------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------- |
| `POST` | `/api/trips`                      | Creates a new trip.                 | `{ "city": "string", "startDate": "string", "endDate": "string" }`   | `201 Created`, returns the new trip object.       |
| `GET`  | `/api/trips/:tripId`              | Retrieves a single trip by its ID.  | None                                                                 | `200 OK`, returns the full trip object.           |
| `POST` | `/api/trips/:tripId/events`       | Adds a new event to a trip.         | `{ "name": "string", "address": "string", ... }`                     | `201 Created`, returns the updated trip object.   |
| `PUT`  | `/api/trips/:tripId/events/:eventId`| Updates an existing event.          | `{ "name": "string", ... }`                                          | `200 OK`, returns the updated trip object.        |
| `DELETE`| `/api/trips/:tripId/events/:eventId`| Deletes an event from a trip.       | None                                                                 | `200 OK`, returns the updated trip object.        |

### Step 3: Refactor Server for Maintainability
- [ ] **Separate Routes:** Create a `routes/` directory and a `trips.js` file inside it. Move all trip-related route definitions into this file using `express.Router()`.
- [ ] **Separate Controllers:** Create a `controllers/` directory and a `tripsController.js` file. Move the business logic (the functions that handle requests) from the routes file into the controller file.
- [ ] **Clean up `app.js`:** Modify the main `app.js` file to import and mount the router. Export the `app` instance.
- [ ] **Create `server.js`:** Create a new `server.js` file whose only job is to import `app` from `app.js` and call `app.listen()` to start the server. This separates the app definition from the server execution, which is crucial for testing.

---

## Phase 3: Frontend UI/UX Development
**Goal:** Build the visual and interactive user interface using React's component-based architecture, breaking the UI into logical, reusable pieces.

### Step 1: Plan React Component Architecture
- [ ] **Define Component Hierarchy:** Plan the structure of the UI by breaking it down into a hierarchy of components as defined in the table below.

| Component Name      | Responsibility                                             | State Managed                                 | Key Props                             |
| ------------------- | ---------------------------------------------------------- | --------------------------------------------- | ------------------------------------- |
| `App`               | Main application shell, handles layout and global state.   | Current trip data (via Context API)           | None                                  |
| `TripSetup`         | Initial page for creating a new trip.                      | City input, start date, end date              | `onTripCreate` (function)             |
| `ItineraryPage`     | Main view showing the itinerary and map.                   | -                                             | `tripId` (from URL)                   |
| `CitySearchInput`   | Input with Google Places Autocomplete.                     | Search query, predictions                     | `onPlaceSelect` (function)            |
| `DateRangePicker`   | Allows selection of trip start and end dates.              | `startDate`, `endDate`                        | `onChange` (function)                 |
| `ItineraryView`     | Left-hand panel displaying the list of days and events.    | -                                             | `trip` (object)                       |
| `EventCard`         | Displays details for a single event.                       | -                                             | `event` (object), `onEdit`, `onDelete` |
| `MapView`           | Right-hand panel rendering the Google Map and markers.     | Map instance, markers                         | `events` (array)                      |
| `EventModal`        | Form/modal for adding or editing an event.                 | Form field values                             | `day`, `onSave`, `eventToEdit` (optional)|

### Step 2: Implement Core UI Components
- [ ] **Build `CitySearchInput`:** Use the `@vis.gl/react-google-maps` library's `useMapsLibrary('places')` hook and a `useRef` to attach Google's Autocomplete service to an input field.
- [ ] **Build `DateRangePicker`:** Install and use the `react-datepicker` library to create a calendar input for selecting a date range.
- [ ] **Build Static UI:** Create the other components (`ItineraryView`, `EventCard`, etc.) with static, hard-coded data first to focus on layout and styling before connecting them to live data.

### Step 3: Implement Global State with React Context
- [ ] **Create a Context:** In a new file, use `React.createContext()` to create a `TripContext`.
- [ ] **Create a Provider:** Create a `TripProvider` component that will manage the trip data state (`useState`) and fetch data from the API (`useEffect`). This component will wrap its children with `TripContext.Provider` and pass the data down via the `value` prop.
- [ ] **Wrap the Application:** In `App.jsx`, wrap the entire component tree with your `<TripProvider>`.
- [ ] **Consume the Context:** In components that need the trip data (like `MapView` or `ItineraryView`), use the `useContext(TripContext)` hook to access the shared state directly.

### Step 4: Integrate Google Maps
- [ ] **Install the Library:** Install the official Google Maps React library.
    ```bash
    npm install @vis.gl/react-google-maps
    ```
- [ ] **Wrap App in `APIProvider`:** In `App.jsx`, wrap the application with the `<APIProvider>` component, passing your `VITE_Google Maps_API_KEY` to its `apiKey` prop.
- [ ] **Create `MapView` Component:** Build the `MapView` component to render the `<Map>` component from the library.
- [ ] **Display Markers Dynamically:** Inside `<MapView>`, consume the `TripContext` to get the list of events. Use the `.map()` method on the events array to render an `<AdvancedMarker>` component for each event, setting its `position` prop to the event's coordinates.

---

## Phase 4: Bridging the Frontend and Backend
**Goal:** Connect the frontend and backend to create a fully dynamic application by making network requests from the React app to the Express server.

### Step 1: Implement Asynchronous Data Fetching
- [ ] **Use `fetch` and `useEffect`:** In components that need data (e.g., `ItineraryPage`), use the `useEffect` hook to call the browser's `fetch` API and make `GET` requests to your backend endpoints when the component mounts.
- [ ] **Handle POST/PUT/DELETE Requests:** Create functions that use `fetch` to make mutating requests (e.g., a function to create a new trip that sends a `POST` request to `/api/trips`).
- [ ] **Manage Loading and Error States:** Use `useState` to create `loading` and `error` state variables. Set them appropriately before, during, and after a `fetch` call to provide user feedback (e.g., show a loading spinner or an error message).

### Step 2: Configure CORS
- [ ] **Understand the Problem:** Recognize that by default, the browser will block requests from the frontend (`localhost:5173`) to the backend (`localhost:3001`) due to the Same-Origin Policy.
- [ ] **Install the `cors` Middleware:** In the `wander-portfolio-server` directory, install the `cors` package.
    ```bash
    npm install cors
    ```
- [ ] **Implement in Express:** In `app.js`, require the `cors` package and apply it as middleware *before* your route definitions.
    ```javascript
    const cors = require('cors');
    // ...
    app.use(cors()); // Allows all origins for local development
    ```

---

## Phase 5: Quality Assurance and Testing
**Goal:** Ensure the application is reliable and bug-free by implementing a comprehensive test suite for both the backend API and the frontend components.

### Step 1: Backend API Testing
- [ ] **Install Dependencies:** In the `wander-portfolio-server` directory, install Jest and Supertest as development dependencies.
    ```bash
    npm install --save-dev jest supertest
    ```
- [ ] **Configure Jest:** Add a `"test": "jest"` script to your `package.json`.
- [ ] **Write API Tests:** Create a `trips.test.js` file. Use `supertest` to make requests to your app instance and use Jest's `expect` function to assert that the HTTP status codes and response bodies are correct for each endpoint.

### Step 2: Frontend Component Testing
- [ ] **Understand the Philosophy:** Adopt React Testing Library's (RTL) user-centric philosophy: test your components by interacting with them as a user would.
- [ ] **Leverage Vite's Setup:** Use the pre-configured Jest, RTL, and `jest-dom` setup that comes with the Vite React template.
- [ ] **Write Component Tests:** Create test files (e.g., `TripSetup.test.jsx`). Use RTL's `render` function, query for elements with `screen`, simulate user actions with `userEvent`, and make assertions with `expect` to verify component behavior.

---

## Phase 6: Deployment and Go-Live
**Goal:** Deploy the application to the internet, carefully configuring the production environment so the frontend and backend can communicate correctly.

### Step 1: Prepare for Production
- [ ] **Centralize Environment Variables:** Identify all configuration variables needed for both development and production, as defined in the table below.

| Variable Name             | Location | Environment(s) | Purpose                                                      |
| ------------------------- | -------- | -------------- | ------------------------------------------------------------ |
| `VITE_Google Maps_API_KEY`| Frontend | Dev, Prod      | Public Google Maps API key for the React app.                |
| `VITE_API_BASE_URL`       | Frontend | Dev, Prod      | Base URL of the backend API (`http://localhost:3001` or the live Render URL). |
| `PORT`                    | Backend  | Prod           | Port number provided by the Render hosting environment.      |
| `CORS_ORIGIN`             | Backend  | Prod           | The specific URL of the deployed Vercel frontend to secure the API. |

### Step 2: Deploy Backend to Render
- [ ] **Push to GitHub:** Initialize a git repository for your project and push it to GitHub.
- [ ] **Create Render Web Service:** In the Render dashboard, create a new "Web Service" and connect it to your GitHub repository.
- [ ] **Configure Build Settings:**
    - **Root Directory:** `./wander-portfolio-server`
    - **Build Command:** `npm install`
    - **Start Command:** `node server.js`
- [ ] **Add Environment Variables:** In the service's "Environment" settings, add the `CORS_ORIGIN` variable (you will get this URL in the next step).
- [ ] **Deploy:** Click "Create Web Service" and monitor the logs. Your API will be live at a `.onrender.com` URL.

### Step 3: Deploy Frontend to Vercel
- [ ] **Create Vercel Project:** In the Vercel dashboard, create a new project and import the same GitHub repository.
- [ ] **Configure Build Settings:**
    - **Framework Preset:** Vercel should detect `Vite`.
    - **Root Directory:** Set this to `./wander-portfolio-client`.
- [ ] **Add Environment Variables:** In the project's settings, add the required production variables:
    - [ ] `VITE_Google Maps_API_KEY`
    - [ ] `VITE_API_BASE_URL` (Use the live URL from your Render deployment).
- [ ] **Deploy:** Click "Deploy". Vercel will build and deploy the application to its global CDN.

### Step 4: Create a Professional README
- [ ] **Finalize Documentation:** In the root of your GitHub repository, create a high-quality `README.md` file that turns your project into a compelling portfolio piece.
- [ ] **Include Key Sections:**
    - [ ] Project Title and a clear description.
    - [ ] A prominent link to the live Vercel demo.
    - [ ] Screenshots and/or an animated GIF of the application in action.
    - [ ] A bulleted list of key Features.
    - [ ] A list of the Technology Stack used.
    - [ ] Clear instructions on how another developer can clone, set up (including `.env` files), and run the project locally.