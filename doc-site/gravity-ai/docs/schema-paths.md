# Structured Data Schema

<i> Note the following only applies to the selected input/output types that contain 'structured data' (i.e json, csv, etc). </i>

## Schema Paths

A schema defines the structure of a data file's records. Each schema consists of one or more paths that describe the location and type of each piece of data. Schema Paths are similar to directory paths in a file system where 'directories' denote parent objects, and 'files' are the actual data field. Gravity Ai uses schema paths to describe the location of each data point in a record. In a csv file, these paths correspond to the column headers, while in json, they correspond to the fields of each object (and sub object).

Paths can describe single fields, nested fields, and fields with arrays of objects.

A path may or may not start with a forward slash (/), and each nested level in an object is divided by a forward slash.

### Example:

```
{
  field:{
    subfield: “value”
  }
}
```

Has a single Path: `field/subfield`

### Array Types

Arrays of objects have to specify the position in the array for the object. In this manner it is possible to have a single array with different kinds of objects. Object array position is specified by enclosing values within brackets (vector notation).

```
{
    field:[
    {
        object1field1: "value",
        object1field2: "value"
    },
    {
        object2field: {
            subfield: "value",
            arrayfield: [1,6,23],
            mixedfield: [1, "value", 1, "value", 4, "value"]
        }
    }
    ]
}
```

Paths for the first object in the array:

- `field[0]/object1field1`
- `field[0]/object1field2`

Paths for the second object:

- `field[1]/object2field/subfield`
- `field[1]/object2field/arrayfield[*]`
- `field[1]/object2field/mixedfield[0+2]`
- `field[1]/object2field/mixedfield[1+2]`

Vector Notation:

- [*] All positions in vector, no maximum length
- [0] position 0 in vector.
- [0+2] All even positions in vector, starting at position 0, no maximum length
- [1+2] All odd positions in vector, starting at position 1, no maximum length

Increment

- [0+2] positions 0,2,4,6,...
- [0+3] positions 0,3,6,9, ...
- [0+4] positions 0,4,8,12, ...
- [0+5] positions 0,5,10,15, …

Maximum count (if Increment is omitted, it defaults to 1)

- `[0,3]` or `[0+1,3]` positions 0,1,2
- `[3,3]` or [3+1,3] positions 3,4,5
- `[3+2,4]` positions 3,5,7,9
- `[3+2,*]` or `[3+2]` positions 3,5,7,9,...

Maximum Range (if Increment is omitted, defaults to 1). The limit is inclusive. It is illegal to specify a maximum range that is less than the first index.

- `[0-3]` or `[0+1-3]` positions 0,1,2,3
- `[3-3]` or `[3+1-3]` position 3
- `[3+2-7]` positions 3,5,7
- `[2+2-7]` positions 2,4,6
- `[2+2-*]` or `[2+2]` positions 2,4,6,8,...

Valid formats:

- `[<single index>]`
- `[<start index> + <increment> - <upper bound index inclusive>]`
- `[<start index> + <increment> , <count>]`

Multi-Dimensional Vectors
You may chain together multiple vector notations to specify a multi-dimensional array.

- `[0][3][0]` is position (0,3,0)
- `[*][2]` is position 2 in all positions in the first dimension (0,2) (1,2) (2,2) (3,2) ...
- `[2][*]` is all positions in the 2nd position of the first dimension (2,0) (2,1) (2,2) (2,3) ...
- `[2+2,3][2]` is positions (2,2) (4,2) (6,2)

## Schema Path for CSV style data

Paths for CSV data work the same way as JSON, except they do not have nested objects. Each path refers to a column name (as supplied in the first header line of the file). If the data file does not include a header, each field may be referred to by its index (i.e. `/1` `/2` etc). If a header occurs more than once in the file, it is considered to be an array, with the left-most filed starting at position 0.
