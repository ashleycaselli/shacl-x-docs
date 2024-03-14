# X API for RDF

## RDF Terms

### Factory Object

During the execution of the custom code, the SHACL engine provides a Factory Object that can be used to create `X` Objects for RDF terms. The Object provides the following three functions:

- `literal(lex, languageOrDatatype)`
- `namedNode(uri)`  
- `blankNode(id)` or `blankNode()`

The RDF terms objects can be created using the following Factory Object:

=== "JavaScript"
    
    ```
    TermFactory
    ```
    
=== "Python"
    
    ```
    py_tf
    ```

### Terms

| Node Kind     | Factory Function                  | Test Function     |
| ------------- | --------------------------------- | ----------------- |
| Literals      | `literal(lex, languageOrDatatype)`| `isLiteral()`     |
| IRIs          | `namedNode(uri)`                  | `isURI()`         |
| Blank Nodes   | `blankNode(id)` or `blankNode()`  | `isBlankNode()`   |

#### Literals
A literal is represented by an X `Literal` Object where `isLiteral()` returns `true` while `isURI()` and `isBlankNode()` return `false`. The lexical form of the literal is accessible through `getLex()`, e.g. `"Switzerland"` for the RDF node `"Switzerland"^^xsd:string`. The language value is accessible through `getLanguage()` and is a lowercase BCP-47 string (for example, "en", "de"), or an empty string if the literal has no language. The literal datatype is accessible through `getDatatype()`, and is a `NamedNode` representing the datatype IRI of the literal. Note that this datatype is never undefined, and language-tagged strings have `rdf:langString` as their datatype.

#### IRIs
An IRI is represented by an X `NamedNode` Object where `isURI()` returns `true` while `isBlankNode()` and `isLiteral()` return `false`. The value of IRI string is accessible through the `getUri()`.

#### Blank Nodes
A blank node is represented by an X `BlankNode` Object where `isBlankNode()` returns `true` while `isURI()` and `isLiteral()` return `false`. The blank node identifier is a string accessible through `getId()`. That string must be consistent for the same RDF node for the duration of a SHACL validation and processing of validation results.

### Example: Term creation using the Factory Object

=== "JavaScript"
    ```js
    // Literals
    
    // when the languageOrDatatype is a string it is considered as the language
    TermFactory.literal("Switzerland", "en")    // "Switzerland"@en
    TermFactory.literal("Schweiz", "de")        // "Schweiz"@de
    
    // when the languageOrDatatype is a NamedNode it is considered as the dataype
    let intType = TermFactory.namedNode("http://www.w3.org/2001/XMLSchema#integer")
    TermFactory.literal(10, intType)            // "10"^^http://www.w3.org/2001/XMLSchema#integer
    
    let stringType = TermFactory.namedNode("http://www.w3.org/2001/XMLSchema#string")
    TermFactory.literal(10, stringType)         // "10"

    // IRIs
    TermFactory.namedNode("http://example.org/ns#germanLabel")

    // Blank Nodes
    TermFactory.blankNode()                     // a random id will be generated
    TermFactory.blankNode("1298eaa4-61b3")
    ```
=== "Python"
    ```py
    # Literals
    
    # when the languageOrDatatype is a string it is considered as the language
    py_tf.literal("Switzerland", "en")    # "Switzerland"@en
    py_tf.literal("Schweiz", "de")        # "Schweiz"@de
    
    # when the languageOrDatatype is a NamedNode it is considered as the dataype
    int_type = py_tf.namedNode("http://www.w3.org/2001/XMLSchema#integer")
    py_tf.literal(10, int_type)            # "10"^^http://www.w3.org/2001/XMLSchema#integer
    
    string_type = py_tf.namedNode("http://www.w3.org/2001/XMLSchema#string")
    py_tf.literal(10, string_type)         # "10"

    # IRIs
    py_tf.namedNode("http://example.org/ns#germanLabel")

    # Blank Nodes
    py_tf.blankNode()                     # a random id will be generated
    py_tf.blankNode("1298eaa4-61b3")
    ```


## Triples
An RDF triple is represented by a `Triple` Object.

| Attribute     | Function          |
| ------------- | ----------------- |
| subject       | `getSubject()`    |
| predicate     | `getPredicate()`  |
| object        | `getObject()`     |


## Graphs

### Accessing the Data Graph
During a validation process, a variable points at the `Graph` Object representing the SHACL data graph.

=== "JavaScript"
    
    ```
    $data
    ```

=== "Python"
    
    ```
    _data
    ```

### Accessing the Shapes Graph
During a validation process, a variable points at the `Graph` Object representing the SHACL shapes graph.

=== "JavaScript"
    
    ```
    $shapes
    ```

=== "Python"
    
    ```
    _shapes
    ```

### Example

=== "JavaScript"
    
    ```
    ```

=== "Python"
    
    ```
    ```