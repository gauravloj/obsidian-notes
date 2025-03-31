Everyday script commands

1. info bash
2. declare -x (-l for lowercase, -u for uppercase, -r for readonly, -a for array, -A for map)
3. https://grml.org/zsh/zsh-lovers.html
4. enable : list of shell builtins
5. compgen -k : list of shell keywords
6. typeset : declaring a variable as local (-i for integer, )
7. $(<filename) : output file content
8. x=abc; abc=def; echo ${!x} -> variable indirection using !
9. Variable operators
    1. ${var:-defaultvalue} -> if var is unset, then default value is returned. Note ‘:-‘ is the operator used
    2. ${var:=defaultvalue} -> if var is unset, then default value is assigned to var and then return the defaultvalue. Note ‘:=‘ is the operator used
    3. ${var:?} -> if var is unset, display error and exit
    4. ${var:+} -> return nothing if unset, else return value
    5. ${var:<num>} -> return the string starting from the ‘num’ index
    6. ${var:<numstart>:<requiredlength>} -> return the string starting from the ‘numstart’ index and ‘requiredlength’ characters
    7. ${#var} -> length of the value of variable var
    8. ${var#pre}  -> remove prefix from the value and return
    9. ${var%post}  -> remove suffix from the value and return
    10. ${var/old/new} -> replace first occurance of old with new in the variable
    11. ${var//old/new} -> replace all occurance of old with new in the variable