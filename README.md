# Swappify

A modern React + Vite application with TailwindCSS for swapping and sharing items within your community.

## Installation

Install all dependencies:

```bash
npm install
```

## Running the App

Start the development server:

```bash
npm run dev
```

The app will be available at `http://localhost:5173` (or another port if 5173 is busy).

## Building for Production

Create a production build:

```bash
npm run build
```

Preview the production build:

```bash
npm run preview
```

## Project Structure

```
swapcircle/
├── index.html          # Main HTML file with root div
├── package.json        # Dependencies and scripts
├── vite.config.js      # Vite configuration
├── tailwind.config.js  # TailwindCSS configuration
├── postcss.config.js   # PostCSS configuration for Tailwind
├── src/
│   ├── main.jsx        # Entry point - renders App into #root
│   ├── index.css       # Global styles with Tailwind directives
│   └── App.jsx         # Main application component
└── README.md           # This file
```

## Adding More Pages

To add more pages to your application:

1. **Create new page components** in the `src/` folder (e.g., `src/About.jsx`, `src/Dashboard.jsx`)
2. **Install React Router** for navigation:
   ```bash
   npm install react-router-dom
   ```
3. **Set up routing** in `src/main.jsx` or `src/App.jsx` using `BrowserRouter`, `Routes`, and `Route` components
4. **Update navigation links** in the Navbar section of `App.jsx` to use React Router's `Link` component

## Creating Reusable Components

For better code organization, create a `src/components/` folder:

```
src/
├── components/
│   ├── Navbar.jsx      # Extract navbar from App.jsx
│   ├── Hero.jsx        # Extract hero section
│   ├── ItemCard.jsx    # Reusable item card component
│   └── ...
├── pages/
│   ├── Home.jsx        # Home page
│   ├── Dashboard.jsx   # Dashboard page
│   └── ...
├── App.jsx
├── main.jsx
└── index.css
```

Then import and use these components in your pages.

## Technologies Used

- **React 18** - UI library
- **Vite** - Fast build tool and dev server
- **TailwindCSS** - Utility-first CSS framework
- **PostCSS** - CSS processing tool

## Learn More

- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [TailwindCSS Documentation](https://tailwindcss.com/)

Happy coding! 🚀
