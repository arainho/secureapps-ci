# SecureApps@CI
[SecureApps@CI](https://gitlab.com/secureapps-ci) is a system to enable Application Security integration on CI/CD Pipelines.

## The API server

### Overview
API specification is available at [SwaggerHub](https://app.swaggerhub.com/apis/ema.rainho/secureapps-ci/v1).    
API design and documentation follows OpenAPI specification and uses the [Connexion](https://github.com/zalando/connexion) library on top of Flask.        

### Requirements
Python 3.5.2+

### Usage
To run the server, please execute the following from the root directory:

```
pip3 install -r requirements.txt
python3 -m swagger_server
```

and open your browser to here:

```
https://localhost:5555/ema.rainho/secureapps-ci/v1/ui/
```

Your Swagger definition lives here:

```
https://localhost:5555/ema.rainho/secureapps-ci/v1/swagger.json
```

To launch the integration tests, use tox:
```
sudo pip install tox
tox
```

### Running with Docker

First, build the docker image from the root directory
```bash
docker build -t swagger_server:v1.0 .
```

Then run the server on a Docker container by executing the following:
```bash
read -s api_key
docker run -e "API_KEY=$api_key" -e "PYTHON_GITLAB_CFG=./private_config/python-gitlab.cfg" -p 5555:5555 swagger_server:v1.0
```

_``Notice that 'api_key' refers to SecureApps@CI and 'PYTHON_GITLAB_CFG' to the configuration file path of GitLab instance``_

You can also share the host `private_config` folder with the container, 
```bash
docker run -v $PWD/private_config:/usr/src/app/private_config -p 5555:5555 swagger_server:v1.0
```

_``This scenario assumes that private_config folder has secrets.py, variables.py and id_development_key files. 
Examples of these files are in 'samples' folder``_

### Validate your swagger definition

The Swagger Validator Badge, validates a Swagger/OpenAPI 2.0 or an OpenAPI 3.0 definition.
https://validator.swagger.io/

```bash
pushd swagger_server/swagger/
curl -X POST --data-binary @dockersec-tool.yaml -H  "Accept: application/yaml" -H 'Content-Type:application/yaml' https://validator.swagger.io/validator/debug
popd
```

### Test upload of yaml definition
```bash
read -s api_key
curl --insecure "https://localhost:5555/ema.rainho/secureapps-ci/v1/health"
curl --location --request POST "https://localhost:5555/ema.rainho/secureapps-ci/v1/analysis/create" \
                            --header "Content-Type: application/octet-stream" \
                            --header "Accept: application/json" \
                            --header "X-API-KEY: $api_key" \
                            --data-binary "@./samples/Security-Analysis.gitlab-ci.yml" \
                            --insecure
```

### security linter 
We use a security linter called bandit, integrated with our version control.
A tool called [pre-commit](https://pre-commit.com) runs hooks on every commit to automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements.

There is a pre-commit configuration in a file named `.pre-commit-config.yaml`

The requirements for this are bandit and pre-commit, to install them run:
```bash
python3 -m pip install -U --user bandit pre-commit
```

run bandit locally before committing to repository
```bash
bandit \
       -v \
       -lll \
       -iii \
       -f screen \
       --recursive \
       --aggregate file \
       $PWD/swagger_server/ $PWD/api_backend/ $PWD/docker_connector/ $PWD/*.py
```

