# Printing with Linux

## Brother Scanner setup

* **installation** <br/>
  Download installation script on official website. <br/>
  Launch the installation, and select ip address as connection (usb has known issue and does not work). <br/>
  The following command will be launched, keep it if your printer's ip address changes : <br/>
  ```brsaneconfig4 -a name=<name> model=<model> ip=192.168.x.y```

* **scanning tool** : install Simple Scan with ```apt-get install simple-scan```

## Scanimage : scanning with command lines
```shell script
# basic scan of a picture  
scanimage --format=PNG > test.png
# scan in B&W 
scanimage --format=PNG --mode=Gray > pix.png
# 150 dpi
scanimage --format=PDF --resolution 150 > ./something.pdf
# scan to image then convert to pdf with Image Magick (better portability)
scanimage --format=PNG -> pix.png
convert pix.png pix.pdf
```

## Merge pdf documents with Image Magick

Install it with ```apt-get install imagemagick``` <br/>
Configure it to allow pdf export : edit ```/etc/ImageMagick-7/policy.xml``` : <br/>
```<policy domain="coder" rights="read | write" pattern="PDF" />```

You can then merge files into a single pdf :
```shell script
convert *.tiff result.pdf
convert something*.pdf someresult.pdf
```

## Merge pdf documents with Pdftk 

Merging with Image Magick may end with a lost of quality. Using Pdftk will provide better results. <br/>
Install it with ```apt-get install pdftk```. <br/>
Then merge documents :
```shell script
pdftk /tmp/document_*.pdf cat output result.pdf
```

## Compress a pdf with Ghostscript

Install it with : ```apt-get install ghostscript```. <br/>
Then : 
```shell script
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```
