# Python

## Numpy
Arrays


## Matplotlib
Plot graphs


## Anytree
Construct Decision tree

## JSON
### encoding

Serialize obj as a JSON formatted _stream_ to fp (a `.write()`-supporting _file-like object_): 
```
json.dump()
```

Serialize obj to a JSON formatted _str_: 
```
json.dumps()
```

### decoding

Deserialize fp (a `.read()`-supporting _text file_ or _binary file_ containing a JSON document) to a _Python object_:
```
json.load()
```

Deserialize s (a _str_, _bytes_ or _bytearray_ instance containing a JSON document) to a _Python object_:
```
json.loads()
```

### Conversion table:
| JSON | Python |
| ---- | ------ |
| object | dict |
| array | list |
| string | str |
| number (int) | int |
| number (real) | float |
| true | True |
| false | False |
| null | None |

