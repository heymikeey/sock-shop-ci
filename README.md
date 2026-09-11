# sock-shop-ci

This repository formerly hosted a polyrepo-in-one mirror of the [Sock Shop](https://github.com/microservices-demo/microservices-demo) microservices demo application, vendored as individual service directories for CI/CD experimentation.

**Each service has been split out into its own repository**, preserving full git history, so that every service can be built, versioned, and deployed independently.

Sock Shop is a reference e-commerce app ("the world's leading provider of fashionable footwear for cats") built as a set of independently deployable microservices, commonly used to demonstrate cloud-native, container, and Kubernetes tooling.

> **Note:** The upstream `microservices-demo` project has been archived/deprecated by its maintainers. Each service repository's README carries the original `DEPRECATED` notice, but the code remains a useful, realistic multi-language, multi-service target for building CI/CD pipelines.

## Services

| Service | Language / Stack | Purpose | Repository |
|---|---|---|---|
| `front-end` | Node.js | Web UI that aggregates and proxies calls to all backend services | [heymikeey/sockshop-front-end](https://github.com/heymikeey/sockshop-front-end) |
| `catalogue` | Go | Product catalogue (listing, details, search) | [heymikeey/sockshop-catalogue](https://github.com/heymikeey/sockshop-catalogue) |
| `carts` | Java (Spring Boot) | Shopping cart management | [heymikeey/sockshop-carts](https://github.com/heymikeey/sockshop-carts) |
| `orders` | Java (Spring Boot) | Order placement and history | [heymikeey/sockshop-orders](https://github.com/heymikeey/sockshop-orders) |
| `payment` | Go | Payment authorization | [heymikeey/sockshop-payment](https://github.com/heymikeey/sockshop-payment) |
| `shipping` | Java (Spring Boot) | Shipment creation | [heymikeey/sockshop-shipping](https://github.com/heymikeey/sockshop-shipping) |
| `queue-master` | Java (Spring Boot) | Consumes the shipping queue and simulates shipment processing | [heymikeey/sockshop-queue-master](https://github.com/heymikeey/sockshop-queue-master) |
| `user` | Go | User accounts, addresses, and payment cards | [heymikeey/sockshop-user](https://github.com/heymikeey/sockshop-user) |

Each service repository is self-contained, with its own `Dockerfile`/`docker-compose*.yml`, build tooling (Maven `pom.xml`, Go modules/`Makefile`, or `package.json`), API spec, tests, and GitHub Actions CI workflow (`.github/workflows/ci.yml`) that builds and pushes its Docker image to `ghcr.io/<owner>/<service>` on every push and semver tag (`v*.*.*`).

## Why split?

Splitting the monorepo into one repository per service (`sockshop-<service>`) allows:

- Independent versioning/tagging and release cadence per service
- CI pipelines scoped to a single service, without cross-service path filters
- Clear ownership boundaries and access control per repository

## License

Each service repository retains its original upstream `LICENSE` (Apache 2.0), inherited from the [microservices-demo](https://github.com/microservices-demo/microservices-demo) project by Weaveworks.
