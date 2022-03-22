# Uploading an R Project

Before uploading a project to gravityAi it need to be prepared using the following steps:

## 1. Project dependencies

Gravity AI depends on renv to load prerequisite packages properly. This ensures that the packages used by your script are loaded properly into the container. Without this step, there would be no guarantee that your code would have the prerequisite packages installed.

Install the latest version of renv ({{supported.r.version_number}} at the time of this writing) to your local computer (from CRAN).

`r$> install.packages(‘renv’)`

Initialize renv. Make sure you are running R from your root project directory.

`r$> renv::init()`

This will create a subdirectory called ‘renv’ in your project folder that contains a few files. It will also create a renv.lock file which contains a list of package dependencies for your project.

Restart R, and verify that the dependencies are correct.

If you change any packages, make sure you update the lock file with:

`r$> renv::snapshot()`

### 1.1 Local and Private Packages

Packages installed from private repositories (or that exist solely on your pc) need special handling and must be included in the project zip file before uploading to gravityAi.

Create a subdirectory called ‘local’ in the renv folder. Copy the source packages (tar.gz files) to this folder, making sure they are named correctly:

`<package>_<version>.tar.gz`

Example:
For a package called ‘mypackage’ with version 1.2.3 copy the source to the following directory:
`./myproject/renv/local/mypackage_1.2.3.tar.gz`

Important: Make sure you have authorization to upload any private packages.

More information available here:
https://rstudio.github.io/renv/articles/local-sources.html

## 2. Zip and upload the project

gravityAi supports multiple zip-style compression formats such as .tar.gz, zip, .7z, etc. Choose a format and then compress your project directory. You may exclude the following directory created by renv (excluding it will make your compressed file much smaller):

`myproject/renv/library`

Be sure to include any data files or other files that your project needs in order to function properly.

### 2.1 Upload the zip file to gravityAi

Create a new project and create a new version under that product. Alternatively, you may create a new version under an existing project. Upload your model (e.g. zip file of code) to the version. When editing your build settings, you will need to select the env.lock file, and the R script file that is the main entrypoint for your code.
