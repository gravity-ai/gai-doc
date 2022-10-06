# Build Testing

It’s possible to run tests at build time to ensure that your container is working properly after it’s been built by gravityAI. All you would need to do is alter your gravityai-build.json file to include paths to both a test input file (“RelativeInputPath”) and a theoretical output file (“RelativeReferencePath”) to verify against. Including such tests will allow you to verify that your build settings are correct.  

It is highly recommended that you include such tests to verify your model.
An example of a gravityai-build.json file containing such tests can be found below:


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

<b>Tests</b> is an array of Test Objects:

Each Test object has the following fields:

- `RelativeInputPath` - A text string path to look at in the archive, relative to the directory location of this configuration file. The path should point to a test data input file that your algorithm will accept and run against at build time. If the output produced is structured data, then it is compared against the expected structure added to the build settings (ie. [Schema Paths](/schema-paths/)).

- `RelativeReferencePath` - A test string path to look at in the archive, relative to the directory location of this configuration file. The path should point to the expected output that your algorithm will product, given the above input file. The results of your algorithm are compared against this file (a <i>byte-wise comparison</i>). If the result is identicle to this file, the test passes. Otherwise the build fails. This filed may be set to null or omitted altogether. This results in the output being verified that it exists, but it is not compared against a file.
  <i>Note that the byte-wise comparison can fail due to character encoding and line-ending difference between linux and the operative system you produce the original file on.</i>