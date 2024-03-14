# Representing X in RDF
The JavaScript-based extension mechanism for SHACL ([SHACL-JS](https://www.w3.org/TR/shacl-js/#js-rdf)) defines some RDF terms that can be used to represent JavaScript code and libraries as part of the shapes graph. SHACL-X extends that syntax by adding new terms for the new supported languages.

The extended vocabulary includes the classes `sh:`X`Executable`, where X = JS | Py (e.g. `sh:PyExecutable`). SHACL instances of this class are called **X executables**.

## FunctionName
Every X executable must have exactly one value for the property `sh:`x`FunctionName`. The value is a `literal` with datatype `xsd:string`.

## x Library
Every X executable must have at least one value for the property `sh:`x`Library`. The values of the property are IRIs or blank nodes. In both cases, they must be a well formed SHACL instance of the class `sh:`X`Library`.

## X Library
The class `sh:`X`Library` is used to declare **X libraries**. An X library is a pointer to zero or more X files that are executed before the evaluation of an X executable. Libraries depdenency is allowed, by declaring further `sh:`x`Library` triples. The values of the property `sh:`x`LibraryURL` are literals with datatype `xsd:anyURI`.