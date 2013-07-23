qrgen
=====

Basic bulk QR Code Generator in Python. 

I wrote this tool to generate QR Codes for PaperCut Mobile Release printers. 
It's provided as is, without any warranty or support. If it's useful to you, great!


Installation
------------
This tool requires *PIL* or *pillow*, and *pyqrcode*. In a virtualenv on Ubuntu, 
you can run:

    pip install pillow qrcode

However, pip and virtualenv aren't always installed by default, so 

    sudo apt-get install python-pip; sudo pip install pillow qrcode

might be required. 

I have no idea about PIL on Windows; I assume there are binary builds available... 
I would look into compiling this for Windows if anyone was ever interested. 


Usage
-----

Help can be output with the -h flag. 

    usage: qrgen.py [-h] [--overlay OVERLAY] [--prefix PREFIX] [--suffix SUFFIX]
                    [--format {png,jpg,bmp}] [--size SIZE] [--border BORDER]
                    [text [text ...]]

    Generate QR codes as PNG files

    positional arguments:
      text                  Text to encode in the QR code.

    optional arguments:
      -h, --help            show this help message and exit
      --overlay OVERLAY     a PNG file to overlay in the centre of the code. Test
                            your codes work!
      --prefix PREFIX, --url PREFIX, -p PREFIX
                            Optional text added to the start of the code's text,
                            e.g. http://server/qr/
      --suffix SUFFIX, -s SUFFIX
                            Optional text added to the end of the code's text.
      --format {png,jpg,bmp}, -f {png,jpg,bmp}
                            Image format (default: PNG)
      --size SIZE           Pixel width per "box" of the QR (default: 5)
      --border BORDER       Border width in "boxes" (at least 4 is required by the
                            standards)

    This tool is provided AS-IS and no support or warranty is given or implied.


Overlay Images
--------------
The tool will overlay a suitably sized PNG file over your QR code. You specify the 
file to overlay from the command line, e.g. --overlay overlay.png. 

**Important**: The overlay is placed AS IS, not resized at ALL. If it is too large, 
the QR code may not work. Test a few codes to make sure it's not interfering - the 
amount of text in the QR code controls how well the image will work. 

For --size=4, I found a 60x60 image worked quite well. Transparency is preserved. 

Images in QR codes work because of QR code error correction. They're obscuring a bit
of the code, but with error correction on, the code keeps repeating itself as many 
times as will fit, to ensure the code can be read. 


Size / Border
-------------
The --size parameter controls how many pixels make up the width of a QR's "pixel". 
The default size is 5; experiment and see. See the pyqrcode docs for more info. 

Changing the --size parameter will not scale your image. 

According to the specifications, the QR code needs a border to work properly. The 
default setting should suffice. 


Input
-----
The text to generate is specified either as one or more arguments, or by using the 
special syntax @filename to read them from a file. 

You can use the switch --prefix "sometext" to prepend something to the text, and
--suffix to add something to the end. 

For example, you can do: 

    python qrgen.py make-this-code another-code "this one has a space" "hello there" etc

Or you could also do this:

    python qrgen.py --prefix "http://print-server/mr/" @printers.txt

Where the contents of printers.txt is one printer name per line. 

Output
------
One QR code is generated per inputted filename. The filename is the same as the string 
generated, with some unsafe characters removed. The --prefix isn't included in the 
name of the file. 

--format controls the format of the file. The default is PNG. 


Example
-------
To generate a bunch of QR codes going to http://print-server:9191/mr/this-printer 
from the file printers.txt, at size 4, with the image overlay.png embedded in the 
center: 

    python qrgen.py --prefix "http://print-server:9191/mr/" --overlay overlay.png --size 4 @printers.txt


It will write out one PNG per line in printers.txt to the current directory. 


Good luck!