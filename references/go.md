# Go Project Analysis Rules

## Signature Detection
- `go.mod` exists

## Scan Steps

### 1. Read go.mod
Extract:
- `module` — Module path (e.g. `github.com/user/project`)
- `go` — Go version
- `require` — Direct dependencies. Key packages to identify:
  - `github.com/gin-gonic/gin`, `github.com/labstack/echo`, `github.com/gorilla/mux` → Web framework/router
  - `github.com/go-chi/chi` → Lightweight router
  - `gorm.io/gorm`, `github.com/jmoiron/sqlx` → ORM/database
  - `github.com/spf13/cobra`, `github.com/urfave/cli` → CLI tool
  - `github.com/grpc/grpc-go` → gRPC
  - `github.com/hashicorp/terraform` → Infrastructure tool
  - `go.mongodb.org/mongo-driver` → MongoDB
  - `github.com/redis/go-redis` → Redis
  - `github.com/gorilla/websocket` → WebSocket
  - `github.com/stretchr/testify` → Testing helpers
  - `google.golang.org/api` → Google Cloud APIs
  - `github.com/aws/aws-sdk-go-v2` → AWS SDK

### 2. Identify Project Type
| Clue | Type |
|---|---|
| `func main()` in root `.go` | CLI tool or server binary |
| Only `.go` files in root, no `main` | Library |
| `cmd/` directory with subdirectories | Multi-binary project |
| `k8s.io/` or `sigs.k8s.io/` dependencies | Kubernetes operator/controller |
| `api/` or `proto/` directory | gRPC/API server |
| `Dockerfile` + `deploy/` or `charts/` | Deployable service |

### 3. Map Directory Structure
| Directory | Common Role |
|---|---|
| `cmd/` | Application entry points (one subdir per binary) |
| `internal/` | Private application code (cannot be imported externally) |
| `pkg/` | Public library code (can be imported externally) |
| `api/` or `proto/` | API definitions (protobuf, OpenAPI) |
| `configs/` or `config/` | Configuration files/templates |
| `scripts/` | Build/deploy/utility scripts |
| `migrations/` | Database migrations |
| `test/` or `testdata/` | Test fixtures and data |
| `docs/` | Documentation |
| `web/` or `ui/` | Frontend assets (if Go + frontend) |
| `vendor/` | Vendored dependencies (if not using modules) |

### 4. Find Entry Points
```bash
grep "func main()" in root *.go
glob cmd/*/main.go
```
Read `main.go` for server startup, dependency wiring, router setup.

### 5. Read Key Files (Priority Order)
1. `main.go` or `cmd/<name>/main.go` — 50 lines
2. `internal/<name>/` — Core logic directory
3. `.env.example` or `config.yaml` — Configuration schema

### 6. Identify Architecture Patterns
Go idioms to look for:
- **Interface-driven**: `//go:generate mockery` or `type Interface interface {`
- **Repository pattern**: `internal/repository/` with interface + impl
- **Clean/hexagonal architecture**: `internal/{domain,usecase,adapter}/`
- **Wire/Dependency injection**: `wire.go` or `internal/wire/` (Google Wire)
- **Worker/Consumer**: Kafka/NATS/RabbitMQ patterns in `internal/worker/`

### 7. Build & Test Commands
- Build: `go build ./...` or `go build -o bin/<name> ./cmd/<name>`
- Run: `go run ./cmd/<name>` or `go run .`
- Test: `go test ./...`
- Lint: `golangci-lint run` (check for `.golangci.yml`)
- Format: `go fmt ./...`
- Generate mocks: `go generate ./...`
