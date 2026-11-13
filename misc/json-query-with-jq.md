# Json Query with JQ

## Setup

* download script in your home folder
* edit `.bashrc` and reload it with `source .bashrc`
```bash
alias jq=/home/USER/jq-linux-amd64
```
* check it
```bash
jq --version
```

## Json Examples
* source : `fruit.json`
```json
{"fruit":{"name":"apple","color":"green","price":1.20}}
```

* source : `fruits.json`
```json
[
  {
    "name": "apple",
    "color": "green",
    "price": 1.2
  },
  {
    "name": "banana",
    "color": "yellow",
    "price": 0.5
  },
  {
    "name": "kiwi",
    "color": "green",
    "price": 1.25
  }
]
```

## Queries
 
```bash
# basic : display all
echo '{"fruit":{"name":"apple","color":"green","price":1.20}}' | jq '.'
cat ./foo.json | jq '.'
jq '.' ./foo.json
jq '.' foo.json

# get an attribute
jq '.records' foo.json

# get a subattribute
jq '.fruit.color' fruit.json

# get multiple attributes
jq '.fruit.color,.fruit.price' fruit.json

# get one attribute from an array
jq '.[] | .name' fruits.json
jq '.records' foo.json | jq '.[] | .CATEGORY'

# get second fruit name
jq '.[1].name' fruits.json

# filtering
jq '.[] | select(.color=="yellow")' fruits.json
jq '.[] | select(.color=="yellow" and .price>=0.5)' fruits.json
jq '.records | .[] | select(.ID==1476317)' foo.json
jq '.records | .[] | select(.CATEGORY=="DocumentCategory0749")' foo.json
jq '.records | .[] | select(.CA_LINK_ID=="{BD11E001-08F1-42D9-B7BB-7376FD57FCD1},")' foo.json

# get color attribute where name starts with "a" (regex)
jq '.[] | select(.name|test("^a.")) | .color' fruits.json

# get unique result when querying an array
jq 'map(.color) | unique' fruits.json
```

sources :  
https://jqlang.github.io/jq/  
https://www.baeldung.com/linux/jq-command-json