# Mapping File Format

When submitting structured data to the container, an optional mapping file may be supplied to map data paths between the input data schema, and the container's schema. Mapping files can be created in the marketplace by testing a dataset against a model. The mapping file for the test can be downloaded from the test results page. It can also be created manually.

The schema mapping file is a json format file. It consists of an array of mapping objects.

## Example

```
[
    {
        "source": "source_path_a",
        "destination": "dest_path_a",
        "defaultValue": 1
    },
    {
        "source": "source_path_b",
        "destination": "dest_path_b",
        "defaultValue": 1
    }
]
```

## Mapping Object

```
{
    "source": "source_path_a",
    "destination": "dest_path_a",
    "defaultValue": 1
}
```

### Source Field

This is a [schema path](schema-paths.md) string that points to a field in the input data.

### Destination Field

This is a [schema path](schema-paths.md) string that points to a field in the destination data (ie. the data expected by the model).

### DefaultValue Field

This is the default value that should be used if data in the input file is missing. This field supports multiple primative data types (i.e. string, number, boolean)
