- XOR
- Symmetric ciphers
	- Byte ciphers
	- Block ciphers
		- AES ECB - each block is encrypted seaprately
		- AES CBC - Cyber block chaining, next block depends on previous encrypted block
- Asymmetric ciphers - public and private keys
	- RSA
- Hashes
- MACs - Message Authentication Code
	- HMAC: `HMAC(key, message) = hash(key + hash(key + message))`

# Attacks

- Stream cipher key reuse
- ECB block reordering
- ECB decryption
- Padding and padding oracles
- Hash length extension
	- [Skull Sec - demo](https://www.skullsecurity.org/2012/everything-you-need-to-know-about-hash-length-extension-attacks)
	- https://book.hacktricks.wiki/en/crypto-and-stego/hash-length-extension-attack.html

- Tips
	- Detect ECB mode by sending 47 'A'

Tools
- https://github.com/AonCyberLabs/PadBuster
- [Keyczar](https://github.com/google/keyczar), [Tink](https://github.com/tink-crypto/tink)
- [NaCL crypto libs](https://nacl.cr.yp.to/), https://github.com/aschoepe/nacl

Dos/donts
- Never `MAC and then encrypt`, always `encrypt and then MAC`
- use MAC instead of hashes
- Don't reuse IV (initialization vector) in AES CBC
- Never use AES ECB
- use `BCrypt`

Challenges
- https://cryptopals.com/
- 