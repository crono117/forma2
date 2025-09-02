# Vue 3 + PixiJS + DragonBones Avatar Demo

This project is a demonstration of how to integrate PixiJS and DragonBones into a Vue 3 application using TypeScript and Vite. It displays a 2D animated avatar on an HTML5 canvas.

## Technologies Used

- **Vue 3:** with Composition API and `<script setup>`
- **TypeScript**
- **Vite:** as the build tool
- **PixiJS v8:** for 2D rendering
- **dragonbones-pixijs:** the PixiJS runtime for DragonBones

## Project Setup

### Prerequisites

- Node.js (v18 or higher recommended)
- npm

### Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

## Running the Development Server

To start the Vite development server, run the following command:

```bash
npm run dev
```

This will start the application, and you can view it in your browser at the URL provided in the console (usually `http://localhost:5173`).

## Project Structure

- `public/assets/`: Contains the DragonBones asset files (`_ske.json`, `_tex.json`, `_tex.png`).
- `src/components/DragonBonesAvatar.vue`: The core Vue component that handles the PixiJS and DragonBones integration.
- `src/App.vue`: The main application component.
- `src/main.ts`: The entry point of the application.
