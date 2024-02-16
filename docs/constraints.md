# X-based Constraints

## Constraints
SHACL-X supports the definition of constraints based on X.

### An Example of a X-based Constraint

#### Example data graph
``` turtle
@prefix ex: <http://example.org/ns#> .

ex:ValidCountry a ex:Country ;
	ex:germanLabel "Spanien"@de .
  
ex:InvalidCountry a ex:Country ;
	ex:germanLabel "Spain"@en .
```
#### Example shapes graph
=== "JavaScript"
    ``` turtle
    ex:LanguageExampleShape
        a sh:NodeShape ;
        sh:targetClass ex:Country ;
        sh:js [
            a sh:JSConstraint ;
            sh:message "Values are literals with German language tag." ;
            sh:jsLibrary [ sh:jsLibraryURL "http://example.org/js/germanLabel.js" ] ;
            sh:jsFunctionName "validateGermanLabel" ;
        ] .
    ```

=== "Python"
    ``` turtle
    ex:LanguageExampleShape
        a sh:NodeShape ;
        sh:targetClass ex:Country ;
        sh:py [
            a sh:PyConstraint ;
            sh:message "Values are literals with German language tag." ;
            sh:pyLibrary [ sh:pyLibraryURL "http://example.org/py/german_label.py" ] ;
            sh:pyFunctionName "validate_german_label" ;
        ] .
    ```

#### Example X function
=== "JavaScript"
    
    ``` js
    function validateGermanLabel($this) {
        let results = [];
        let p = TermFactory.namedNode("http://example.org/ns#germanLabel");
        let s = $data.find($this, p, null);
        for (let t = s.next(); t; t = s.next()) {
            let object = t.getObject();
            if (!object.isLiteral() || !object.getLanguage().startsWith("de")) {
                results.push({
                    value: object
                });
            }
        }
        return results;
    }
    ```
    
=== "Python"
    
    ``` py
    def validate_german_label(this):
        results = []
        p = py_tf.namedNode("http://example.org/ns#germanLabel")
        s = _data.find(this, p, None)

        while (t := s.next()) != None:
            object = t.getObject()
            if not object.isLiteral() or not object.getLanguage().startswith("de"):
                results.append({
                    "value": object
                })

        return results

    ```

#### Example validation results

=== "JavaScript"
    ``` turtle
    [   a           sh:ValidationReport ;
        sh:conforms false ;
        sh:result   [   a                             sh:ValidationResult ;
                        sh:focusNode                  ex:InvalidCountry ;
                        sh:resultMessage              "Values are literals with German language tag." ;
                        sh:resultSeverity             sh:Violation ;
                        sh:sourceConstraint           []  ;
                        sh:sourceConstraintComponent  sh:JSConstraintComponent ;
                        sh:sourceShape                ex:LanguageExampleShape ;
                        sh:value                      "Spain"@en
        ]
    ] .
    ```

=== "Python"

    ``` turtle
    [   a           sh:ValidationReport ;
        sh:conforms false ;
        sh:result   [   a                             sh:ValidationResult ;
                        sh:focusNode                  ex:InvalidCountry ;
                        sh:resultMessage              "Values are literals with German language tag." ;
                        sh:resultSeverity             sh:Violation ;
                        sh:sourceConstraint           []  ;
                        sh:sourceConstraintComponent  sh:PyConstraintComponent ;
                        sh:sourceShape                ex:LanguageExampleShape ;
                        sh:value                      "Spain"@en
        ]
    ] .

    ```

## Constraint Components

### An Example of a X-based Constraint Component

#### Example data graph

``` turtle
@prefix ex: <http://example.org/ns#> .

ex:ValidCountry a ex:Country ;
    ex:name "Italy" .

ex:InvalidCountry a ex:Country ;
    ex:name "Switzerland" .
```
#### Example shapes graph

``` turtle
ex:CountryExampleShape
    a sh:NodeShape ;
    sh:targetClass ex:Country ;
    sh:property [
        sh:path ex:name ;
        ex:maxLength 5 ;
    ] .

ex:MaxLengthConstraintComponent
    a sh:ConstraintComponent ;
	sh:parameter [
		sh:path ex:maxLength ;
		sh:datatype xsd:integer ;
	] ;
	sh:validator ex:hasMaxLength .
```

=== "JavaScript"
    ``` turtle
    ex:hasMaxLength
        a sh:JSValidator ;
        sh:message "Value has more than {$maxLength} characters" ;
        sh:jsLibrary [ sh:jsLibraryURL "http://example.org/js/hasMaxLength.js"^^xsd:anyURI ] ;
        sh:jsFunctionName "hasMaxLength" ;
        rdfs:comment """
            Note that $value and $maxLength are RDF nodes expressed in JavaScript Objects.
            Their string value is accessed via the .getLex() and .getUri() methods.
            The function returns true if no violation has been found.
            """ .
    ```

=== "Python"
    ``` turtle
    ex:hasMaxLength
        a sh:PyValidator ;
        sh:message "Value has more than {$maxLength} characters" ;
        sh:pyLibrary [ sh:pyLibraryURL "http://example.org/js/has_max_length.py"^^xsd:anyURI ] ;
        sh:pyFunctionName "has_max_length" ;
        rdfs:comment """
            Note that _value and _maxLength are RDF nodes expressed in Python Objects.
            Their string value is accessed via the .getLex() and .getUri() attributes.
            The function returns true if no violation has been found.
            """ .
    ```


#### Example X function

=== "JavaScript"
    ``` js
    function hasMaxLength($value, $maxLength) {
        if ($value.isLiteral()) {
            return $value.getLex().length <= $maxLength.getLex();
        } else if ($value.isURI()) {
            return $value.getUri().length <= $maxLength.getLex();
        } else { // Blank node
            return false;
        }
    }
    ```
=== "Python"
    ``` py
    def has_max_length(_value, _maxLength):
        if _value.isLiteral():
            return len(_value.getLex()) <= int(_maxLength.getLex())
        elif _value.isURI():
            return len(_value.getUri()) <= int(_maxLength.getLex())
        else:
            return False


    ```

#### Example validation results

``` turtle
[   a            sh:ValidationReport ;
    sh:conforms  false ;
    sh:result   [   a                               sh:ValidationResult ;
                    sh:focusNode                    ex:InvalidCountry ;
                    sh:resultMessage                "Value has more than 5 characters" ;
                    sh:resultPath                   ex:name ;
                    sh:resultSeverity               sh:Violation ;
                    sh:sourceConstraintComponent    ex:MaxLengthConstraintComponent ;
                    sh:sourceShape                  []  ;
                    sh:value                        "Switzerland"
    ]
] .
```