Task: The /lucky.php page has a button that appears to be disabled. Try to enable the button, and then click it to get the flag.

Upon examining the response using zap hud, I found

```
<button class='btn block-cube block-cube-hover' id='submit' type='submit' formmethod='post' name='getflag' value='true' disabled>
```

There’s a disabled attribute in the code. Break the request and deleted the attribute. Found the flag.

## Question 2

Task: The /admin.php page uses a cookie that has been encoded multiple times. Try to decode the cookie until you get a value with 31-characters. Submit the value as the answer.

I used burp decoder as it’s easier than zap for this purpose.

`cookie=4d325268597a6b7a596a686a5a4449314d4746684f474d7859544d325a6d5a6d597a63355954453359513d3d`

I sent this to decoder. Decode with ascii hex and base 64, got `3dac93b8cd250aa8c1a36fffc79a17a` which is the correct flag.

## Question 3

Note: I chose to use zap but in this case using burp is much easier. If you don’t wanna write custom scripts just use burp.

Opened /admin.php with zap. Chose the request > attack > fuzz. I replaced the cookie value with `3dac93b8cd250aa8c1a36fffc79a17a`.

As the wordlist I used `SecLists/Fuzzing/alphanum-case.txt` and as the processing rule chose the MD5 hash as prefix. Also I added the `Base64-encode` and `Encode as ASCII hex` processing rules.
![Image: Additional External Screenshot](https://i.ibb.co/YBfrwRr9/Pasted-image-20250615145931.png)



Since ZAP doesn’t have ASCII-Hex encoder by default, I wrote this script:

```
def process(payload):
	encoded = "".join("{:02x}".format(ord(c)) for c in payload)
	return encoded
```

This code takes each character in `payload`, gets its ASCII code using `ord(c)`, formats it as a two-digit hexadecimal value with `{:02x}`, and then joins all these hex values together into one continuous string. Since Jython only supports Python 2 as of 2024, you can’t use the built-in bytes.hex() method in Python 3. If you don’t understand this for loop syntax, search for ‘generator expression’.

To use this you need to install Python scripting from Marketplace. If you use the latest version of zap, the icon with three cubes is available, which is ‘manage add-ons’.

View > Show tabs> Script tabs, add a new script. Make sure to choose ‘payload processor’ not fuzzer http processor.

And ZAP doesn’t have ‘add prefix’ processor like burp too, so you have to make a custom script for this. Hard coding the cookie value doesn’t work because you need to transform the text this order: prefix addition -> Base64 encoding -> ASCII-hex encoding.

Add this as a payload processor:

```
def process(payload):
	prefix = "3dac93b8cd250aa8c1a36fffc79a17a"
	return prefix + payload
```

After fuzzing with the correct transformation, you’ll find one response stands out as larger (~1,900 bytes). That response body contains a flag.

## Question 4

Task: You are using the ‘auxiliary/scanner/http/coldfusion_locale_traversal’ tool within Metasploit, but it is not working properly for you. You decide to capture the request sent by Metasploit so you can manually verify it and repeat it. Once you capture the request, what is the ‘XXXXX’ directory being called in ‘/XXXXX/administrator/..’?

Run msfconsole and use auliary/scanner/http/coldfusion_locale_traversal. Set the RHOST, RPORT and PROXIES.

![Image: Latest External Screenshot](https://i.ibb.co/7tZ9ydqF/Screenshot-2025-06-15-151858.png)


![Image: Final External Screenshot](https://i.ibb.co/KjHs1Sfw/Screenshot-2025-06-15-151902.png)

Dont forget to change /etc/proxychains4.conf
add
http ur proxy

```
set RHOST google.com
set RPORT 80
set PROXIES HTTP:127.0.0.1:8080
```

Make sure to intercept the request and you’ll find the flag.
