# ReGEX

syntax

```js
/pattern/modifiers; e.g /code/i
```

sequence of characters that form a search pattern.
regex in js used with two string methods:

```
search() - uses an expression to search for a match,returns position of match
replace() - returns a modified string where the pattern is replaced.
```

```js
let text = "visit w3school"
let n = text.search("w3school")

//  returns 6
```

using replace

```js
let text = "visit facebook"
let result = text.replace(/facebook/i, "code")
```

### Regex modifiers

```
i - performs case insensitive marching.
g - perform global match
m - perform multiline matching.
```

### Regex patterns

brackets used to find a range of characters.

```
[abc] - find any characters between the brackets
[0-9] - find any of digits between brackets
(x|y) - find any of alternatives separated by |
```

#### metacharacters

have a special meaning

```
\d - digit
\w - any character in alphabet
\s - whitespace
\b find match at begining of a word like this \bHELLO or at the end HELLO\b
\uxxxx - unicode character specified by hex given
```

## quantifiers

```
n+ match any character that contains at least one n
n* - match zero or many
n? - match any string that contain zero or one occurrence of n e.g no
```

### test()

seach for occurence of pattern and returns true or false

```js
const pattern = /e/
pattern.test("The best things in life are free!")
```
```js

/hello/.test("ok this is cool")
```

### group
```
(x)
```