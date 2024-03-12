# **SHACL** e**X**tended

## Overview

**SHACL** e**X**tended (SHACL-X) is a fork of the open source implementation of SHACL, [TopBraid SHACL API](https://github.com/TopQuadrant/shacl), based on Java and Apache Jena.

Following the deprecation of the Nashorn JavaScript engine, TopBraid SHACL API dropped the support of the SHACL JavaScript Extensions (SHACL-JS) features to avoid Nashorn-related issues. Beginning from TopBraid SHACL API version [1.4.0](https://github.com/TopQuadrant/shacl/releases/tag/v1.4.0), these features are no longer available.

SHACL-X reintroduces support for the SHACL JavaScript Extensions by replacing the Nashorn engine with GraalVM and the Polyglot APIs. The codebase of SHACL-X corresponds to TopBraid SHACL API version [1.3.2](https://github.com/TopQuadrant/shacl/releases/tag/shacl-1.3.2).

## X &rarr; supported languages
The `X` in SHACL-X can also be read as a placeholder for the languages supported by the tool. Currently, `JavaScript` and `Python` are the supported ones. In this documentation, anytime you encounter an `X` occurrence you can replace it by any of the supported languages. In case some functionalities are only available for a specific language, it will be explictly stated (i.e. SHACL-JS, SHACL-Py).

## Usage

The tool is released to the [GitHub Packages registry](https://github.com/SHACL-X/shacl-x/packages/) as Maven artifact and Docker image.

### Application dependency

=== "Maven"
    ``` xml
    <dependency>
        <groupId>io.github.shacl-x</groupId>
        <artifactId>shacl-x</artifactId>
        <version>1.4.0</version>
    </dependency>
    ```

### Docker

The docker image accepts one parameter as input (**command**):

- `validate`
- `infer`

After pulling the latest version as follows:
```
docker pull ghcr.io/shacl-x/shacl-x:latest
```

one can run the tool:
```
docker run --rm \
    -v ${PWD}/data:/data ghcr.io/shacl-x/shacl-x COMMAND \
    -datafile /data/data.ttl \
    -shapesfile /data/shapes.ttl
```

Please note that

- `COMMAND` must be one between the defined ones
- a `${PWD}/data` must exist, as well as `data.ttl` and `shapes.ttl` files
- if the shapes contain SHACL-X features that use the network, a Docker network configuration must also be done (e.g. create a docker network called `shacl` and pass `--network="shacl"` to the docker run command)

### Command line (not recommended)

Download the latest release from:

[https://github.com/SHACL-X/shacl-x/packages?ecosystem=maven](https://github.com/SHACL-X/shacl-x/packages?ecosystem=maven)

The binary distribution (shacl-x-VERSION-bin.zip) can be found in the Assets section of the release page.

Two command line utilities are included: shaclvalidate (performs constraint validation) and shaclinfer (performs SHACL rule inferencing). To use them, follow the steps as explained in the [TopBraid SHACL API readme](https://github.com/topquadrant/shacl?tab=readme-ov-file#command-line-usage).



## License

The software is released under the Apache 2.0 License.