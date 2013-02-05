# Smartresize

… is a Bash script to scale down an image. It uses **convert/mogrify** functions of **ImageMagick** and predefined configuration that I found to produce the best results in my experience. Almost like Photoshop's "Bicubic Automatic!" (Read below for technical details).

### Requirements

Please make sure your computer has:

* imagemagick
* bc
* python

### Installing

You can simply put the script into the folder with image(s) and run:

	bash smartresize [options]

Or you can put it in some folder included in your PATH (f.e. `/usr/bin/'):

	sudo cp smartresize /usr/bin/
	sudo chmod +x /usr/bin/smartresize

...and then run from any folder:
 
	smartresize [options]
	
### Options

Option|Description
-|-
`-?` | Show help message
`-i` | path to input file (required)
`-w` | target width in pixels
`-h` | target height in pixels
`-o` | output file (optional)
`-q` | output JPEG quality (optional)
`-a` | additional imagemagick arguments (use with double quotes and caution)

* Either width or height is required (if both are provided, output file will be cropped to match the new ratio).
* Input file will be replaced if `-o` is blank.

### Examples

Suppose we have some test*.jpg files, 2560x1600 each.  

Resize test1.jpg to 800x500 (keep ratio):

	smartresize -i test1.jpg -w 800

Resize test1.jpg to 800x600 (crop):

	smartresize -i test1.jpg -w 800 -h 600
	
Resize test1.jpg to 800x500 and save as test1.small.jpg:

	smartresize -i test1.jpg -w 800 -o test1.small.jpg
	
Resize all JPG files in folder:

	for f in $(ls *.jpg); do smartresize -i "${f}" -w 800; done;
	
Take all JPG files in folder "originals" and resize into "small" folder:

	for f in $(cd originals; ls *.jpg); do smartresize -i "originals/${f}" -w 800 -o "small/${f}"; done;
	
	
### Notes

Only scaling down is supported. 
	
### Why smartresize, not just convert -resize ?

The biggest problem of scaling down an image is to find right settings for **resampling** and **sharpening**. After many expreiments, I found it impossible to achieve good results by simply running **convert** with a line of arguments. So I came up with this script, which basically does the following:

* configures -interpolate **bicubic** -filter **Lagrange**;
* resizes source image 80%;
* applies -unsharp 0.44x0.44+0.44+0.008
* repeats steps 2 & 3 until target size is reached.

This seems to work very well with all types of resizes (f.e. 2560x2560 to 2048x2048 and 2560x2560 to 200x200 both are sharp enough and not too sharp).

### License

* Do whatever you want! 
* I am not responsible for any harm.

### Author
Written by Vlad Gerasimov  
<http://www.vladstudio.com>  
<vladstudio@gmail.com>


