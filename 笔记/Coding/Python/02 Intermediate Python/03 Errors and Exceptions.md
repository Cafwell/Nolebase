## Errors
Syntax errors, also known as parsing errors, are perhaps the most common kind of complaint you get while you are still learning Python:
```Python
while True print('Hello world')
---
while True print('Hello world')
               ^^^^^
SyntaxError: invalid syntax
```
The error is detected at the function print(), since a colon (':') is missing before it.
```Python
while True: print('Hello world')
# Infinite loop
```

## Exception

### Raising exceptions
