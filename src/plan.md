PLAN
====

Project: Leftovers - Free Food Sharing Desktop App

Objective:
Develop a desktop application using Java, Spring Boot, and JavaFX to allow users to share and claim free food based on their location, reducing food waste. The app will include a backend for managing food listings and users, and a JavaFX GUI for posting food, viewing nearby listings, and claiming food.

---

1. Project Scope
----------------
- Core Features:
  - Post food items with title, description, and location (latitude/longitude).
  - View nearby food listings within a configurable radius (default: 10 km).
  - Claim food to mark it as unavailable.
  - Simple map visualization using JavaFX Canvas to show user and food locations.
- Tech Stack:
  - Backend: Java 17, Spring Boot, Spring Data JPA, H2 Database, Lombok
  - Frontend: JavaFX (controls and FXML)
  - Database: In-memory H2 database (for development)
- Constraints:
  - JavaFX lacks native geolocation; use manual coordinate input.
  - Keep dependencies minimal; avoid external mapping libraries for simplicity.
  - No authentication for initial version to focus on core functionality.
Below is a mock `PLAN.txt` file outlining a high-level plan for developing the "Leftovers" desktop application, a Java-based app using Spring Boot and JavaFX to reduce food waste by enabling users to share and claim free food based on location. The plan assumes the use of the provided `FoodItem`, `User`, `Location`, and `MainApp` classes, along with the `application.properties`, `pom.xml`, and the updated JavaFX-based `README.md`. It details the approach, tasks, and considerations for tackling the project, written as if planning the development from scratch.

2. Development Approach
----------------------
- Use Agile methodology with iterative development.
- Break down into phases: setup, backend, frontend, integration, testing, and deployment.
- Prioritize minimal viable product (MVP) with core features first, then consider enhancements.
- Use Maven for dependency management and project builds.
- Test incrementally using H2 console and manual GUI testing.

3. Task Breakdown
-----------------

### Phase 1: Project Setup (1-2 days)
- [ ] Set up development environment:
  - Install Java 17, Maven 3.6+, and JavaFX SDK (OpenJFX 17+).
  - Configure IDE (e.g., IntelliJ IDEA) with JavaFX module path.
- [ ] Create Maven project:
  - Place `pom.xml` in root directory with dependencies for Spring Boot, H2, Lombok, and JavaFX.
  - Ensure `javafx-maven-plugin` is included for GUI builds.
- [ ] Define project structure:
  - Create `src/main/java/com/leftovers/app/` for Java classes.
  - Create `src/main/resources/` for `application.properties`.

### Phase 2: Backend Development (3-4 days)
- [ ] Design data model:
  - Create `FoodItem` class (entity with ID, title, description, latitude, longitude, postedAt, isAvailable, user).
  - Create `User` class (entity with ID, username, email, password, latitude, longitude).
- [ ] Implement repositories:
  - Embed `FoodItemRepository` in `FoodItem` with custom Haversine query for nearby listings.
  - Embed `UserRepository` in `User` for basic CRUD and email lookup.
- [ ] Implement REST API:
  - Add controller methods in `FoodItem` for:
    - GET `/api/food/nearby` (find nearby listings).
    - POST `/api/food` (create food listing).
    - POST `/api/food/{id}/claim` (claim food).
- [ ] Configure database:
  - Create `application.properties` in `src/main/resources/` for H2 in-memory database.
  - Use `Location.generateConfig()` to generate the config file programmatically.
  - Enable H2 console for debugging (`http://localhost:8080/h2-console`).

### Phase 3: Frontend Development (4-5 days)
- [ ] Design JavaFX GUI in `MainApp` class:
  - Use `TabPane` with two tabs: "Post Food" and "View Nearby".
  - **Post Food Tab**:
    - Add fields for title, description, latitude, longitude.
    - Include "Post Food" button to send POST request to `/api/food`.
  - **View Nearby Tab**:
    - Add fields for user latitude, longitude, and distance.
    - Include "Search Nearby" button to fetch listings via GET `/api/food/nearby`.
    - Use `ListView` to display food items.
    - Use `Canvas` to draw a simple map (user as circle, food as squares).
    - Allow clicking a list item to claim food via POST `/api/food/{id}/claim`.
- [ ] Handle location input:
  - Use text fields for manual latitude/longitude input (default: 51.505, -0.09).
  - Display warning if coordinates are invalid.
- [ ] Integrate with backend:
  - Use `RestTemplate` to make HTTP requests to the Spring Boot API.
  - Handle errors with `Alert` dialogs for user feedback.

### Phase 4: Integration and Testing (2-3 days)
- [ ] Integrate backend and frontend:
  - Ensure `FoodItem.main()` launches both Spring Boot and JavaFX (`MainApp.launchApp()`).
  - Verify API calls from JavaFX to backend work correctly.
- [ ] Test core functionality:
  - Post food with valid/invalid inputs.
  - Search for nearby listings with different coordinates and distances.
  - Claim food and verify it becomes unavailable.
  - Check H2 console for data consistency.
