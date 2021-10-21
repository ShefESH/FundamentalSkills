# Downloading files and packages

In Linux there are two main utilities for downloading files, and both have a very similar sort of syntax. The go-to for downloading files is `wget`, it is avaliable on (allmost all) Linux distributions as well as windows.

To use `wget`, simply type `wget [url of file/page]`.

![[Pasted image 20211021132339.png]]

This will reach out to the website using http/s and download the file into the current working directory.

There is another command that works for downloading files called `curl`. `curl` is different to `wget` as it's default place to 'save' the file is the standard output (displayed on the terminal). To change this we can use the command `curl [url of file/page] -o /path/to/save/file`

![[Pasted image 20211021132816.png]]