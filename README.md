# snoing-2.0
A singularity recipe for building a SNO+ environment for RAT. 

# To build the container
To build, you must have root permissions and singularity installed on your machine (the image can be moved to a cluster once it has been built). Install singularity manually, or do `sudo apt-get install singularity-container` on debian-based systems (like Ubuntu).

Download the `Singularity` recipe file, and run the following command, which will produce a container file called `snoing.simg`:

`sudo singularity build snoing.simg Singularity`

# Instructions on how to use the container with RAT

**To build RAT**:
- After building the container above, clone RAT from GitHub
- Add execute permissions to the rat/env.sh script with this command:

`chmod +x path/to/rat/env.sh`
- Enter the following command, filling in the path to RAT with your own. This will mount your RAT repo to the directory /rat inside the container:

`singularity run --app build-rat -B path/to/rat:/rat snoing.simg`
- RAT is now ready to use! Look at the instructions below for how to run it

**To exit the container**:

`exit`

**To update RAT**:

- Outside of the container, `cd` into your RAT repo, and run:

`git fetch && git merge`
- Then, follow the build instructions above to (re)build RAT

**To run RAT**:
- Enter the following command, filling in the path to RAT with your own:

`singularity shell -B path/to/rat:/rat snoing.simg`

- It is important to mount your rat dir to /rat as the build scripts look there for it!
- RAT is primed, now you can navigate to /rat to run things in the repository. To use other directories as well, see below.

**To read/write files from directories outside of RAT/home**:
- Add additional bind mounts to your singularity shell command
- Example:
`singularity run --app build-rat -B path/to/rat:/rat,other/path:/stuff snoing.simg`
- Now in the container, you have access to other/path by going to /stuff

**To use a specific branch of RAT**:
- Ensure you git checkout to the branch OUTSIDE the container to avoid issues, then run RAT like above

**To see this help message on the command line**:

`singularity help snoing.simg`

# FAQ

**I'm seeing an issue related to rat/env.sh or "/scif/apps/build-rat/scif/runscript: line 9: scons: command not found"**
- Ensure that you have added execute permissions to this file so that singularity can access and execute it (since it is outside the container)
