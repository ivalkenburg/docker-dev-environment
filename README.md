# Docker Dev Environment

## Debugging using PhpStorm

1. Create a server in `File | Settings | Languages & Frameworks | PHP | Servers`.
2. Make sure the server name matches to value `CONTAINER_PREFIX` in the docker .env file.
3. Hostname and port should be set to wherever the application is being served from. Most likely `localhost:80`
4. Map the local project directory to the absolute path `/app`.
5. Set a breakpoint in your source code and start listening for a debug connection.
6. Happy debugging.

## Debugging using Visual Studio Code

1. Make sure you have the PHP Debug extension installed.
2. Create a debug profile containing the correct path mapping: `"app": "${workspaceFolder}"`.
3. Optionally, set listening hostname to `0.0.0.0` to listen on IPv4 interfaces. Php Debug listens by default on IPv6 by default.
4. Set a breakpoint in your source code and start listening for a debug connection.
5. Happy debugging.
