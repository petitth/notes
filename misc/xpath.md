# XPath

## Install a tool to apply XPath expresions on xml files

### Xidel

#### Introduction
Xidel is a command line tool to download and extract data from HTML/XML pages using CSS selectors, XPath/XQuery 3.0, as well as querying JSON files or APIs (e.g. REST) using JSONiq.

#### Installation 

Get archive : https://www.videlibri.de/xidel.html#downloads  
After installation, open a terminal and try it : 

```bash
xidel --version
```

Try it :
```bash
# xpath
xidel books.xml --xpath "//title[@lang='fr']" --output-format=xml
# xquery
xidel test1.html --xquery '//div[@class="description"]' --output-format=xml
```

sources :  
https://www.videlibri.de/xidel.html#home  
https://github.com/benibela/xidel

## Examples of XPath calls

### Books list
```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
    <book id="1" category="linux">
        <title lang="en">Linux Device Drivers</title>
        <year>2003</year>
        <author>Jonathan Corbet</author>
        <author>Alessandro Rubini</author>
    </book>
    <book id="2" category="linux">
        <title lang="en">Understanding the Linux Kernel</title>
        <year>2005</year>
        <author>Daniel P. Bovet</author>
        <author>Marco Cesati</author>
    </book>
    <book id="3" category="novel">
        <title lang="en">A Game of Thrones</title>
        <year>2013</year>
        <author>George R. R. Martin</author>
    </book>
    <book id="4" category="novel">
        <title lang="fr">The Little Prince</title>
        <year>1990</year>
        <author>Antoine de Saint-Exupéry</author>
    </book>
</books>
```

* second book : 
```bash
//books/book[position()=2]
# short version 
//books/book[2]
```

* get book title where `lang='fr'` 
```bash
//books/book/title[@lang='fr']
# or 
//book/title[@lang='fr']
# or 
//title[@lang='fr']
```

* books released in 2005 : `//books/book[year=2005]`  

* all books titles : 
```bash 
# get each title in a tag 
//books/book/title
# get only titles
//books/book/title/text()
```

* last book title : `//books/book[last()]/title`
* book titles where year >= 2005 : `//book[year>=2005]/title/text()`

### Misc

* last name of items where country tag = 'Brazil' : `//List/item[country = 'Brazil']/lastName`
* items where country tag = 'Brazil' : `//List/item[country = 'Brazil']`
* second item where country tag = 'Brazil' : `//List/item[country = 'Brazil'][position()=2]`