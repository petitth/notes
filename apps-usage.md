# Applications usage

<a name="index"></a>

## Table of contents
1. [Gimp](#gimp)
2. [Pdf handling](#pdf)
3. [Markdown files](#markdown)
4. [Video editing](#video-editing)
5. [Audio handling](#audio)
6. [LibreOffice Calc](#calc)
7. [Chrome](#chrome)


<a name="gimp"></a>
## Gimp

### Draw rectangle on selection

- use "rectangle select tool"
- select the color you want to use
- then go to ```edit | stroke selection```

<a href="#index">back to index</a>

### Replace a color by transparent

- go to `Colors` menu, and select `Color to Alpha`
- select color to replace in next screen

<a href="#index">back to index</a>

<a name="pdf"></a>
## Pdf handling

### Merge documents with command lines

```bash
# install tool on debian (if not present) 
apt-get install pdftk
# concat multiple pdf documents with same prefix
pdftk ./file-*.pdf cat output ./result.pdf
# concat 2 pdf documents
pdftk ./file1.pdf ./file2.pdf cat output result.pdf
```

### Extract pictures from documents

```bash
# install tool on debian (if not present)
apt-get install poppler-utils

# pdfimages <input.pdf> <output-prefix>
pdfimages example.pdf some_images
```

<a href="#index">back to index</a>


### Split an A3 document into A4 documents

```bash
# install tool on debian (if not present)
apt install mupdf-tools

# split a3 into a4 documents
mutool poster -x 2 a3-input.pdf a4-output.pdf

# Change -x to -y if the cut shouldn't be vertical (as in, turn it into a left-half and right-half), but horizontal (create a page for the top-half and one for the bottom-half).
```
source : https://askubuntu.com/questions/697091/divide-a-a3-document-in-two-a4-documents

<a href="#index">back to index</a>


<a name="markdown"></a>
## Markdown files

### Convert markdown to pdf with command lines

```bash
# install tool on debian (if not present)
apt-get install pandoc texlive-latex-base texlive-fonts-recommended texlive-extra-utils texlive-latex-extra
# convert markdown (or txt) to pdf
pandoc document.md -o document.pdf
```

<a href="#index">back to index</a>

<a name="video-editing"></a>
## Video editing

### Rotate a video with command lines

```bash
ffmpeg -i in.mov -vf "transpose=1" out.mov
```

For the transpose parameter you can pass:
```text
0 = 90CounterCLockwise and Vertical Flip (default)
1 = 90Clockwise
2 = 90CounterClockwise
3 = 90Clockwise and Vertical Flip
```

source :  
https://askubuntu.com/questions/83711/how-can-i-rotate-a-video

### Compress a video

```bash
# best
ffmpeg -i input.mp4  -vcodec libx265 -crf 28 output.mp4
# variants 
ffmpeg -i input.mp4 -vcodec h264 -acodec mp2 out.mp4
ffmpeg -i input.mp4 -vcodec h264 -acodec mp3 out.mp4
```

### Convert a video

```bash
# 2160p to 1080p 
ffmpeg -i input_video.mp4 -vf "scale=1920:1080" -c:v libx264 -crf 23 -c:a copy output_1080p.mp4

# mov to mp4
ffmpeg -i input.mov -qscale 0 output.mp4
```

<a href="#index">back to index</a>

<a name="audio"></a>
## Audio handling

### Rip a cd with Asunder

```bash
# install it
apt-get install asunder lame
# launch it from terminal
asunder 
```

### Convert mp4 to mp3

```bash
# install sound converter
apt-get install soundconverter
```

<a href="#index">back to index</a>

<a name="calc"></a>
## LibreOffice Calc

### Create list

- create a dedicated sheet for lists, and put values of one list in one column
- select the cells of the list to create, then go to ```menu | Data | Define Range```
- when the new window pops up, enter a descriptive name in the text box

- now go to the sheet you want to use the list
- select the cells to apply the list
- go to ```menu | Data | Validity```
- in the ```Allow``` drop-down, select ```Cell Range``` and then type the name of the Cell Range you just defined
- click ```OK``` to apply

source : https://www.techrepublic.com/blog/diy-it-guy/diy-add-drop-down-lists-to-libreoffice-spreadsheets/

<a href="#index">back to index</a>

### Use a formula from one cell on following lines

- write the formula in the cell (or an expression using cell references)
```text
="select name from person where id=" & A10 
```
- click on the cell with the formula
- scroll to the last element in the column
- click on the last row in this column while holding `shift` button, so that the column becomes highlighted, while the first row in the selection contains the formula
- click `CTRL+D` and the formula will be spread for the whole selection

source : https://stackoverflow.com/questions/26868186/quickest-way-to-apply-a-formula-to-an-entire-column

<a href="#index">back to index</a>

### Exemples of formulas

- basic sum : make the sum of cells A1 to A100 <br/>
  ```SUM(A1:A100)```

- sumif : for first 100 cells, make the sum of A cells where B cell has "apple" value <br/>
  ```SUMIF(B1:B100,"apple",A1:A100)```
- if : ```IF(Test, Then Value, Otherwise Value)``` <br/>
  example ```IF(A2>500,"High","Low")```

<a href="#index">back to index</a>

### Rotate text

- Select the cells whose text you want to rotate.
- Choose `Format - Cells`. You will see the `Format Cells` dialog.
- Click the `Alignment` tab.
- In the `Text orientation` area use the mouse to select in the preview wheel the direction in which the text is to be rotated. Click `OK`.

soure :
https://help.libreoffice.org/6.2/en-US/text/scalc/guide/text_rotate.html

<a href="#index">back to index</a>

### Freeze cells

`menu | View | Freeze Cells | Freeze First Column`

<a href="#index">back to index</a>


<a name="chrome"></a>
## Chrome

* hide network timeline : click on `network settings` icon (on the right) then untick `show overview`

* network : Chrome uses system settings

<a href="#index">back to index</a>