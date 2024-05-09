# X Rules

SHACL-X allows to define new rules types that are X-based.


| Rule Type IRI     | Key Property          | Other Properties  |
| ----------------- | --------------------- | ----------------- |
| `sh:JSRule`       | `sh:jsFunctionName`   | `sh:jsLibrary`    |
| `sh:PyRule`       | `sh:pyFunctionName`   | `sh:pyLibrary`    |

## An Example of an X rule

The following example illustrates the use of a X rule to compute the area of a rectangle, by multiplying width and height.

#### Example data graph

``` turtle
ex:ExampleRectangle
	a ex:Rectangle ;
	ex:width 7 ;
	ex:height 8 .
```

#### Example shapes graph

=== "JavaScript"
    ``` turtle
    ex:RectangleShape
	a sh:NodeShape ;
	sh:targetClass ex:Rectangle ;
	sh:rule [
	    a sh:JSRule ;
		sh:jsFunctionName "computeArea" ;
		sh:jsLibrary [ sh:jsLibraryURL "http://example.org/js/rectangle.js"^^xsd:anyURI ] ;
    ] .
    ```
=== "Python"
    ``` turtle
    ex:RectangleShape
	a sh:NodeShape ;
	sh:targetClass ex:Rectangle ;
	sh:rule [
	    a sh:PyRule ;
		sh:pyFunctionName "compute_area" ;
		sh:pyLibrary [ sh:pyLibraryURL "http://example.org/py/rectangle.py"^^xsd:anyURI ] ;
    ] .
    ```


#### Example X rule
=== "JavaScript"
    ``` js
    let NS = "http://datashapes.org/js/tests/rules/rectangle.test#";

    function computeArea($this) {
        let width = getProperty($this, "width");
        let height = getProperty($this, "height");
        let area = TermFactory.literal(width.getLex() * height.getLex(), width.getDatatype());
        let areaProperty = TermFactory.namedNode(NS + "area");
        return [ [$this, areaProperty, area] ]; 
    }

    function getProperty($this, name) {
        let it = $data.find($this, TermFactory.namedNode(NS + name), null);
        let result = it.next().getObject();
        it.close();
        return result;
    }
    ```

=== "Python"
    ``` py
    ns = "http://datashapes.org/js/tests/rules/rectangle.test#";

    def compute_area(_this):
        width = get_property(_this, "width")
        height = get_property(_this, "height")
        area = py_tf.literal(width.getLex() * height.getLex(), width.getDatatype())
        areaProperty = py_tf.namedNode(ns + "area")
        return [ [_this, areaProperty, area] ]

    def get_property(_this, name):
        it = _data.find(_this, py_tf.namedNode(ns + name), None)
        result = it.next().getObject()
        it.close()
        return result
    ```

#### Example inferred triples

```
ex:ExampleRectangle ex:area 56 .
```

