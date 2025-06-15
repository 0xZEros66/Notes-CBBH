

## Beautify

```javascript
eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('g 4(){0 5="6{7!}";0 1=8 a();0 2="/9.c";1.d("e",2,f);1.b(3)}', 17, 17, 'var|xhr|url|null|generateSerial|flag|HTB|flag|new|serial|XMLHttpRequest|send|php|open|POST|true|function'.split('|'), 0, {}))
```

Go 
https://prettier.io/playground/

   

![Obfuscated JavaScript code using eval and regex replacement.](https://academy.hackthebox.com/storage/modules/41/js_deobf_beautifier_1.jpg)

## Deobfuscate
https://matthewfl.com/unPacker.html
============================

## POST Request
```shell-session
curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
```

 Try applying what you learned in this section by sending a 'POST' request to '/serial.php'. What is the response you get?
 curl -s http://83.136.253.201:42030/serial.php -X POST
result : N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz
so lets decode it
echo N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz | base64 -d

curl -s http://83.136.253.201:42030/serial.php -X POST -d "serial=7h15_15_a_s3cr37_m3554g3"

result :HTB{}

============================================
# Skills Assessment

Try to study the HTML code of the webpage, and identify used JavaScript code within it. What is the name of the JavaScript file being used?

Answer: Just CTRL+U = AND U WILL FIND THE FILE NAME 

Once you find the JavaScript code, try to run it to see if it does any interesting functions. Did you get something in return?

 As you may have noticed, the JavaScript code is obfuscated. Try applying the skills you learned in this module to deobfuscate the code, and retrieve the 'flag' variable.

Answer: After you find the file name , see the  js code 

view-source:http://94.237.59.174:30158/api.min.js

`   eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('t 5(){6 7=\'1{n\'+\'8\'+\'9\'+\'a\'+\'b\'+\'c!\'+\'}\',0=d e(),2=\'/4\'+\'.g\';0[\'f\'](\'i\',2,!![]),0[\'k\'](l)}m[\'o\'](\'1{j\'+\'p\'+\'q\'+\'r\'+\'s\'+\'h\'+\'3}\');', 30, 30, 'xhr|HTB|_0x437f8b|k3y|keys|apiKeys|var|flag|3v3r_|run_0|bfu5c|473d_|c0d3|new|XMLHttpRequest|open|php|n_15_|POST||send|null|console||log|4v45c|r1p7_|3num3|r4710|function'.split('|'), 0, {}))   `
go 
https://prettier.io/playground/
after Beautify
go Deobfuscate
https://matthewfl.com/unPacker.html

so lets analyse the code
function apiKeys()
	`{`
	`var flag='HTB`
		`{`
		`n'+'3v3r_'+'run_0'+'bfu5c'+'473d_'+'c0d3!'+'`
	`}`
	`',xhr=new XMLHttpRequest(),_0x437f8b='/keys'+'.php';`
	`xhr['open']('POST',_0x437f8b,!![]),xhr['send'](null)`
`}`
`console['log']('HTB`
	`{`
	`j'+'4v45c'+'r1p7_'+'3num3'+'r4710'+'n_15_'+'k3y`
`}`
`');`

===================
part 1 :

`function apiKeys()`
	`{`
	`var flag='HTB`
		`{`
		`n'+'3v3r_'+'run_0'+'bfu5c'+'473d_'+'c0d3!'+'`
	`}`
	

we have var flag = htb
so lets get the flag

console.log('HTB{n'+'3v3r_'+'run_0'+'bfu5c'+'473d_'+'c0d3!'+'}')

so lets go to the [jsconsole](https://jsconsole.com/)
put

console.log('HTB{n'+'3v3r_'+'run_0'+'bfu5c'+'473d_'+'c0d3!'+'}')

: HTB{n3v3r_run_0bfu5c473d_c0d3!}

======================
part 2 from the code

`console['log']('HTB`
	`{`
	`j'+'4v45c'+'r1p7_'+'3num3'+'r4710'+'n_15_'+'k3y`
`}`
`');`

its like part 1 from the code

console.log('HTB{j'+'4v45c'+'r1p7_'+'3num3'+'r4710'+'n_15_'+'k3y}')

: HTB{j4v45cr1p7_3num3r4710n_15_k3y}

=====================================================

 Try to Analyze the deobfuscated JavaScript code, and understand its main functionality. Once you do, try to replicate what it's doing to get a secret key. What is the key?

Answer: 4150495f70336e5f37333537316e365f31355f66756e

we have /keys.php
but its XMLHttpRequest thats mean  create the object  to send http request 


`',xhr=new XMLHttpRequest(),_0x437f8b='/keys'+'.php';`
	`xhr['open']('POST',_0x437f8b,!![]),xhr['send'](null)`
`}`
xhr open post thats mean we opened post request for keys.php

curl http://94.237.59.174:30158 -X POST


![[Pasted image 20250615133610.png]]
================================================
Once you have the secret key, try to decide it's encoding method, and decode it. Then send a 'POST' request to the same previous page with the decoded key as "key=DECODED_KEY". What is the flag you got?

Answer : go Hashes.com
pase 4150495f70336e5f37333537316e365f31355f66756e


![[Pasted image 20250615133712.png]]
![[Pasted image 20250615133723.png]]
