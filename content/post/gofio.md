+++
date = "2017-09-04T15:18:58+01:00"
title = "Gofio"
menu = "main"
+++
Ever so often I have a 4 Mb digital photo that I want to use on a web site. Obviously a 4 Mb image on a web site isn't such a good idea. I could of course open the image in some photo editing program, but a simple command line app that did the job would be nice. Hmmm, perhaps I could write one myself in Go?

It turned out that I could and it wasn't a very big job either. Admittedly I didn't do it from scrath. Instead I based it on the [nfnt/resize](https://github.com/nfnt/resize) package and just wrote a wrapper to it.

For now only jpg images are supported, but I'll probably add png handling as well and perhaps a few more features, such as converting jpg to png.

To use Gofio you need to build it from source:
````
$ git clone https://github.com/hfogelberg/gofio
$ go build main.go
$ go install
````

## Usage
Basic usage is just to type `$ gofio -i my-file.jpg`. The file will be reduced to a default width of 700 px and the output file will be named with a unix timestamp.

## Flags
For more advanced usage input flags are used.

-i: Name of input file. Obligatory
-o: Name of output file. Default is <timestamp>.jpg
-w: Width of output file in pixels. Default is 700. The height is set automatically in proportion to the width.
-r: Remove original file. Default is false.
-h: Show help

So, the command `$ gofio -i big.jpg -o small.jpg -w 1200 -r` will create a 1200 px wide copy of the file big.jpg, name it small.jpg and remove the original file big.jpg.


