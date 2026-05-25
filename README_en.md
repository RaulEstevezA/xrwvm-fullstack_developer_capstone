[Back to main README](README.md) | [Leer en español](README_es.md)

# Full Stack Application Development Project

Full-stack car dealership review application created as a learning project from
IBM's **Full Stack Application Development Project** course on edX.

The application lets users browse dealerships, filter them by state, open dealer
details, register or log in, submit reviews, and see an automatically generated
sentiment marker for each review.

<p align="center">
  <img src="images/home.png" alt="Home page" width="600">
</p>

## Table of Contents

- [About the Project](#about-the-project)
- [Features](#features)
- [Architecture](#architecture)
- [Screenshots](#screenshots)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Local Setup](#local-setup)
- [Useful Endpoints](#useful-endpoints)
- [Author](#author)
- [License](#license)

## About the Project

This repository is based on IBM Developer Skills Network course material for the
edX course:

[Full Stack Application Development Project](https://www.edx.org/learn/full-stack-development/ibm-full-stack-application-development-project?index=rv_product_summary&queryId=77288f71350e59e3061c194489b4387e&position=1)

It is structured as a multi-service project:

- Django handles the main backend, authentication, admin panel, static pages,
  and API routing.
- React provides the interactive dealership, login, registration, and review UI.
- Express exposes dealership and review data from MongoDB.
- Flask provides review sentiment analysis using NLTK/VADER.

## Features

- Home, About, and Contact pages served by Django.
- User registration, login, logout, and session-based review access.
- Django admin panel.
- Dealership listing with state filter.
- Dealer detail page with reviews.
- Review submission form for authenticated users.
- Car make/model data populated through Django models.
- Sentiment classification for reviews: positive, neutral, or negative.
- MongoDB seed data for dealerships and reviews.

## Architecture

```text
React UI
  |
  v
Django application
  |-- SQLite: Django users and car models
  |-- Express API: dealerships and reviews
  |-- Flask sentiment service: review analysis

Express API
  |
  v
MongoDB
```

Default service ports:

| Service | Path | Port |
| --- | --- | --- |
| Django app | `server/` | `8000` |
| React app | `server/frontend/` | `3000` |
| Express/Mongo API | `server/database/` | `3030` |
| Flask sentiment service | `server/djangoapp/microservices/` | `5050` |
| MongoDB | Docker service | `27017` |

## Screenshots

<h3 align="center">Admin Login</h3>

<p align="center">
  <img src="images/admin_login.png" alt="Admin login screen" width="600">
</p>

<h3 align="center">Home</h3>

<p align="center">
  <img src="images/home.png" alt="Application home page" width="600">
</p>

<h3 align="center">Dealership List</h3>

<p align="center">
  <img src="images/get_dealers.png" alt="Get dealers screen" width="600">
</p>

<h3 align="center">Review Submission</h3>

<p align="center">
  <img src="images/dealership_review_submission.png" alt="Dealership review submission screen" width="600">
</p>

<h3 align="center">Dealer Details With Sentiment</h3>

<p align="center">
  <img src="images/deployed_add_review.png" alt="Dealer details after adding a review" width="600">
</p>

## Tech Stack

- Python
- Django
- React
- JavaScript
- Node.js
- Express
- MongoDB
- Flask
- NLTK/VADER sentiment analysis
- Docker and Docker Compose
- SQLite

## Project Structure

```text
.
|-- images/                         # Project screenshots
|-- server/
|   |-- djangoapp/                   # Django app, views, URLs, models, API helpers
|   |   |-- microservices/            # Flask sentiment analyzer
|   |-- djangoproj/                  # Django project settings and root URLs
|   |-- database/                    # Express API, Mongo models, seed JSON, Docker files
|   |-- frontend/                    # React application
|   |-- manage.py
|   |-- requirements.txt
|-- LICENSE
|-- README.md
|-- README_en.md
|-- README_es.md
```

## Local Setup

The project has several moving pieces. Run each service in its own terminal.

### 1. Backend Python Environment

```bash
cd server
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Build the React Frontend

```bash
cd server/frontend
npm install
npm run build
```

For React development mode:

```bash
cd server/frontend
npm start
```

### 3. Start MongoDB and the Express API

```bash
cd server/database
npm install
docker build -t nodeapp .
docker compose up
```

The Express API is available at `http://localhost:3030`.

### 4. Start the Sentiment Microservice

```bash
cd server/djangoapp/microservices
pip install -r requirements.txt
flask run --host=0.0.0.0 --port=5050
```

The Django helper expects the sentiment analyzer at `http://localhost:5050/` by
default.

### 5. Start Django

```bash
cd server
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Open `http://localhost:8000`.

Optional environment variables can be placed in `server/djangoapp/.env`:

```env
backend_url=http://localhost:3030
sentiment_analyzer_url=http://localhost:5050/
```

## Useful Endpoints

### Django

| Endpoint | Description |
| --- | --- |
| `/` | Home page |
| `/admin/` | Django admin |
| `/djangoapp/register` | Register user |
| `/djangoapp/login` | Log in |
| `/djangoapp/logout` | Log out |
| `/djangoapp/get_dealers` | Get all dealerships |
| `/djangoapp/get_dealers/<state>` | Get dealerships by state |
| `/djangoapp/dealer/<dealer_id>` | Get dealer details |
| `/djangoapp/reviews/dealer/<dealer_id>` | Get dealer reviews with sentiment |
| `/djangoapp/add_review` | Submit a review |
| `/djangoapp/get_cars` | Get car makes and models |

### Express API

| Endpoint | Description |
| --- | --- |
| `/fetchDealers` | Fetch all dealerships |
| `/fetchDealers/:state` | Fetch dealerships by state |
| `/fetchDealer/:id` | Fetch one dealer |
| `/fetchReviews` | Fetch all reviews |
| `/fetchReviews/dealer/:id` | Fetch reviews for one dealer |
| `/insert_review` | Insert a new review |

## Author

- LinkedIn: [linkedin.com/in/raulesteveza](https://www.linkedin.com/in/raulesteveza/)
- Web: [raulesteveza.github.io](https://raulesteveza.github.io)
- GitHub: [github.com/RaulEstevezA](https://github.com/RaulEstevezA)

## License

This repository keeps the original IBM Developer Skills Network license notice.
The project is distributed under the Apache License 2.0. See [LICENSE](LICENSE)
for the full license text.
