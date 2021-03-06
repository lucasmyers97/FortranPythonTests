# F2PY Instructions
What follows is a brief guide for how to use the F2PY package to compile Fortran code into an importable Python package. Additionally, there is a brief guide about how to set up the relevant compiler on a machine running Windows. 
## Setting up the necessary Fortran and C compilers on Windows
Before starting, I'll note that there is a really good Stack Overflow post [here][SO1] explaining how to do this. Now, if you don't already have a Fortran compiler installed (one is not installed by default on Windows), you will need to do that. For this tutorial, we use [gfortran][fg] which comes with the [MinGW][MGW] framework which incorporates a bunch of compilers. MinGW can be downloaded [here][MGWD]. I recommend using the "Online Installer" for ease of use. However, note that you will need to change the default architecture from i686 to x86_64 if you are on a machine running x86 architecture (which you probably are). 

After you have completed the installation, you'll need to add the MinGW binary files folder to your Path environment variable so that your computer knows where to look for the compilers. To do this, you first need to find where MinGW is kept. Search "This PC" in the search bar and click the `This PC` app. If it is installed in the default folder, you should be able to get to it by navigating `C:\Program Files\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64\bin`. It may be that you have installed a slightly different version, so you'll have to navigate through `Program Files`, `mingw-w64`, etc. by just clicking through until you find the `bin` folder. In any case, once you find it copy the folder path. 

To add this path to your Path environment variable, seach "Environment Variables" in your Windows search bar and then click the `Edit the system environment variables` prompt. Then click the `Environment Varibles...` button. There should be a variable called `Path`. Double clicking that should bring up the paths stored in your Path environment variable. Double click on an empty space and paste in the folder path. That's all!

## Compiling simple Fortran programs in the command terminal and making them more "pythonic"
F2PY has a very simple and concise introduction to the package [here][f2py] that can be used immediately after you have installed and linked your compilers. Note that, for Windows users who have installed Python via Anaconda, you will want to use the Anaconda Prompt to execute the command line commands. The only other problem that Windows users may run into (that I ran into) was that the default C-compiler is the Microsoft Visual Studios compiler. If this is the case, it will throw an error claiming that it cannot find a particular `.bat` file. To circumvent this, just add the `--compiler=mingw32` option at the end whenever you are using the `-c` compile option with F2PY.

## Compiling simple Fortran programs via a Python script
Sometimes it is nicer to use a Python script to do these kinds of compilations if say, you needed to programmatically read in lots of filenames from a folder. Information on how to do this is found at the bottom of [this page][npf2py]. One just needs to import `numpy.f2py` and then use either the function `numpy.f2py.run_main()` to generate a signature file (equivalent to running `f2py <args>`), or the function `numpy.f2py.compile()` to actually compile the Fortran code (i.e use the `-c` option with the `f2py` program). The only difference is that the source file must be read into Python as a string. This method is shown in the `Example1` and `Example2` folders in this repository. They recreate the first two examples from the F2PY documentation using pure Python.

## Compiling Fortran programs which use subroutines in different files
Oftentimes Fortran programs will use subroutines that are defined in separate `.f` files in order to make code more modular. To tell the compiler where these subroutines are defined, one must pass in the separate filenames as extra arguments in the `numpy.f2py.compile()` function. Note that this works both with and without a modified signature file (i.e. "The quick way" and "The smart way"). In `Example3` and `Example4`, there is a subroutine `fibadd` which first calls the `fib` subroutine from before to generate the Fibonacci numbers, and then adds one to every entry -- `fibadd` lives in a file `fibadd1.f` while `fib` lives in a file `fib1.f`. These examples show how to properly link these files for compilation.


[SO1]: https://stackoverflow.com/questions/48826283/compile-fortran-module-with-f2py-and-python-3-6-on-windows-10
[fg]: https://gcc.gnu.org/wiki/
[MGW]: http://www.mingw.org/
[MGWD]: https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/7.2.0/threads-posix/seh/
[f2py]: https://numpy.org/doc/stable/f2py/f2py.getting-started.html
[npf2py]: https://numpy.org/doc/stable/f2py/usage.html
