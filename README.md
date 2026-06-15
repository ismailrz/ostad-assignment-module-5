# Node Express App

A simple Node.js + Express web server with automated CI/CD via GitHub Actions.

## Prerequisites

- Node.js v22

## Getting Started

```bash
# Install dependencies
npm install

# Run the application
npm start

# Run tests
npm run check
```

The server starts on port `3000` by default. Set the `PORT` environment variable to override.

## API Routes

| Route | Method | Response |
|-------|--------|----------|
| `/` | GET | Serves `index.html` (HTML page) |
| `/api` | GET | `{ "message": "Hello World" }` (JSON) |

## Testing

Tests are written with Mocha, Chai, and chai-http and run against a local server on port `3001`.

```bash
npm run check
```

## CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/ci-cd.yml`) triggers on every push to `main` and runs two sequential jobs:

### 1. Test Job (`ubuntu-latest`)
1. Checks out code
2. Sets up Node.js v22
3. Installs dependencies
4. Runs the test suite and saves output to `test-results.txt`
5. Uploads `test-results.txt` as a build artifact

### 2. Deploy Job (self-hosted runner)
> Runs only after the test job succeeds.

1. Checks out code
2. Downloads the `test-results` artifact and prints it
3. Installs dependencies
4. Deploys with PM2:
   ```bash
   pm2 delete ostad-app || true
   pm2 start src/server.js --name ostad-app
   pm2 save
   ```
5. Verifies deployment with `pm2 status`

## Project Structure

```
.
├── src/
│   └── server.js        # Express server
├── test/
│   └── server.test.js   # Mocha/Chai tests
├── .github/
│   └── workflows/
│       └── ci-cd.yml    # GitHub Actions pipeline
└── package.json
```
