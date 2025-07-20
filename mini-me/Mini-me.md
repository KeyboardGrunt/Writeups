# DUCTF/web Mini-me (easy)

Mini-me provides you with a login form at first glance, and upon submitting any credentials at all you will be taken to a brainrot page. If we check the source of the login page we can see a reference to a JavaScript file.
![alt text](https://github.com/KeyboardGrunt/Writeups/blob/main/mini-me/Pasted%20image%2020250718193536.png "web source")

At the very end of this JS file we can see a comment referring to a "test-main.min.js.map" file.
![alt text](https://github.com/KeyboardGrunt/Writeups/blob/main/mini-me/Pasted%20image%2020250718193658.png "test file reference")

This file has a section in the "sourcesContent" key which looks like this when formatted a bit:
![alt text](https://github.com/KeyboardGrunt/Writeups/blob/main/mini-me/Pasted%20image%2020250718194127.png "test file content")
First, a bunch of jumbled variable names correspond to ASCII characters. Then they are XORed with an index+1 function. If we take the individual characters and translate their ASCII value we get the following string:

> UWMC(RRFN'_YCI\"DD\\T9FW_MK

We can perform the same XOR function on the code again to get the original value:

```
const obfuscated = "UWMC(RRFN'_YCI\"DD\\T9FW_MK";

const result = obfuscated.split('').map((ch, i) => 
    String.fromCharCode(ch.charCodeAt(0) ^ (i + 1))
).join('');

console.log(result);
```
_(Shove this into the console portion of whichever browser you prefer and it will spit out the key.)_

This returns the string TUNG-TUNG-TUNG-TUNG-SAHUR.

By analyzing other parts of the original JS file that was mentioned in the source of the initial login page, we can see API references. Combined with the note in the test file "Key is now secured..." we can infer that this is an API key.

If we take a look at the Python server code attached to the challenge we can see this function at the very end:
![alt text](https://github.com/KeyboardGrunt/Writeups/blob/main/mini-me/Pasted%20image%2020250718194611.png "test file content")

Sending an API **POST** request using your preferred method (I used Burp) with the X-API-KEY header and the API key value we decoded, we receive the flag for this challenge. 
_Make sure it's a POST request and not a GET request. Otherwise you will get a 405 error and no flag._