- [ ] Debug issues:
  - Use logging (`logging.level.com.leftovers.app=DEBUG`) for backend errors.
  - Test JavaFX GUI responsiveness and error handling.

### Phase 5: Documentation and Deployment (1-2 days)
- [ ] Write `README.md`:
  - Include project overview, features, tech stack, setup instructions, usage, and troubleshooting.
  - Document `pom.xml` and `application.properties` setup.
- [ ] Package the app:
  - Build executable JAR with `mvn clean install`.
  - Test running the JAR with JavaFX VM options.
- [ ] Plan for production (optional):
  - Outline steps for switching to PostgreSQL.
  - Suggest adding Spring Security for authentication.

4. Key Considerations
---------------------
- **Geolocation**: JavaFX lacks native geolocation. Manual coordinate input is a workaround. For a real app, consider integrating a geolocation API or platform-specific libraries.
- **Map Visualization**: The Canvas-based map is basic. For advanced mapping, explore embedding a web view with Leaflet or using JXMapViewer (additional dependency).
- **Scalability**: The in-memory H2 database is suitable for development but not production. Plan to switch to a persistent database.
- **Security**: No authentication in MVP. Add Spring Security for user login in future iterations.
- **Mobile Support**: JavaFX is desktop-focused. For mobile, consider Gluon Mobile or rewriting the frontend in a mobile framework (e.g., Android SDK).

5. Timeline
-----------
- Total Estimated Time: 11-16 days (assuming 1 developer, ~4-6 hours/day)
- Phase 1: 1-2 days
- Phase 2: 3-4 days
- Phase 3: 4-5 days
- Phase 4: 2-3 days
- Phase 5: 1-2 days

6. Risks and Mitigation
-----------------------
- **Risk**: JavaFX setup issues (e.g., module path errors).
  - **Mitigation**: Provide clear IDE setup instructions in `README.md` and test on multiple environments.
- **Risk**: Backend-frontend integration issues (e.g., API call failures).
  - **Mitigation**: Use `RestTemplate` with robust error handling and test API endpoints with tools like Postman.
- **Risk**: Limited map functionality due to Canvas.
  - **Mitigation**: Document limitations and suggest advanced mapping libraries for future enhancements.

7. Future Enhancements
----------------------
- Add user authentication with Spring Security.
- Implement image uploads for food listings.
- Integrate a geolocation API for automatic location detection.
- Enhance the map with a proper mapping library or web view.
- Add notifications for new nearby listings.
- Adapt for mobile using Gluon Mobile or a native mobile framework.

8. Milestones
-------------
- **MVP Complete**: Basic posting, viewing, and claiming functionality with JavaFX GUI and H2 database.
- **Beta Release**: Add error handling, input validation, and improved UI styling.
- **Production Ready**: Add authentication, persistent database, and advanced mapping.

---
### Placement
- Save this content as `PLAN.txt` in the **root directory** of the project, alongside `pom.xml` and `README.md`:
  ```
  leftovers/
  ├── PLAN.txt
  ├── README.md
  ├── pom.xml
  ├── src/
  │   ├── main/
  │   │   ├── java/
  │   │   │   └── com/leftovers/app/
  │   │   │       ├── FoodItem.java
  │   │   │       ├── User.java
  │   │   │       ├── Location.java
  │   │   │       └── MainApp.java
  │   │   └── resources/
  │   │       └── application.properties
  ```

### Explanation
- **Purpose**: The `PLAN.txt` outlines a structured approach to developing the "Leftovers" desktop app, covering scope, tasks, timeline, risks, and future enhancements.
- **Alignment with Code**: Reflects the provided `FoodItem`, `User`, `Location`, and `MainApp` classes, the JavaFX frontend, and the `application.properties` and `pom.xml` configurations.
- **Key Sections**:
  - **Scope**: Defines core features and tech stack, noting the switch to JavaFX and manual location input.
  - **Approach**: Uses Agile with iterative phases for setup, backend, frontend, integration, and documentation.
  - **Tasks**: Breaks down development into actionable steps, aligned with the provided code structure.
  - **Considerations**: Addresses JavaFX limitations (e.g., no geolocation) and plans for production readiness.
  - **Timeline**: Estimates 11-16 days for a single developer, realistic for an MVP.
  - **Risks**: Identifies potential issues like JavaFX setup or integration, with mitigation strategies.
  - **Enhancements**: Suggests future features like authentication or mobile support, consistent with the `README.md`.
- **Format**: Simple text file for clarity, suitable for project planning and sharing with stakeholders.

### Notes
- The plan assumes a single developer working part-time (4-6 hours/day). Adjust the timeline for multiple developers or different work schedules.
- The `Creative Commons Legal Code License` from the `README.md` is noted but not detailed here, as licensing is typically handled in `README.md`.
- If you need specific additions (e.g., detailed testing strategies, team roles, or budget estimates), let me know, and I can expand the plan.
