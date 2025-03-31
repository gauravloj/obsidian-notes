
Everyday vim commands

  
- without plugin: [https://youtu.be/XA2WjJbmmoM](https://youtu.be/XA2WjJbmmoM)
- shell from vi: [https://blog.sanctum.geek.nz/shell-from-vi/](https://blog.sanctum.geek.nz/shell-from-vi/)
- thoughtbot vim meetup: [https://www.youtube.com/watch?v=XA2WjJbmmoM&list=PL8tzorAO7s0jy7DQ3Q0FwF3BnXGQnDirs](https://www.youtube.com/watch?v=XA2WjJbmmoM&list=PL8tzorAO7s0jy7DQ3Q0FwF3BnXGQnDirs)
- vimconf 2022: [https://www.youtube.com/watch?v=z-k9Chwt0Co&list=PLcTu2VkAIIWzH2-dUu1oxucXenDzzcn_q](https://www.youtube.com/watch?v=z-k9Chwt0Co&list=PLcTu2VkAIIWzH2-dUu1oxucXenDzzcn_q)
- vimconf 2023: [https://www.youtube.com/watch?v=pmLn5pFu27k&list=PLhlaLyAlbLlr-usEauWLPy4O2ggAvZuKl](https://www.youtube.com/watch?v=pmLn5pFu27k&list=PLhlaLyAlbLlr-usEauWLPy4O2ggAvZuKl)

 1. preview window
 2. vim modeline
 3. = : indent code
 4. format options
3. [{, ]{ : find previous/next unmatched {
4. [count] [[ : Move backward to the preceding { in column 1. 
5. [count] [] : Move backward to the preceding } in column 1. 
6. [count] ]] : Move forward to the next { in column 1.
7. [count] ][ : Move forward to the next } in column 1. 
8. [/ , [* : move backward to the start of the first C comment it can find.
9. ]/ , ]* : move you forward to the end of the next C comment it can find. 
10. :set path ^= ../path/to/add : add path to vim discoverable path
11. :checkpath : check if included files are discoverable
12. [i , ]j, : search first/next occurance of the word under cursor
13. [I , ]I : lists all lines/(lines after current cursor location) which contain the keyword under the cursor. 
14. “register]p|P : insert the register content with automatic indentation
15. folding

16. ex commands

17. put vim in background -> fg

18. map unused commands to useful action - like enter, copy clipboard, s key, 

19. g modifier:

    1. gI - insert at the first column of line

    2. gJ - join without space

    3. gr, gR - replace the virtual character (maintain tab indentation)

    4. g~<motion> - change case based on motion command

    5. g~~, g~g~ - change case for entire line

    6. <count>gUU - change to upper case for count lines

    7. gU<motion> - change to upper case based on motion

    8. g*, g# - substring search for current word

    9. gp, gP : move cursor to end of the inserted text 

    10. ga - get ascii

    11. <count>go, g<ctrl-g> : count character, byte status

    12. <count>gs : cleep for count sec

    13. g0, g^, g$, gj, gk, gm - screen line instead of actual line

    14. gv - reselect the last visual selection

    15. gq : format the selected visual block

    16. gq{motion} : format the text object

    17. g? : encode with rot13 cipher

    18.  

20. z modifier:

    1. z<height> : resize terminal to <height> lines

    2. zt, zb, z. - adjust view without changing cursor columm]

    3. zh, zl, : screen character motion command

    4. zL, zR : screen scroll commands

21. K - man pages

22. ctrl-k char1 char 2 -> diagraphs

23. Searching

    1. /<searchterm>/<offset> - search and move cursor to <offset> lines ahead. Eg. /norabi/2 , ?norabi?2

    2. possible offset values -> 

    3. positive/negative integers, 

    4. b<number> - same as s

    5. s<number> - first letter of the match

    6. e<number> - last letter of the match

    7. //<offset> : repeat the search with new offset

    8. \/ : Search forward for the last pattern used.

    9. \? : Search backward for the last pattern used.

    10. \& : Search forward for the pattern last used as substitute pattern. 

    11. /first//second/ : finds the string first and then searches for the string second. 

    12. 7/term : start searching ‘term’ from line 7

    13. 

26. Regular expressions

    1. \< , \> : word boundary

    2. \+ : one or more of previous atom

    3. \= : zero or one occurance

    4. \a - alphabets, \d - digits, 

    5. [<list of chars>] : range of characters

    6. character classes

    7. \{min, max} : repetation count for last atom, negative number will turn off the greedy search

    8.  \(, \) : grouping

    9. \| : or operator

27. Marks

    1. [ , ] - beginning and end of last inserted text

    2. “ - last cursor location before previous exit

28. Registers

    1. “ : unnamed register

    2. 0 : The last yanked text

    3. - : The last small delete

    4. . :  The last inserted text

    5. % : The name of the current file

    6. # : The name of the alternate file

    7. / : The last search string

    8. : : The last “:” command

    9. _ : The black hole

    10. = : An expression 

    11. * : The text selected with the mouse

29. Insert mode commands:

    1. ctrl+u - delete line

    2. ctrl+w - delete word before cursor

    3. ctrl+<left> , ctrl+<right> : word back and forth

    4. ctrl+a : insert again the last inserted text

    5. ctrl+v<number> : insert ascii denoted by number

    6. ctrl+v : escape the next character

    7. ctrl+y, ctrl+e : insert the character from above/below the cursor  line

    8. ctrl+r <register> : insert the register content

    9. ctrl-d - unindent | 0<ctrl-d> - reset indentation | ^<ctrl-d> - move to first column for only one line

    10. ctrl-t : insert indentation 

    11. <ctrl-r><ctrl-p> register : let vim indent the inserted content 

    12. <ctrl-r><ctrl-o> register : disable auto-indent on insert          

30. window management

    1. <ctrl-w>t : top window

    2. <ctrl-w>b : bottom window

    3. <ctrl-w>p : previous window

    4. <ctrl-w>w : next window with wrap around

    5. <ctrl-w>W : previous window with wrap around

    6. <ctrl-w>r : rotate window downwards

    7. <ctrl-w>R : rotate window upwards

    8. <ctrl-w>x : exchange current window with next window

    9. <ctrl-w><ctrl-i> : Split the window with current word as search term

    10. :mksession filename -> save current session.  check ’sessionoptions’  flag

31. Visual mode

    1. aw : around word

    2. as : around sentence

    3. ap : around paragraph

    4. o : move between two ends of the selection (other end)

    5. O : move between two ends of the selection in block visual (other end)

    6. ~ : toggle case

    7. u : lower case

    8. U : upper case

    9. J : join selected lines

    10. gq : format the block

    11. gh, gH, g<ctrl-h> : Select mode

    12. <ctrl-g> : toggle select mode to visual mode

    13. <ctrl-v> : from select mode, switch to visual mode for one command

32. Command mode:

    1. range delete register count : delete <range> lines (count specifies the number of lines)

    2. :1,5 delete | 1,5 d : delete lines 1-5

    3. <num>:  - start command mode with range current to next ‘num-1’ lines

    4. :range copy address  - copy ‘range’ lines after the line specified by address

    5. :1,3 copy 4 - copy lines 1-3 after 4th line

    6. :range move address  - move ‘range’ lines after the line specified by address

    7. :1,3 move 4 - move lines 1-3 after 4th line

    8. :<linenum>append - to append text (similar to insert mode) after linenum

    9. :<linenum>insert - to append text (similar to insert mode) before linenum

    10. :<range> print - print the ‘range’ lines

    11. :<range> # - print the ‘range’ lines along with line numbers

    12. :<range> list - print the ‘range’ lines along with invisible characters

    13. :<linenum> z <count> - print lines starting from ‘linenum’ to ‘linenum + count’

    14. :range substitute /from/to/flags count  : substitute command

    15. :range snomagic /from/to/flags count : force disable the magic characters

    16. :range smagic /from/to/flags count : force the magic characters

    17. :range & flags count : repeat previous substitution with new flag and count

    18. :range~ flags count : user last searched (using / or ?) pattern as old text and keep the new text same os previous substitute

    19. :range global /pattern/ command - execute ‘command’ on ‘range’ lines matched by ‘pattern’ 

    20. :range vglobal /pattern/ command - execute ‘command’ on ‘range’ lines not matched by ‘pattern’ 

    21. :range global! /pattern/ command - same as vglobal

    22. :ilist /pattern/   - list all the lines containing ‘pattern’

    23. :isearch /pattern/   - list first line containing ‘pattern’

    24. :cd <path> - change to ‘path’ directory

    25. :cd - :- change to previous directory

    26. :pwd - same as shell pwd

    27. :file - print current file and line info

    28. :file <filename> - start editing current file as ‘filename’

    29. :=. - print current line number

    30. :write >> ‘filename’ - append current file content to ‘filename’

    31. :write !<shell command> - send current file content as input to ‘shell command’

    32. :update - update only if current buffer is modified

    33. :line read file  - read the content of ‘file’ and insert it after line number ‘line’

    34. :line read !<shellcommand>  - read the output of ‘shellcommand’ and insert it after line number ‘line’

    35. :line @register - execute ‘register’ macro on line ‘line’

    36. :range > - shift ‘range’ line to the right

    37. :range < - shift ‘range’ line to the left

    38. :change - deletes the text and enter insert mode (insert mdoe for command line, different form vim insert mode)

    39. :startinsert - same as pressing ‘i’ in normal mode

    40. :join range - join ‘range’ lines

    41. :range yank register - yank ‘range’ lines to register

    42. :linenum put register : insert the content of ‘register’ after line ‘linenum’

    43. :linenum put! register : insert the content of ‘register’ before line ‘linenum’

    44. :undo , :redo

    45. :linenum mark {register} - mark ‘linenum’ with ‘register’

    46. :linenum k{register} - same as above

    47. :!cmd - run shell command

    48. :!! - run previous shell command

    49. :shell - opens the shell terminal

    50. :history - print command mode history

    51. :history <code> range : show filtered history based on code. Codes are

        1. c, cmd, :

        2. s, search, /

        3. e, expr, =

        4. i, input, @

        5. a, all 

    52. :messages - show previous error messages

    53. :redir > file - redirect command output to file

    54. :redir >> file - append command output to file

    55. :redir END - stop redirecting

    56. :normal <commands> - execute vim normal mode ‘commands’

    57. :range exit <filename> - writes only if file has changed

33. functions and expressions:

    1. :let <var> = <expression>  - create a new variable

    2. Vim variables : <all uppercase>, <initial uppercase>, <all/partial lower case>, $environmentvar, @register, &optonname, b:buffervariable, w:window, g:global, a:funcarg , v:viminternal

    3. Arithmatic operator: +, -, *, / , %, -

    4. comparison operator - ==, !=, <, <=, >, >=

    5. Extra String comparison - 

        1. =~ , !~  : match regex

        2. ==?, !=?, <?, <=?, >?, >=? : force ignore case 

        3. ==#, !=#, <#, <=#, >#, >=# : force case match 

    6. :unlet[!].  - delete a variable

    7. Special filename variables:

        1. % - current filename

        2. # - alternate filename

        3. <cword> : current word under cursor

        4. <cWORD> : current WORD under cursor

        5. <cfile> : filename under the cursor

        6. <afile> : filename used during execution of related autocommand

        7. <abuf> : current buf name in related autocommand

        8. <amatch> : like abuf, but it is filetype or syntax name

        9. <sfile> : sourced filename

    8. filename modifiers:

        1. :p - full file path

        2. :~ - convert absolute path to shorter version

        3. :. - convert to relative path

        4. :h - head of filename. (dirname equivalent)

        5. :t - tail of filename. (basename equivalent)

        6. :r - filename without extension

        7. :e - file extension

        8. :s?from?to?  - substitute in filename

        9. :gs?from?to? - global substitute

    9. expand(‘expression’) - evaluate the vim ‘expression’

        1. :echo expand(‘<cword>:p’) - will print the absolute path for current word as filename

        2. :echo “string”

        3. :echo <optionname>

        4. :echon - doesn’t append new line in the end

        5. :echohl <groupname> - activate the highlight syntax for ‘groupname’

        6. :echohl None - deactivate any highlight syntax 

    10. :highlight - check the highlight groups  
   13. :execute “string” - evaluate string as a command mode command

    15. :[range] call {function}([parameters]) - call the ‘function’ for each range line

    17. :function {name}() range - define a range function with a:firstline, a:lastline as pre-defined parameters

    18. :function {name}() abort - abort on first error

    19. :function - list of function

    20. :function <name> - see function definition

    21. :delfunction <name> - delete function ‘name’

    22. :command <commandname> <series of commands> - user-defined commands for ‘command-mode’

    23. :command - list all custom commands

    24. :delcommand - delete

    25. :command <command name> -nargs=<arg code> <commands> . arg codes are

        1. -nargs=0 - no argument

        2. -nargs=1 - 1 argument

        3. -nargs=* - zero or more

        4. -nargs=? - zero or one

        5. -nargs=+  - one or more

        6. -range

        7. -count

        8. -register

        9. -bang

    26.