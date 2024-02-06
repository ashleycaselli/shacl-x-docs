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
#### IRIs
#### Blank Nodes


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