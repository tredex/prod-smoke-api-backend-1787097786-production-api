# api



## Quick Start

### Development

```bash
go mod download
go run cmd/app/main.go
```

Visit http://localhost:8080

### Production

```bash
go build -o server cmd/app/main.go
./server
```

### Docker

```bash
docker build -t api .
docker run -p 8080:8080 api
```

## API Endpoints

- `GET /` - Welcome message
- `GET /health` - Health check

## Environment Variables

- `PORT` - Server port (default: 8080)
- `ENV` - Environment (development/production)
- `VERSION` - Application version
- `DATABASE_URL` - Injected when connected to a Stelm Database component (empty DB until you migrate)

## Database and migrations

Stelm does not run migrations for you. Apply schema on startup or via your own CI:

```go
db, err := sql.Open("postgres", os.Getenv("DATABASE_URL"))
if err != nil {
    log.Fatal(err)
}
_, err = db.Exec(`CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    email TEXT NOT NULL UNIQUE
)`)
```

Or use [golang-migrate](https://github.com/golang-migrate/migrate) in a workflow step you add.

Guide: https://app.stelm.dev/app/docs/database-migrations

## CI/CD

This project includes Gitea Actions workflow for automated builds and deployments.

The workflow will:
1. Build Docker image
2. Push to container registry
3. Trigger deployment via API

## License

MIT
