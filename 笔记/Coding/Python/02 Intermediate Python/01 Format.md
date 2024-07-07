## f-string
Also called formatted string literals, is a new syntax of formatting string.

```Python
name = str(input("give me a name: "))
print(f'hello, my name is {name}')
---
give me a name: nice
hello, my name is nice
```

It can be used in a string, but should use a different ' '/" "
```Python
f'python is {"good"}'
or
f"python is {'good'}"
```
Definition expression in { }:
```Python
var=10
print(f'{var+1}')
---
11
```

Floating-point values are suffixed with f:
```Python
a = 3
print(f"I have £ {a:.2f}")
---
I have £ 3.00
```

### % 
The earliest format was % (the percent sign) , which it used:
```Python
name1 = 'Xiaohu'
print('Hello %s' %name1)
---
'Hello Xiaohu'
```
% s means formatted as a string, with %d (decimal integer) andn %f (floating point) commonly used.
This usage is still widely used today, but it is actually a grammar that is not advocated for use.


---

## Examples

join( ) function: Used to concatenate elements in a sequence with a specified character to generate a new string

```
str.join(sequence)
```
+ str - a string " "
+ sequence - The sequence of elements to concatenate

```Python
myTuple = ("John", "Peter", "Vicky")
x = " ".join(myTuple)
print(x)
---
John Peter Vicky

s = "what is your name"
s_list = s.split()
y = "-".join(s_list)
print(y)
---
what-is-your-name
```

### Morse code
```Python
letter_to_morse = {
    'a':'.-', 'b':'-...', 'c':'-.-.', 'd':'-..', 'e':'.', 'f':'..-.',
    'g':'--.', 'h':'....', 'i':'..', 'j':'.---', 'k':'-.-', 'l':'.-..', 'm':'--',
    'n':'-.', 'o':'---', 'p':'.--.', 'q':'--.-', 'r':'.-.', 's':'...', 't':'-',
    'u':'..-', 'v':'...-', 'w':'.--', 'x':'-..-', 'y':'-.--', 'z':'--..',
    '0':'-----', '1':'.----', '2':'..---', '3':'...--', '4':'....-',
    '5':'.....', '6':'-....', '7':'--...', '8':'---..', '9':'----.', ' ':'/'
}
message = str(input("Your message is: "))
morse = []
for letter in message:
    mc = letter_to_morse[letter]
    morse.append(mc)
morse_message = " ".join(morse)
print(f"morse cose is: {morse_message}")
---
Your message is: please help
morse cose is  .--. .-.. . .- ... . / .... . .-.. .--.
```
