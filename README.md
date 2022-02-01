# SecureApps@CI
The [SecureApps@CI](https://gitlab.com/secureapps-ci) primary objective is to create a system for integrating and orchestrating arbitrary security tests into CI/CD pipelines. The developed code and pipeline samples are available on [gitlab.com/secureapps-ci](https://gitlab.com/secureapps-ci).

This work is part of my Master's dissertation that I defended on [February 8, 2021](https://www.ua.pt/pt/noticias/10/65849) at the University of Aveiro, Portugal, Europe (WEST). The dissertation is entitled [Container security in CI / CD pipelines](https://ria.ua.pt/bitstream/10773/31292/1/Documento_Andr%c3%a9_Br%c3%a1s.pdf) and is available online at [RIA](http://hdl.handle.net/10773/31292), the Institutional repository of University of Aveiro (UA).

## The API server

### Overview
API specification is available at [SwaggerHub](https://app.swaggerhub.com/apis/ema.rainho/secureapps-ci/v1).    
API design and documentation follows OpenAPI specification and uses the [Connexion](https://github.com/zalando/connexion) library on top of Flask.        
