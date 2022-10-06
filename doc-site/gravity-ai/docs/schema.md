# What is a “schema”? 

Put simply, a “schema” is simply the organization and/or logic of data, which then can be used to establish tables of data.  Think of it as a sort of blueprint for understanding how your data should exist within a database. 

# How are we using “schema” with regards to input/output for models?

It’s important to understand that data can be presented to a model in a myriad of ways.  It can be “structured” where there’s a sort of order or hierarchy of data (JSON, csv, etc), or it can be “unstructured” where data is presented pretty much as-is.  

In the case of unstructured data, we really don’t need to have a schema as the data is presented as-is.  Text in a text file is just a series of alphanumeric characters.  Images and videos is just binary data at the end of the data.  There’s no logic behind this data, and hence no need to define a schema.  

In the case of structured data, you might need to establish a schema that defines the logic of how your data is structured so that we, at gravityAI, can understand it.  

There are numerous ways of defining the schema structure, but we are using the concept of “schema paths”.  Like the concept of a “directory path,” except we are looking to find data fields and not files, we are simply looking for a way of showing how to find individual fields in the data your model consumes.  


Schema Paths can describe single fields, nested fields, and fields with arrays of objects.
A path may or may not start with a forward slash (/), and each nested level in an object is divided by a forward slash.


## Example
```json
{
  field:{
    subfield: “value”
  }
}
```
Has a single Path: field/subfield

# Array Types

Arrays of objects have to specify the position in the array for the object. In this manner it is possible to have a single array with different kinds of objects. Object array position is specified by enclosing values within brackets (vector notation).

```json
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

**Paths for the first object in the array:**

- field[0]/object1field1
- field[0]/object1field2

**Paths for the second object:**

- field[1]/object2field/subfield
- field[1]/object2field/arrayfield[*]
- field[1]/object2field/mixedfield[0+2]
- field[1]/object2field/mixedfield[1+2]

**Vector Notation:**

- [*] All positions in vector, no maximum length
- [0] position 0 in vector.
- [0+2] All even positions in vector, starting at position 0, no maximum length
- [1+2] All odd positions in vector, starting at position 1, no maximum length

**Increments:**

- [0+2] positions 0,2,4,6,...
- [0+3] positions 0,3,6,9, ...
- [0+4] positions 0,4,8,12, ...
- [0+5] positions 0,5,10,15, …

**Maximum count (if Increment is omitted, it defaults to 1):**

- [0,3] or [0+1,3] positions 0,1,2
- [3,3] or [3+1,3] positions 3,4,5
- [3+2,4] positions 3,5,7,9
- [3+2,*] or [3+2] positions 3,5,7,9,...

**Maximum Range (if Increment is omitted, defaults to 1). The limit is inclusive. It is illegal to specify a maximum range that is less than the first index.**

- [0-3] or [0+1-3] positions 0,1,2,3
- [3-3] or [3+1-3] position 3
- [3+2-7] positions 3,5,7
- [2+2-7] positions 2,4,6
- [2+2-*] or [2+2] positions 2,4,6,8,...

**Valid formats:**

- `[<single index>]`
- `[<start index> + <increment> - <upper bound index inclusive>]`
- `[<start index> + <increment> , <count>]`

**Multi-Dimensional Vectors You may chain together multiple vector notations to specify a multi-dimensional array.**

- [0][3][0] is position (0,3,0)
- [*][2] is position 2 in all positions in the first dimension (0,2) (1,2) (2,2) (3,2) ...
- [2][*] is all positions in the 2nd position of the first dimension (2,0) (2,1) (2,2) (2,3) ...
- [2+2,3][2] is positions (2,2) (4,2) (6,2)


# Schema Path for CSV style data

Paths for CSV data work the same way as JSON, except they do not have nested objects. Each path refers to a column name (as supplied in the first header line of the file). If the data file does not include a header, each field may be referred to by its index (i.e. /1 /2 etc). If a header occurs more than once in the file, it is considered to be an array, with the left-most filed starting at position 0.

## Important to remember!


- It’s important to remember that, for structured data, schemas are required for both input and output.  
- It’s important to remember that your schema must match the data you supply to your model.  If you provide an input field that specifies it is of “string” type, but really, your model accepts an _array_ of strings, it simply will not work. 
    * Be sure to “preview” and verify your schema before building. 
