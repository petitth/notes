# Postman

## Introduction

Postman is an API platform for building and using APIs.  
Postman simplifies each step of the API lifecycle and streamlines collaboration so you can create better APIs—faster.  
link : https://www.postman.com

## Misc

### Working directory

Default working directory, used by Postman is `~/Postman/files`

## Testing examples

### Basic validation

Response is `I am alive` text with 200 return code.  
The following example will have 3 tests :

```javascript
console.log("raw response : body='" + responseBody + "' - code = '" + responseCode.code + "'");
pm.test('response code 200 expected', function () {
    pm.response.to.have.status(200);
});

if (responseCode.code === 200) {
    console.log("response code is 200");
    tests["calling isAlive is success"] = pm.response.text() === 'I am alive';
}

pm.test("check result", function () {
    var result = pm.response.text();
    //console.log("isAlive result : " + result);
    pm.expect(result).to.not.be.null;
    pm.expect(result).to.eql("I am alive");
    pm.expect(result, "unexpected is alive result: '" + result + "'").to.eql("I am alive");
});
```

### Basic json

Response is `{"isAlive": true}` json with 200 return code :

```javascript
pm.test("check result", function () {
    var result = pm.response.json();
    //console.log("isAlive result : " + JSON.stringify(result));
    pm.expect(result).to.not.be.null;
    pm.expect(result.isAlive).to.eql(true);
});
```

### Json array

```javascript
pm.test("check array result", function () {
    var result = pm.response.json();
    pm.expect(result).to.not.be.null;
    pm.expect(result).to.have.lengthOf(91);
    pm.expect(result.length).to.be.above(0);
    pm.expect(result).to.include("red");
    pm.expect(result).to.include("green");
    pm.expect(result).to.include("blue");
});
```

### Session variables

```javascript
    pm.variables.unset("total");
    pm.variables.set("total", jsonData.total);
    console.log("total : " + pm.variables.get("total"));
```

### If

```javascript
if (responseCode.code === 200) {
    console.log("response code is 200");
}
```

### Loop through results

```javascript
    for (var i=0; i<jsonData.results.length; i++) {
        console.log("(" + i + ") " + jsonData.results[i].foo);
        pm.expect(jsonData.results[i].foo).to.not.be.null;
    }
```

### Xml result

The best way is to convert the result to json, then check its content :

```javascript
pm.test("check result", function () {
    var result = xml2Json(responseBody);
    //console.log("json : " +  JSON.stringify(result) );
    pm.expect(result).to.not.be.null;
    pm.expect(result.List).to.not.be.null;
    pm.expect(result.List.item).to.not.be.null;
    pm.expect(result.List.item.length).to.be.above(0);
});
```

### Misc

```javascript
    pm.expect(jsonResult.someNumber).to.eql(1);
    pm.expect(jsonResult.someNumber).gt(0);
```

# Newman

## Introduction

Newman is a command-line Collection Runner for Postman.   
It enables you to run and test a Postman Collection directly from the command line.  
It's built with extensibility in mind so that you can integrate it with your continuous integration servers and build systems.

## Install Newman

```bash
# install npm (if not done yet)
sudo apt install nodejs
sudo apt install npm
nodejs --version
npm --version 

# install newman (if not done yet) [as root]
npm install -g newman
```

## Launch it

```bash
newman run spring-boot-rest-sandbox.postman_collection.json
newman run spring-boot-rest-sandbox.postman_collection.json --environment=local-springboot.postman_environment.json --delay-request=500
```