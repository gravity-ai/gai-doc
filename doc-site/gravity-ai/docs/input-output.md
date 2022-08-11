# How should your model handle input and output?

To get your model to handle input and output, your entry script must be executed with (at minimum) two arguments representing paths to input and output files.  Input files cannot be assumed to be static files existing in your model, as the end user of your container will need to supply their own input files.  

# Getting Input/Output recognized when NOT using the gravityai library.

One way of handling input and output is to handle an input file path as supplied at the CLI directly.  

Below is a working example (an example of the pythagorean theorem) of this use case: 


``` py
1.  import numpy as np
2.  import pandas as pd
3.  import math
4.  import os
5.  import warnings
6.  from pathlib import Path, PurePath
7.  import sys
8. 
9.
10. warnings.filterwarnings("ignore")
11. 
12.
13. if (len(sys.argv) <= 1):
14.    sys.stderr.write("No file specified on command line")
15.    sys.exit(2)
16.    
17. datafile = Path(sys.argv[1])
18. if (not datafile.is_file()):
19.    sys.stderr.write("Input file not found")
20.    sys.exit(2)
21.    
22. dataPath = str(datafile.resolve()) 
23.
24. 
25. if (len(sys.argv) <= 2):
26.    sys.stderr.write("No output file specified on command line")
27.    sys.exit(3)
28.    
29. outfile = Path(sys.argv[2])
30. print(outfile)
31.
32.
33. outPath = str(outfile.absolute())
34. print(outPath)
35.
36.
37. #print(__file__)
38. me = Path(__file__)
39. dir = me.parent
40.
41.
42. os.chdir(str(dir.resolve()))
43.
44.
45. #sample data for testing the model prediction
46. test_df = pd.read_csv(dataPath, encoding="utf-8")
47. 
48.
49. #Calc Hypotenuse
50. test_df["z"] = np.sqrt(np.power(test_df["x"],2) + np.power(test_df["y"],2))
51.
52.
53. test_df.to_csv(path_or_buf=outPath, index=False)
54. 
55. 
56. print(test_df.to_csv(index=False))
```

## Processing Input:
* line 39: test_df = pd.read_csv(dataPath, encoding="utf-8")
    * This is how our input file is being processed/read into a pandas dataframe, where dataPath is the path to our input file

_Which is parsed on_

* line 17: `datafile = Path(sys.argv[1])`
* line 18: `if (not datafile.is_file()):`
* line 19:	  `sys.stderr.write("Input file not found")`
* line 20:	  `sys.exit(2)`
* line 21:
* line 22: `dataPath = str(datafile.resolve())`
     
## Processing Output:
* line 53: `test_df.to_csv(path_or_buf=outPath, index=False)`
    * This is how our output file is being generated,

_Which is parsed on_

* line 29: `outfile = Path(sys.argv[2])`
* line 30: `print(outfile)`
* line 31: 
* line 32: 
* line 33: `outPath = str(outfile.absolute())`
 
So, looking at 17 and 29, you can see that sys.argv[1] and sys.argv[2] are used for the input and output files respectively.
Relative paths are made absolute and data is parsed and processed.  
 
## How this script should look at the CLI:
 
Since there are no switches, and we are simply processing sys.argv[1] and sys.argv[2], you can expect the python script to run as follows
 
`$> python test.py input.dat output.dat` 
 
if you were to run it locally and outside of the container.
 
## How build settings look on gravityai when setting up the pythagorean model:
 

Note especially the "Command line arguments" line `{input} {output}`.

`{input}` and `{output}` are just generic placeholders for the input file and output filenames.
 
Assume your script runs at the CLI as follows...
`$> python test.py input.dat output.dat` , then  `{input}` and `{output}` maps like this...

``` 
$> python test.py input_file.dat output_file.dat 
                  {input}        {output}
```

More specifically (just in case the alignment above got screwed up), `{input}` maps to  `input_file.dat` and `{output}` maps to `output_file.dat`
 
All your entry script needs to be able to do is accept command line arguments for an input file and output file respectively.  You don't need a specific naming scheme or anything like that.  
 
 
NOTE: Your Command Line Input in the builds settings will change if you introduce additional arguments for your entry script.  E.g.  If your scripts runs like 
$> python test.py -i input.dat -o output.dat
 
Then you will need to alter your gravityai build settings, so that the command line input is 
 
`-i {input} -o {output}`
 
 
 
----------------------------
 
# Getting Input/Output recognized when using the gravityai library.
 
While using our library function grav.wait_for_requests(your_function), your_function should assume as arguments an input file path and output file path respectively.  Your script should then parse that input file, perform whatever logic necessary, and generate an output file referenced by the output file path.  
 
See below for a working example (implementing the pythagorean theorem):

``` py
1.  import numpy as np
2.  import pandas as pd
3.  from gravityai import gravityai as grav
4.  
5.  
6.  async def doIt(dataPath, outPath):
7.     test_df = pd.read_csv(dataPath, encoding="utf-8")
8.     # Calc Hypotenuse
9.     test_df[0]["z"] = np.sqrt(np.power(test_df[0]["x"], 2) + np.power(test_df[0]["y"], 2))
10. 
11.    test_df.to_csv(path_or_buf=outPath, index=False)
12. 
13.  
14. grav.wait_for_requests(doIt)
``` 
 
 
 
## Processing Input:
* line 14: `grav.wait_for_requests(doIt)`
    * This is the entry line to the script.  In particular, it's calling wait_for_requests, which takes as an argument a function with two arguments referencing an input file path and output file path.

_Input is then parsed as follows..._

* Line 6: `async def doIt(dataPath, outPath):`
* Line 7:        `test_df = pd.read_csv(dataPath, encoding="utf-8")`

## Processing Output:
* line 14: `grav.wait_for_requests(doIt)`
    * This is the entry line to the script.  In particular, it's calling wait_for_requests, which takes as an argument a function with two arguments referencing an input file path and output file path.

_Input is then parsed as follows..._
* Line 6: `async def doIt(dataPath, outPath):`
* Line 11:       `test_df.to_csv(path_or_buf=outPath, index=False)`
 
 
## How this script should look at the CLI:
 
Due to the reliance on the gravityai library for this example, the CLI might look a bit differently.  In particular, the command "run" must be supplied.
 
In particular, it might look like this...
 
`$> python test.py run input.dat output.dat` 
 
Don't worry, you need not account for this in your entry script, as we will be supplying the path to input.csv and output.csv for you.  All you need to do is code a function that expects an input file path and output file path.  See below.
 
## How build settings look on gravityai when setting up the pythagorean model:  
 
Note that you no longer need to specify that command line input.  This is because you're now using a helper method
 
* line 14: `grav.wait_for_requests(doIt)`
 
which leads to a user defined
 
* line 6: `async def doIt(dataPath, outPath)`
 
More specifically, `grav.wait_for_requests()` accepts as an argument, a function which, in turn, accepts as arguments an input file path and output file path respectively.   This function can be named whatever, and the arguments can be named whatever you wish as well.  The important thing is that it's a function which accepts as arguments an input file path and output file path and the logic of that user defined function should assume such.  
 
----------------------------

