# Create React App in MacOS M1

1. Check if NodeJS and npm has been installed on your macbook:

   Open your Terminal, run:

    ```
    node --version
    ```
    ```
    npm --version
    ```
    If you see a `command not found` then need to install. 

2. Install NodeJS

   Go to https://nodejs.org/en/download and download for the LongTermSupport version(or Current as you want).

   Follow the installer to install NodeJS. The version commands in Step1 should be working now.

3. Create React app

   Run in your terminal under desired location:
   ```
   npx create-react-app project_name
   ```

# Run the React app

We suggest that you begin by typing:
```
cd project_name
npm start
```

### Inside that directory, you can run several commands:

```
npm start
```
Starts the development server.

```
  npm run build
```
Bundles the app into static files for production.

```
npm test
```
Starts the test runner.

```
  npm run eject
```
Removes this tool and copies build dependencies, configuration files
and scripts into the app directory. If you do this, you can’t go back!

