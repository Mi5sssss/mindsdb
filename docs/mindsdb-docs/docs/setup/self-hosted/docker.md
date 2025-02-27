# Setup for Docker

## Install Docker

Install [Docker](https://docs.docker.com/install) on your machine. To make sure Docker is successfully installed on your machine, run:

```bash
docker run hello-world
```

You should see the `Hello from Docker!` message displayed. If not, check [Docker's Get Started](https://www.docker.com/get-started) documentation.

???+ warning "Docker for Mac users, RAM Allocation issues"

    By default, Docker for Mac allocates **2.00GB of RAM**. This is insufficient for deploying MindsDB with docker. We recommend increasing the default RAM limit to **4.00GB**. Please refer to Docker's [Docker Desktop for Mac user manual](https://docs.docker.com/desktop/mac/#resources) for more information on how to increase the allocated memory.

## Start MindsDB

Run the below command to start MindsDB in Docker:

```bash
docker run -p 47334:47334 -p 47335:47335 mindsdb/mindsdb
```

The `-p` flag allows access to MindsDB by two different methods:

- `#!bash -p 47334:47334` - Publishes port 47334 to access MindsDB GUI and HTTP API.
- `#!bash -p 47335:47335` - Publishes port 47335 to access MindsDB MySQL API.

## Optional Additional Configuration

### Default Configuration

The default configuration for MindsDB's Docker image is represented as a JSON block:

```json

{
 "config_version":"1.4",
 "storage_dir": "/root/mdb_storage",
 "log":
  { "level": { "console": "ERROR", "file": "WARNING", "db": "WARNING" } },
 "debug": false,
 "integrations": {},
 "api":
  {
   "http": { "host": "0.0.0.0", "port": "47334" },
   "mysql": { "host": "0.0.0.0", "password": "", "port": "47335", "user": "mindsdb", "database": "mindsdb", "ssl": true },
   "mongodb": { "host": "0.0.0.0", "port": "47336", "database": "mindsdb" }
  }
}

```

To override the default configuration, you can provide values via the `MDB_CONFIG_CONTENT` environment variable.


### Example

```bash
docker run -e MDB_CONFIG_CONTENT='{"api":{"http": {"host": "0.0.0.0","port": "8080"}}}' mindsdb/mindsdb
```
### Known Issues

??? warning "MKL Issues"
    If you experience any issues related to MKL or if your training process does not complete, please add the `#!bash MKL_SERVICE_FORCE_INTEL` environment variable:

	```bash
    docker run -e MKL_SERVICE_FORCE_INTEL=1 -p 47334:47334 -p 47335:47335 mindsdb/mindsdb
	```
