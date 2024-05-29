# X-based Functions

This section shows how to define `XFunctions`. The defined function can be used within a SPARQL query.

#### Example data graph
``` turtle
@prefix ex: <http://example.org/ns#> .

ex:JohnDoe a ex:Person ;
    ex:name "John" ;
    ex:surname "Doe" .
```

#### Example shapes graph
=== "JavaScript"
    ``` turtle
    ex:FullNameShape
        a sh:NodeShape ;
        sh:targetClass ex:Person ;
        sh:rule [
            a sh:SPARQLRule ;
            sh:construct """
                PREFIX ex: <http://example.org/ns#>
                CONSTRUCT {
                    $this ex:fullName ?fullName .
                }
                WHERE {
                    $this ex:name ?name .
                    $this ex:surname ?surname .
                    BIND(ex:concat(?name, ?surname) AS ?fullName)
                }
            """
        ] .

    ex:concat
        a sh:JSFunction ;
        sh:parameter [
            sh:path ex:arg1 ;
            sh:datatype xsd:integer ;
        ] ;
        sh:parameter [
            sh:path ex:arg2 ;
            sh:datatype xsd:integer ;
        ] ;
        sh:returnType xsd:integer ;
        sh:jsLibrary [ sh:jsLibraryURL "http://shacl-x-web/utils.js"^^xsd:anyURI ] ;
        sh:jsFunctionName "concat" .
    ```

=== "Python"
    ``` turtle
    ex:FullNameShape
        a sh:NodeShape ;
        sh:targetClass ex:Person ;
        sh:rule [
            a sh:SPARQLRule ;
            sh:construct """
                PREFIX ex: <http://example.org/ns#>
                CONSTRUCT {
                    $this ex:fullName ?fullName .
                }
                WHERE {
                    $this ex:name ?name .
                    $this ex:surname ?surname .
                    BIND(ex:concat(?name, ?surname) AS ?fullName)
                }
            """
        ] .

    ex:concat
        a sh:PyFunction ;
        sh:parameter [
            sh:path ex:arg1 ;
            sh:datatype xsd:integer ;
        ] ;
        sh:parameter [
            sh:path ex:arg2 ;
            sh:datatype xsd:integer ;
        ] ;
        sh:returnType xsd:integer ;
        sh:pyLibrary [ sh:pyLibraryURL "http://shacl-x-web/utils.py"^^xsd:anyURI ] ;
        sh:pyFunctionName "concat" .
    ```

#### Example X function
=== "JavaScript"
    ``` js
    function concat($arg1, $arg2) {
        return $arg1.getLex() + " " + $arg2.getLex();
    }
    ```
=== "Python"
    ``` py
    def concat(_arg1, _arg2):
        return _arg1.getLex() + " " + _arg2.getLex()
    ```

#### Example inferred triples
``` turtle
ex:JohnDoe ex:fullName "John Doe" .
```