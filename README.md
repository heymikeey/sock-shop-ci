# sock-shop-ci

A polyrepo-in-one mirror of the [Sock Shop](https://github.com/microservices-demo/microservices-demo) microservices demo application, vendored here as individual service directories for CI/CD experimentation.

Sock Shop is a reference e-commerce app ("the world's leading provider of fashionable footwear for cats") built as a set of independently deployable microservices, commonly used to demonstrate cloud-native, container, and Kubernetes tooling.

> **Note:** The upstream `microservices-demo` project has been archived/deprecated by its maintainers. Each vendored service README below carries the original `DEPRECATED` notice, but the code remains a useful, realistic multi-language, multi-service target for building CI/CD pipelines.

## Services

| Service | Language / Stack | Purpose |
|---|---|---|
| [`front-end`](./front-end) | Node.js | Web UI that aggregates and proxies calls to all backend services |
| [`catalogue`](./catalogue) | Go | Product catalogue (listing, details, search) |
| [`carts`](./carts) | Java (Spring Boot) | Shopping cart management |
| [`orders`](./orders) | Java (Spring Boot) | Order placement and history |
| [`payment`](./payment) | Go | Payment authorization |
| [`shipping`](./shipping) | Java (Spring Boot) | Shipment creation |
| [`queue-master`](./queue-master) | Java (Spring Boot) | Consumes the shipping queue and simulates shipment processing |
| [`user`](./user) | Go | User accounts, addresses, and payment cards |

Each service directory is self-contained, with its own `Dockerfile`/`docker-compose*.yml`, build tooling (Maven `pom.xml`, Go modules/`Makefile`, or `package.json`), API spec, and tests. See each service's own `README.md` for build and run instructions specific to that service.

## Repository Layout

```
.
├── carts/         # Java - shopping cart service
├── catalogue/      # Go - product catalogue service
├── front-end/      # Node.js - web front end
├── orders/         # Java - order service
├── payment/        # Go - payment service
├── queue-master/   # Java - shipping queue consumer
├── shipping/       # Java - shipping service
└── user/           # Go - user/account service
```

## Building & Running Services

Most services support Docker-based builds via their own `Dockerfile` and `docker-compose*.yml` files, e.g.:

```bash
cd carts
docker build -t carts .
```

Java services (`carts`, `orders`, `shipping`, `queue-master`) use Maven:

```bash
cd carts
mvn -DskipTests package
```

Go services (`catalogue`, `payment`, `user`) use their `Makefile`/vendored dependencies:

```bash
cd catalogue
make build
```

The Node.js `front-end` uses Yarn/npm:

```bash
cd front-end
npm install
npm start
```

## CI

Each service historically shipped its own Travis CI (`.travis.yml`) configuration for building, testing, and publishing Docker images independently. This repository consolidates those services to support experimenting with unified, repo-wide CI/CD pipelines across the whole application.

Services are being migrated one at a time from Travis CI to GitHub Actions:

| Service | CI |
|---|---|
| `carts` | [GitHub Actions](./.github/workflows/carts.yml) — builds with Maven and pushes the Docker image to GitHub Container Registry (GHCR) |
| all others | Travis CI (`.travis.yml`, legacy) |

The `carts` workflow (`.github/workflows/carts.yml`) triggers on pushes/PRs touching `carts/**`, builds the service with Maven, then builds and pushes a Docker image to `ghcr.io/<owner>/carts` (tagged by commit SHA, branch, and `latest` on the default branch) using the built-in `GITHUB_TOKEN` — no additional secrets required.

## License

Each service directory retains its original upstream `LICENSE` (Apache 2.0), inherited from the [microservices-demo](https://github.com/microservices-demo/microservices-demo) project by Weaveworks.