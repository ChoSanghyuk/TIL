# React



## React Ecosystem Overview



### 1.📦 Node.js

- **What:** A JavaScript runtime environment that runs outside the browser.
- **Why it’s used:** Needed to run build tools, servers, and package managers for React apps.
- **Example:** When you run npm run dev, Node.js is executing the script.



### 2. 📦 npm (Node Package Manager)

- **What:** A tool to install and manage JavaScript packages (like React).
- **Why it’s used:** Lets you install libraries (e.g. react, react-dom, axios) and run scripts defined in package.json.
- **Alternative:** yarn, pnpm



### 3. ⚛️ React

- **What:** A JavaScript library for building **user interfaces**.
- **Why it’s used:** Lets you build interactive UIs using **components** and **state management**.
- **Core packages:**
  - react: the UI library
  - react-dom: connects React to the browser



### 4. React Frameworks & Build Tools

| **Tool**                   | **Type**       | **Purpose**                                                  |
| -------------------------- | -------------- | ------------------------------------------------------------ |
| **Next.js**                | Full framework | Adds **routing**, **SSR/SSG**, **API routes**, and more      |
| **Vite**                   | Build tool     | Fast dev server and build pipeline (used with plain React)   |
| **CRA (Create React App)** | Starter kit    | Sets up Webpack & Babel to start coding quickly (legacy/less favored now) |



### 5. Routing

React itself doesn’t include routing. These tools handle it:

- react-router-dom → for **client-side routing**
- Next.js → has built-in **file-based routing**



### 6. Styling

Options to style your app:

- **CSS Modules** – Scoped styles per component
- **Styled-components** / **Emotion** – CSS-in-JS
- **Tailwind CSS** – Utility-first CSS framework
- **Sass / Less** – Preprocessors



### 7. Build Tools & Compilers

| **Tool**     | **Role**                                          |
| ------------ | ------------------------------------------------- |
| **Vite**     | Fast bundler + dev server                         |
| **Webpack**  | Older but powerful bundler (used by CRA, Next.js) |
| **Babel**    | JS transpiler (e.g. ES6 → ES5)                    |
| **ESLint**   | Linter for JS/React code                          |
| **Prettier** | Code formatter                                    |



### 8. State Management

Manages application state across components.

| **Tool**                                                     | **Use Case**            |
| ------------------------------------------------------------ | ----------------------- |
| **React Context**                                            | Basic, built-in         |
| **Redux**                                                    | Complex state logic     |
| **Zustand**                                                  | Lightweight alternative |
| **Recoil**                                                   | Atom-based state        |
| **Jotai**, **MobX**, **XState** – also used in certain projects |                         |



### 9. API/Data Handling

Common tools for calling backend APIs:

- axios or fetch – for HTTP requests
- react-query / tanstack-query – for **data fetching & caching**
- SWR – lightweight alternative from Vercel



### 10. Testing

Tools for unit, integration, and E2E testing:

| **Tool**                  | **Purpose**                |
| ------------------------- | -------------------------- |
| **Jest**                  | Unit testing               |
| **React Testing Library** | Component testing          |
| **Cypress**               | End-to-end testing         |
| **Playwright**            | Browser automation testing |



### 11. Package Bundlers

These tools build your app for production:

- **Vite** (ESBuild under the hood)
- **Webpack** (used by CRA/Next)
- **Rollup** (used in libraries)



### 12. Deployment

Popular platforms to deploy React apps:

- **Vercel** → Best for **Next.js**
- **Netlify** → Good for Vite, CRA, static sites
- **Render**, **Firebase Hosting**, **AWS Amplify**, etc.



✅ Summary Table

| **Name**     | **Category**         | **Purpose**                              |
| ------------ | -------------------- | ---------------------------------------- |
| React        | UI Library           | Build UI with components                 |
| Node.js      | Runtime              | Run JS on server & tooling               |
| npm / yarn   | Package Manager      | Install & manage dependencies            |
| Next.js      | Full Framework       | Routing, SSR, APIs, optimized deployment |
| Vite         | Dev Server / Bundler | Fast dev & build for React               |
| CRA          | Starter Kit (legacy) | Quick React project setup                |
| React Router | Routing              | Navigation within the app                |
| Tailwind     | Styling              | Utility-first CSS                        |
| Redux        | State Management     | Centralized app state                    |
| Jest         | Testing              | Unit testing                             |
| Axios        | API Call             | Fetch data from backend                  |





## Next.js 



### Next.js routing

- Uses file-based routing for both pages AND API routes
  - app/api/free-joke/route.ts → creates /api/free-joke endpoint
  - app/page.tsx → creates / page

