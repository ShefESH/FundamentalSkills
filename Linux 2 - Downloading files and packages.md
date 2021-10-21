# Downloading files and packages

## Downloading files

In Linux there are two main utilities for downloading files, and both have a very similar sort of syntax. The go-to for downloading files is `wget`, it is avaliable on (allmost all) Linux distributions as well as windows.

To use `wget`, simply type `wget [url of file/page]`.

![[Pasted image 20211021132339.png]]

This will reach out to the website using http/s and download the file into the current working directory.

There is another command that works for downloading files called `curl`. `curl` is different to `wget` as it's default place to 'save' the file is the standard output (displayed on the terminal). To change this we can use the command `curl [url of file/page] -o /path/to/save/file`

![[Pasted image 20211021132816.png]]

## Downloading packages

Packages are basically the applications and libraries that your computer can use. Package managers automate downloading, installing and also updating all of your packages for you in a simple and relatively intuative way (It's a lot better than how Windows manages installing software).

Your package manager normally depends on what distribution of Linux you have installed. For example Ubuntu uses the `apt` package manager, a relatively common package manager. 

I'll start off with installing packages on Ubuntu, as it's the most common distribution. Like I said earlier, Ubuntu uses `apt` to install packages. Before apt can install packages, it needs to update a list of avaliable applications it can install. To do this we type `sudo apt update`. Once apt has fetched the list of avaliable packages, we can install a package with `sudo apt install [package name]` then press `y` to install the package. We can also update all the currently installed packages with `sudo apt upgrade`, again we need to press `y` to continue with the installation.

![[Pasted image 20211021144503.png]]

If you want to find packages and how to install them on your distribution, you can head over to [command-not-found.com](https://command-not-found.com), the website allows you to search for packages based on the command, for example the `ls` command is part of the `coreutils` package.

## Installing packages manually
Manually installing packages means that you have to type some extra commands to install them, and you have to maintian them yourself (as the package managers generally won't update them for you).


