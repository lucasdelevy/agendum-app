# Agendum App

A professional Vue.js calendar application with authentication and weekly view.

## Features

- **Authentication System** - Login/logout with session persistence
- **Weekly Calendar View** - Navigate through weeks with arrow controls
- **Hourly Grid** - 24-hour time slots with current time indicator
- **Current Day Highlighting** - Visual indication of today's column
- **Professional UI** - Sidebar navigation with hamburger menu
- **Responsive Design** - Clean, modern interface

## Getting Started

### Prerequisites

- Node.js (version 14 or higher)
- npm or yarn

### Installation

1. Clone or navigate to the project directory:
   ```bash
   cd agendum-app
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

4. Open your browser and go to `http://localhost:5173`

### Login

- Enter any email and password to log in (mock authentication)
- Session persists across browser refreshes
- Use the logout button in the header to sign out

## Project Structure

```
src/
├── components/
│   ├── AppHeader.vue      # Main header with menu and logout
│   ├── LoginPage.vue      # Authentication form
│   ├── Sidebar.vue        # Navigation menu
│   └── WeeklyCalendar.vue # Main calendar component
├── App.vue                # Root component with auth logic
└── main.js               # Application entry point
```

## Build for Production

```bash
npm run build
```

The built files will be in the `dist/` directory.