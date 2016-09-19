---
title:  "Using terminal to copy files from remote"
date:  2016-09-19
categories: [linux]
tags: [server, linux]
---
For copying files from one server to the other you can use both the terminal and GUI. Listed below are the steps that you can follow to copy files using the linux **terminal**.

## [**scp**](http://man7.org/linux/man-pages/man1/scp.1.html) secure copy protocol.  
--------

```
$ scp <source> <destination>  
```
When the source is a remote:  

```
$ scp user@source_ip:/path/to/file /path/to/local/destination/directory
```

When both the source and the destination are remote:  

```
$ scp user@source_ip:/path/to/file user@destination_ip:/path/to/destination/
```

scp can also be used to copy entire directory from the source:

```
$ scp -r user@source_ip:/path/to/directory user@destination_ip:/path/to/destination/  
$ scp -r dhruv@8.8.8.8:/home/dhruv/Desktop/dhruvsingh dhruv@4.4.4.4:/home/dhruv/Desktop/dhruv
```
**-r** - denotes the recursive flag, which copies the source directory recursively to the destination. Please note scp follows symbolic links as well!

Recursively copying will mean going to each file and issuing a copy command, fair enough. But what if you have 10,000 files to copy;  
oops!, waiting....  

### Enter [tar](http://man7.org/linux/man-pages/man1/tar.1.html), an archiving utility in linux

Use **tar** to make a tarball on the source and then use scp to download the same.

```
$ tar -czvf archive_name.tar.gz /path/to/directory_that_needs_to_be_compressed
$ scp user@source_ip:/path/to/archive_name.tar.gz /local/destination/directory
```

A single file or an entire directory can be compressed to form a tarball.

* **-czvf**, that's 4 flags being passed!
  * -c - **c**ompress
  * -z - g**z**ip (protocol for compression)
  * -v - **v**erbose (show the progress, what file is being compressed)
  * -f - **f**ilename

Using tar to compress multiple files and directories into one tarball.

```
$ tar -czvf archive_name.tar.gz file_one.py /path/to/dir_1
```

Now when the archive is on your machine, use tar again to uncompress to the current directory, just replace **c** in **c**zvf to **-x**(e**x**tract).

```
$ tar -xzvf name_of_archive.tar.gz
```

[FileZilla](https://filezilla-project.org/) a GUI, utility can also be used to copy files from remote to your machine.