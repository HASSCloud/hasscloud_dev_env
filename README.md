# HASSCloud Dev Environment

## Quickstart

Load openstack config

```#!bash
. openstack-rc.sh

docker-compose up
```

To customise the docker-compose configuration it is also possible to
export the env var COMPOSE\_FILE. e.g.:

```#!bash
docker-compose -f file1.yaml -f file2.yaml
```

becomes:

```#!bash
COMPOSE_FILE=file1.yaml:file2.yaml docker-compose up
```

or just export `COMPOSE_FILE`.

## Working onthe UI

UI is fairly simple. just stop the service and run your own. Everything
is connected through localhost.

```#!bash
docker-compose stop workspace-ui
```

Clone the workspace-ui repository:

```#!bash
cd src
git clone git@github.com:hasscloud/workspace-ui.git
cd ..
```

### Using docker for dev

start up your own instance with code editing

```#!bash
docker-compose -f docker-compose.yaml -f src/docker-compose-workspace-ui.yaml up workspace-ui
```

### Start locally

Workspace-ui is a pure browser application and doesn\'t need any backend
service itself. After stoping the container, it is possible to start up
locally.

Init yarn project:

```#!bash
yarn
```

Start dev server:

```#!bash
yarn start
```

## Working on Workspace

Clone the Workpsace repository:

```#!bash
cd src
git clone git@github.com:hasscloud/workspace.git
cd ..
```

This set up will only mount the src/workspace/src/workspace folder into
the container. If you make changes to files outside this folder, you\'ll
have to rebuild the container.

```#!bash
docker-compose -f docker-compose.yaml -f src/docker-compose-workspace.yaml build workspace

# stop current running workspace
docker-compose stop workspace

# start up workspace container again
docker-compose -f docker-compose.yaml -f src/docker-compose-workspace.yaml up workspace
```

### Debugging Workspace

```#!bash
# it may be necessary to remove any existing containers
docker-compose rm workspace

# run up container and drop into an interactive shell
docker-compose -f docker-compose.yaml -f src/docker-compose-workspace.yaml run --rm --service-ports --name workspace workspace bash

# Inside container start up workspace app. Doing it this way you'll be able to use pdb breakpoints.
# paster serve --reload /code/workspace/development.ini
```

## JupyterHub Notes

This environment uses docker to spawn a notebook server. It may be
necessary to pull the configured notebook image before JupyterHub is
able to spawn a new notebook. See `config/jupyterhub/profiles.yaml` for
correct image names and tags.

```#!bash
docker pull hub.bccvl.org.au/jupyter/scipy-notebook:latest
```
