# Docker Dev Environment

## Using Xdebug

To able to debug using xdebug, set `PHP_XDEBUG` to `true` in `.env`. If you have previously already build the images make sure to rebuild both nginx and php-fpm by running `docker-compose build nginx php-fpm`.

### Debugging using PhpStorm

1. Create a server in `File | Settings | Languages & Frameworks | PHP | Servers`.
2. Make sure the server name matches to value `XDEBUG_SERVER_NAME` in the docker .env file.
3. Hostname and port should be set to wherever the application is being served from, by default `localhost:80`
4. Map the local project directory to the absolute working path within the container, by default `/app`.
5. Set a breakpoint in your source code and start listening for a debug connection.
6. Happy debugging.

### Debugging using Visual Studio Code

1. Make sure you have the [PHP Debug extension](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug) installed.
2. Create a debug profile containing the correct path mapping: `"app": "${workspaceFolder}"`.
3. Optionally, set listening hostname to `0.0.0.0` to listen on IPv4 interfaces. Php Debug listens by default on IPv6 by default (if available) for some wierd reason.
4. Set a breakpoint in your source code and start listening for a debug connection.
5. Happy debugging.
