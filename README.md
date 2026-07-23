# Movie Database

A Rails 7 CRUD application for managing a movie catalog. Supports creating, viewing, editing, and deleting movie records through a web UI and a JSON API.

## Stack

- **Ruby** 3.3.8
- **Rails** 7.1
- **Database** — SQLite3 in development/test, PostgreSQL in production
- **Frontend** — Hotwire (Turbo + Stimulus), importmap
- **Web server** — Puma

## Data model

| Column | Type | Notes |
|---|---|---|
| `title` | string | |
| `rating` | string | e.g. PG, R |
| `description` | text | |
| `release_date` | datetime | |
| `created_at` / `updated_at` | datetime | managed by Rails |

## Routes

```
GET    /movies          # list all movies
GET    /movies/:id      # show a movie
GET    /movies/new      # new movie form
POST   /movies          # create a movie
GET    /movies/:id/edit # edit form
PATCH  /movies/:id      # update a movie
DELETE /movies/:id      # delete a movie
```

The root path (`/`) redirects to `/movies`. All routes also respond to `.json`.

## Local setup

```bash
bundle install
bin/rails db:create db:migrate
bin/rails server
```

The app is then available at `http://localhost:3000`.

## Tests

```bash
bin/rails test            # unit + controller tests
bin/rails test:system     # Capybara/Selenium system tests
```

## Docker

Build and run the production image locally:

```bash
docker build -t movie-database .
docker run -e DATABASE_URL=<your-pg-url> -e RAILS_MASTER_KEY=<key> -p 3000:3000 movie-database
```

## Deployment (Render)

The `bin/render-build.sh` script runs on every deploy:

```bash
bundle install
bin/rails assets:precompile assets:clean
bin/rails db:migrate db:seed
```

Set the following environment variables on the host:

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `RAILS_MASTER_KEY` | Value from `config/master.key` |
| `RAILS_MAX_THREADS` | (optional) Puma thread count, defaults to 5 |
