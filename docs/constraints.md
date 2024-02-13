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
