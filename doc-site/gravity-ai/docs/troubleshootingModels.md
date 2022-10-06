# Troubleshooting

**Q. My model is hanging, what gives?!** <br/>

- This can be the result of numerous things, but likely, your model is likely incorrect.  This can be anything from a spacing problem in a python script, to a dependency problem leading to X not being recognized.  
- The best way to resolve this issue is to log into a debian slim-buster container and attempt to build and run your application there (in the container).  If you see any exceptions generated in the container CLI as you run your application, try to address those problems (and if necessary correct your dependencies) and re-upload/rebuild.
- You might save yourself a lot of heartache by simply setting up a debian vm and developing your project (initially) there. There’s also nothing to stop you from developing your project from within the debian slim-buster container.    


**Q. My model can’t find my input file, what do I do?** <br/>

- Your script should be expecting an input string denoting a path to an input file.  
    * See "Input Output Handling"

**Q. My container failed with some weird error message, but it worked fine locally. What’s wrong?** <br/>

- This can be a couple of things.  In particular… 
    * if you develop your application on a MAC/Windows platform and expect your dependencies to be the same on debian, you might be in for a rude awakening.  Make sure your model works on linux (debian, preferably)!
    * Say if you were to take special care to develop/run your application in the same environment you’re deploying to (currently, a debian slim-buster container), and it works there but doesn’t work when a job is submitted (via our GUI).  Then this is likely due to your build settings. Re-check your build settings and make sure your schema, CLI arguments, etc. are correct. 
