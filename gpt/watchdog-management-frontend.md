# Frontend Design Document

## Introduction

---

This document outlines the frontend design for a web application to manage watchdogs deployed on multiple servers. The application will enable ops engineers to search, start, stop, and view metadata and status information for each watchdog.

## User Stories

---

1. As an ops engineer, I want to search for watchdogs by name or other relevant attributes, so that I can easily find the specific watchdog I'm looking for among the large number of watchdogs.
2. As an ops engineer, I want to start a watchdog, so that I can enable monitoring and ensure the processes it watches are running as expected.
3. As an ops engineer, I want to stop a watchdog, so that I can pause monitoring or perform maintenance tasks.
4. As an ops engineer, I want to view a watchdog's metadata, so that I can get detailed information about its configuration, history, and purpose.
5. As an ops engineer, I want to view the current status of watchdogs, so that I can monitor their health, performance, and activity in real-time.
6. As an ops engineer, I want to see the change history of a watchdog, so that I can review past modifications, identify any issues, and understand the evolution of the watchdog's configuration over time.

## UI/UX

---

### `Home page:`

Use a welcoming hero section with a clear heading and a brief description of the application.
Consider adding quick action buttons or cards for navigating to the main sections of the application, such as "Manage Watchdogs" and "Settings."
Optionally, you can include some statistics or insights (e.g., the total number of watchdogs, active watchdogs, etc.) using Ant Design's Statistic or Card components.
Watchdog page:

Place a search bar at the top of the page using Ant Design's Input.Search component to allow users to search for watchdogs easily.
Below the search bar, display a table containing the list of watchdogs using Ant Design's Table component. Show relevant columns like watchdog name, status, and actions (start/stop buttons).

Implement pagination for the table using Ant Design's Pagination component to allow users to navigate through a large number of watchdogs.
For start/stop actions, use Ant Design's Button component with appropriate icons (e.g., play and pause icons).

When a user clicks on a watchdog's name or a "details" button, open a modal or a separate page with more information about the watchdog, including its metadata, status, and change history. Use Ant Design's Modal or Descriptions components for this purpose.

### `Settings page:`

Organize settings into different sections or tabs, such as general settings, notification settings, and user management. Use Ant Design's Tabs component for this purpose.
Within each settings section, use Ant Design's Form component to create forms for updating various settings. Use appropriate form controls like Input, Switch, Radio, and Checkbox for different types of settings.
Navigation and layout:

Use Ant Design's Layout component with a Sider to create a sidebar for navigating between different pages. Include icons and text labels for each menu item.
Place your application's logo or name at the top of the sidebar, and include a user menu (with the user's name or avatar) at the bottom of the sidebar for accessing user settings, profile, and logout options. Use Ant Design's Dropdown and Avatar components for this purpose.

## Frontend Structure

---

### Client-Side Rendering and Independent Frontend Server

We'll use client-side rendering and an independent frontend server for the following reasons:

- Separation of concerns: By decoupling the frontend and backend, we can develop and deploy each part independently, which simplifies maintenance and updates.
- Scalability: The independent frontend server can be scaled separately from the backend, ensuring the application remains responsive under heavy load.
- Improved performance: Client-side rendering allows for faster initial load times and smoother page transitions, providing a better user experience.

### Folder Structure

- `src/`
  - `components/`: Reusable UI components (e.g., sidebar, search bar, table, etc.)
  - `containers/`: Container components responsible for managing state and connecting to the Redux store
  - `pages/`: Components representing each page in the application (e.g., home, watchdog, settings)
  - `redux/`: Redux store configuration, actions, and reducers
  - `services/`: API service functions and utility functions
  - `styles/`: Global styles and theme configuration
  - `tests/`: Test files for unit, integration, and end-to-end testing
  - `index.tsx`: Entry point for the application
  - `App.tsx`: Main application component
- `public/`: Static files and assets (e.g., favicon, images)
- `.eslintrc.json`: ESLint configuration file
- `.prettierrc`: Prettier configuration file
- `.gitignore`: Git ignore file to exclude specified files and directories from version control
- `package.json`: Package dependencies and scripts

### example package.json

```json
{
  "name": "watchdog-management",
  "version": "1.0.0",
  "description": "A web application to manage watchdogs deployed on multiple servers.",
  "main": "index.js",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "lint": "eslint 'src/**/*.tsx'",
    "lint:fix": "eslint --fix 'src/**/*.tsx'",
    "prettier": "prettier --check 'src/**/*.tsx'",
    "prettier:fix": "prettier --write 'src/**/*.tsx'"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/yourusername/watchdog-management.git"
  },
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/yourusername/watchdog-management/issues"
  },
  "homepage": "https://github.com/yourusername/watchdog-management#readme",
  "dependencies": {
    "@ant-design/icons": "^4.7.0",
    "@ant-design/pro-table": "^2.60.0",
    "@testing-library/jest-dom": "^5.14.1",
    "@testing-library/react": "^12.1.2",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.0.2",
    "@types/node": "^16.11.12",
    "@types/react": "^17.0.37",
    "@types/react-dom": "^17.0.11",
    "antd": "^4.17.3",
    "axios": "^0.24.0",
    "cypress": "^9.1.1",
    "keycloak-js": "^16.0.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-redux": "^7.2.6",
    "react-router-dom": "^6.2.1",
    "react-scripts": "4.0.3",
    "redux": "^4.1.2",
    "redux-toolkit": "^1.7.1",
    "typescript": "^4.5.4"
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^5.7.0",
    "@typescript-eslint/parser": "^5.7.0",
    "eslint": "^8.5.0",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-prettier": "^4.0.0",
    "eslint-plugin-react": "^7.27.1",
    "eslint-plugin-react-hooks": "^4.3.0",
    "prettier": "^2.5.1"
  },
  "eslintConfig": {
    "parser": "@typescript-eslint/parser",
    "extends": [
      "react-app",
      "plugin:react/recommended",
      "plugin:@typescript-eslint/recommended",
      "plugin:prettier/recommended"
    ],
    "plugins": [
      "react",
      "@typescript-eslint"
    ],
    "rules": {
      "react/react-in-jsx-scope": "off",
      "react/jsx-filename-extension": [
        1,
        {
          "extensions": [
            ".js",
            ".jsx",
            ".ts",
            ".tsx"
        ]
      ]
    },
    "settings": {
      "react": {
        "version": "detect"
      }
    }
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}

```

## Tech Stack

---

- Programming language: TypeScript
- Framework: React
- UI library: Ant Design
- State management: Redux Toolkit
- Authentication: Keycloak
- Package management: npm or yarn
- Unit testing: Jest
- End-to-end testing: Cypress

## Testing

---

- Unit testing: Write unit tests for individual components and utility functions using Jest. Test the functionality and behavior of your components in isolation, including rendering, user interactions, and state changes. Use the React Testing Library to test your components in a way that resembles how they would be used by real users.
- Integration testing: Test the interaction between different components and parts of your application. This can include testing the interaction between the Redux store and your components, as well as the communication between the frontend and the backend. Use Jest and the React Testing Library for integration testing as well.

- End-to-end testing: Write end-to-end tests using Cypress to simulate user interactions with the application in a real browser environment. This type of testing helps ensure that the application works correctly as a whole, from frontend to backend. Create test scenarios that cover critical user flows, such as searching for watchdogs, starting and stopping watchdogs, and viewing watchdog metadata and status information.

- Test organization: Organize your tests in a logical manner, such as mirroring the folder structure of your application. Keep test files in a separate tests/ folder, or place them alongside the components and modules they test.
