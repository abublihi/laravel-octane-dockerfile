# Laravel Octane Dockerfile
<a href="/LICENSE"><img alt="License" src="https://img.shields.io/github/license/abublihi/laravel-octane-dockerfile"></a>
<a href="https://github.com/abublihi/laravel-octane-dockerfile/releases"><img alt="GitHub release (latest by date)" src="https://img.shields.io/github/v/release/abublihi/laravel-octane-dockerfile"></a>
<a href="https://github.com/abublihi/laravel-octane-dockerfile/pulls"><img alt="GitHub closed pull requests" src="https://img.shields.io/github/issues-pr-closed/abublihi/laravel-octane-dockerfile"></a>
<a href="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/tests.yml"><img alt="GitHub Workflow Status" src="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/roadrunner-test.yml/badge.svg"></a>
<a href="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/tests.yml"><img alt="GitHub Workflow Status" src="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/swoole-test.yml/badge.svg"></a>
<a href="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/tests.yml"><img alt="GitHub Workflow Status" src="https://github.com/abublihi/laravel-octane-dockerfile/actions/workflows/frankenphp-test.yml/badge.svg"></a>


Dockerfiles for [Laravel Octane](https://github.com/laravel/octane)
powered web services and microservices.

The Docker configuration provides the following setup:

- PHP 8.2 and 8.3 official Debian-based and Alpine-based images
- Preconfigured JIT compiler and OPcache

## Container modes

You can run the Docker container in different modes:

| Mode                  | `CONTAINER_MODE` | HTTP server         |
| --------------------- | ---------------- | ------------------- |
| HTTP Server (default) | `http`           | FrankenPHP / Swoole / RoadRunner |
| Horizon               | `horizon`        | -                   |
| Worker                | `worker`         | -                   |

## Usage

### Building Docker image
1. Clone this repository:
```
git clone --depth 1 git@github.com:abublihi/laravel-octane-dockerfile.git
```
2. Copy cloned directory content including `deployment` directory, `Dockerfile`, and `.dockerignore` into your Octane powered Laravel project
3. Change the directory to your Laravel project
4. Build your image:
```
docker build -t <image-name>:<tag> -f <your-octane-driver>.Dockerfile .
```
### Running Docker container

```bash
# HTTP mode
docker run -p <port>:8000 --rm <image-name>:<tag>

# Horizon mode
docker run -e CONTAINER_MODE=horizon --rm <image-name>:<tag>

# Worker mode
docker run -e CONTAINER_MODE=worker -e WORKER_COMMAND="php /var/www/html/artisan foo:bar" --rm <image-name>:<tag>

# Running a single command
docker run --rm <image-name>:<tag> php artisan about
```

## Configuration

### Recommended `Swoole` options in `octane.php`

```php
// config/octane.php

return [
    'swoole' => [
        'options' => [
            'http_compression' => true,
            'http_compression_level' => 6, // 1 - 9
            'compression_min_length' => 20,
            'package_max_length' => 20 * 1024 * 1024, // 20MB
            'open_http2_protocol' => true,
            'document_root' => public_path(),
            'enable_static_handler' => true,
        ]
    ]
];
```

## Notes

- Laravel Octane logs request information only in the `local` environment.
- Please be aware of `.dockerignore` content


## Contributing

Thank you for considering contributing! If you find an issue, or have a better way to do something, feel free to open an
issue, or a PR.

## Credits
- [SMortexa](https://github.com/smortexa)
- [All contributors](https://github.com/abublihi/laravel-octane-dockerfile/graphs/contributors)

## License

This repository is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
