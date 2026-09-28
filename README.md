# coit-frontend: Sentiment Analysis UI

A React single-page app where you type a sentence and see its sentiment polarity. The sentence is sent to the [coit-backend1](https://github.com/nixvarghese01/coit-backend1) web API.

The app is a three-tier sentiment-analysis demo: you type a sentence in the web UI and it returns a polarity score.

```
React frontend  --POST /sentiment-->  Spring Boot web API  --POST /analyse/sentiment-->  Python (Flask + TextBlob)
   (nginx :80)                            (:8080)                                         (:5000)
```

## Tech stack
React 16 (Create React App), nginx or Apache for serving, Docker, Jenkins, Knative, SonarQube

## Run locally
```bash
yarn install      # or: npm install
yarn start        # http://localhost:3000
yarn build        # production build in ./build
```

> The backend URL is hardcoded in `src/App.js` (`fetch('http://<host>:8080/sentiment')`). Change it to point at your web API.

## Docker
| File | What it does |
|---|---|
| `Dockerfile` | nginx serving a local `build/` (run `yarn build` first) |
| `Dockerfile-apache` | Apache on Ubuntu serving a local `build/` |
| `Dockerfile-dev` / `Dockerfile-multistage` | Builds with Node 16, then serves with nginx (no local build needed) |

```bash
docker build -f Dockerfile-multistage -t <dockerhub-user>/sentiment-analysis-frontend .
docker run -d -p 80:80 <dockerhub-user>/sentiment-analysis-frontend
```

## Deployment
- `Jenkinsfile`: builds the app and archives `build/`; sends an email on failure
- `knative_service.yaml`: deploys the image as a Knative Service
