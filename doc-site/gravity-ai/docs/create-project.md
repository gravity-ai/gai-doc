# Creating a New Project

## Project Type

There are a few different types of projects that can be created to share on the marketplace:
<br/><br/>
1. Production Code <br/>
2. R Environment Code <br/>
3. Teal Visualizations <br/>


### Production Code

This project is meant to host code in a production environment. You upload your code/model as an archive, which is then packaged into a distributable container with a standard API.

#### Creating Your Model

Language Options include Python and R.

#### Preparing your model for upload.

Models should be archived into a zip format (gzip, etc) archive. Your source code, data files, and anything else needed to distribute your model should be included in the zip file. Additionally, you can include a [Build Configuration File](build-config.md) to further customize your build settings.

#### Python Library

A Pypy hosted library is available for python to help make it easier to integrate your code with our system.

#### Command line execution

Your script will be run from the command line. It must not rely on any user interaction. Your script should accept an input file path, and an output file path. Any command line options that are required for launch should be added to the command line field template during the configuration step.
