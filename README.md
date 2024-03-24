# PHP

This project provides a comprehensive solution for building, managing, and deploying Dockerized PHP services across multiple PHP versions, catering to the diverse needs of modern web applications. It leverages a structured approach with a `Makefile` to automate the creation of Docker images for various PHP-based services such as CLI, Composer, Cron, and FPM, each tailored to specific versions of PHP.

Designed to streamline development workflows and CI/CD pipelines, the project simplifies the process of image management, enabling developers to easily build, clean, and push images to a Docker registry. With its focus on flexibility and ease of use, this project is an essential tool for developers looking to enhance their PHP service deployments in a Dockerized environment.

## Usages

### Build

```bash
make build
```

```bash
PHP_VERSIONS=8.3 make build
```

### Push

```bash
make push
```

```bash
PHP_VERSIONS=8.3 make push
```

### Clean

```bash
make clean
```

```bash
PHP_VERSIONS=8.3 make clean
```

### Build & Push

```bash
make all
```

```bash
PHP_VERSIONS=8.3 make all
```
