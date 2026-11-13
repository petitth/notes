# VI

VI is a text editor, present on almost all Linux distributions. <br/>
You can use it in a terminal to create/edit a file with ```vi filename```

## Edit mode

```
i                       inserts text before current cursor location
I                       insert text at beginning of current line
o                       creates a new line for text entry below cursor location
O                       creates a new line for text entry above cursor location
ESC                     go back to command mode
```

## Command mode
### Main commands

```
# basics
:wq!                    save and quit          
:q!                     quit without saving    
:w                      save                   
u                       cancel last command

# misc    
:set number             add line numbers
:set nonumber           remove line numbers
```

### Search / replace commands

```
/str                    search first occurrence of 'str' after cursor (press 'n' to go to next occurence)
:g/str                  search all occurrences of 'str' 
:%s/find/replace        replace all occurrences of a string
```

### Copy / paste / delete commands

```
dd                      delete line under cursor                  
dxd                     delete x lines from the one under cursor  
yy                      copy current line in the clipboard            
y                       copy selection in the clipboard               
P                       copy clipboard before current cursor location 
p                       copy clipboard after current cursor location  
```

## FAQ

### Hitting arrow keys adds characters in vi editor

install vim package : ```sudo apt-get install vim``` <br/>
source : https://askubuntu.com/questions/353911/hitting-arrow-keys-adds-characters-in-vi-editor
