# Docker Dev Environment

## Using Xdebug

To be able to debug using xdebug, set `PHP_XDEBUG` to `true` in `.env`. If you have previously already build the images without xdebug enabled make sure to rebuild both workspace and php-fpm by running `docker-compose build php-fpm workspace`.

### PhpStorm

1. Create a server in `File | Settings | Languages & Frameworks | PHP | Servers`.
2. Make sure the server name matches to value `XDEBUG_SERVER_NAME` in the docker .env file.
3. Hostname and port should be set to wherever the application is being served from, by default `localhost:80`
4. Map the local project directory to the absolute working path within the container, by default `/app`.
5. Set a breakpoint in your source code and start listening for a debug connection.
6. Happy debugging.

### Visual Studio Code

1. Make sure you have the [PHP Debug extension](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug) installed.
2. Create a debug profile containing the correct path mapping. Do so by adding `"/app": "${workspaceFolder}"` to your `pathMappings`.
3. Optionally, set `hostname` to `0.0.0.0` to listen on IPv4 interfaces. Php Debug listens by default on IPv6 by default (if available) for some wierd reason.
4. Set a breakpoint in your source code and start listening for a debug connection.
5. Happy debugging.
