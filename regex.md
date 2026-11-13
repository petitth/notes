# Regex examples

- first 10 characters of each line : ```^.{10}```
- lines with numeric content :       ```[0-9]*```
- lines with only numeric content :  ```^[0-9]*$```
- lines with only numeric and/or alpha numeric content (no special characters) : ```^[a-zA-Z0-9]*$```
- lines with only numeric and alpha numeric content (no special characters) : ```^([A-Za-z]+[0-9]|[0-9]+[A-Za-z])[A-Za-z0-9]*$```
- lines ending with json : ```.*\\.json$```
