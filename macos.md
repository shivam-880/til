# Pretty print Json
1. Add following to your `.zshrc`:
  ```bash
  alias prettyjson='python -m json.tool'
  
  prettyjson_s() {
      echo "$1" | python -m json.tool
  }
  
  prettyjson_f() {
      python -m json.tool "$1"
  }
  
  prettyjson_w() {
      curl "$1" | python -m json.tool
  }
  ```

2. Usage:
 ```bash
 curl http://localhost:10102 | prettyjson
 prettyjson_s '{"name": "John", "age": 30}'
 prettyjson_f file.json
 prettyjson_w https://api.example.com/data
 ```

# Run VSCode from Mac Terminal
**Refer**: https://stackoverflow.com/a/36882426/1879109
