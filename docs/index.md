# **SHACL** e**X**tended

## Overview

**SHACL** e**X**tended (SHACL-X) is a fork of the open source implementation of SHACL, [TopBraid SHACL API](https://github.com/TopQuadrant/shacl), based on Java and Apache Jena.

Following the deprecation of the Nashorn JavaScript engine, TopBraid SHACL API dropped the support of the SHACL JavaScript Extensions (SHACL-JS) features to avoid Nashorn-related issues. Beginning from TopBraid SHACL API version [1.4.0](https://github.com/TopQuadrant/shacl/releases/tag/v1.4.0), these features are no longer available.

SHACL-X reintroduces support for the SHACL JavaScript Extensions by replacing the Nashorn engine with GraalVM and the Polyglot APIs. The codebase of SHACL-X corresponds to TopBraid SHACL API version [1.3.2](https://github.com/TopQuadrant/shacl/releases/tag/shacl-1.3.2).

## X &rarr; supported languages
The `X` in SHACL-X can also be read as a placeholder for the languages supported by the tool. Currently, `JavaScript` and `Python` are the supported ones. In this documentation, anytime you encounter an `X` occurrence you can replace it by any of the supported languages. In case some functionalities are only available for a specific language, it will be explictly stated (i.e. SHACL-JS, SHACL-Py).

## License

The software is released under the Apache 2.0 License.