# Build Configuration File

You can control additional build options for your project by including a "gravityai-build.json" file in your uploaded archive. This file should be in standard json format. Be sure it's located at the root level of the project:<br/>
```
Project_to_Upload/
   |--> src/
   |--> gravityai-build.json
```

## Example File

The following example installs tesseract as a dependency. It also offers an input file along with expected output to use as a test at build time.

```
{
  "SystemPackages": [
    "g++",
    "libleptonica-dev",
    "autoconf",
    "automake",
    "libtool",
    "pkg-config",
    "libpng-dev",
    "libjpeg62-turbo-dev",
    "libtiff5-dev",
    "zlib1g-dev",
    "tesseract-ocr"
  ],
  "Tests": [
    {
      "RelativeInputPath": "./OCR-Passage-Test-1.jpg",
      "RelativeReferencePath": "./OCR-Passage-Test-1.result.txt"
    },
    {
      "RelativeInputPath": "./OCR-Check-Test-1.jpg",
      "RelativeReferencePath": null
    }
  ]
}
```

## SystemPackages Field

This is an array of string values, each the name of a linux system package. At the moment, these packages should be available on the debian linux distribution. The base container image is debian {{supported.debian.version_name}}, so you may need to include additional packages not normally available on {{supported.debian.version_name}}.

```
"SystemPackages": [
    "g++",
    "automake"
]
```

## Tests Field

<b>Tests</b> is an array of Test Objects:

```
"Tests": [
    {
      "RelativeInputPath": "./OCR-Passage-Test-1.jpg",
      "RelativeReferencePath": "./OCR-Passage-Test-1.result.txt"
    },
    {
      "RelativeInputPath": "./OCR-Check-Test-1.jpg",
      "RelativeReferencePath": null
    }
]
```

Each Test object has the following fields:

- `RelativeInputPath` - A text string path to look at in the archive, relative to the directory location of this configuration file. The path should point to a test data input file that your algorithm will accept and run against at build time. If the output produced is structured data, then it is compared against the expected structure added to the build settings (ie. [Schema Paths](/schema-paths/)).

- `RelativeReferencePath` - A test string path to look at in the archive, relative to the directory location of this configuration file. The path should point to the expected output that your algorithm will product, given the above input file. The results of your algorithm are compared against this file (a <i>byte-wise comparison</i>). If the result is identicle to this file, the test passes. Otherwise the build fails. This filed may be set to null or omitted altogether. This results in the output being verified that it exists, but it is not compared against a file.
  <i>Note that the byte-wise comparison can fail due to character encoding and line-ending difference between linux and the operative system you produce the original file on.</i>
