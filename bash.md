## All about bash
 
-  all scripts start with ``` #!/bin/bash ```

#### WORKING WITH VARIABLES:

- Scalar variables can only story one value at a time
- Assignement is the same like in programming languages
- Refer to variables using $VARIABLE_NAME   
- Use the readonly keyword to make a variable readonly
- Using ``` unset ``` removes the value of the variable
- Bash special variables:
   - ```$#``` - number of commandline args
   - ```$_``` set after all the shell and contains the absolute fine name of the script being executed  as passerd in the argument list
   - ```$-``` expands to the current option flags as specified by the set build-in command set by the shell itself
   - ```$?``` exit value or status of last executed command
   - ```$``` process numer of the shell
   - ```$!``` process number of last background command
   - ```$0``` name of the script
   - ```$n``` individual args .default bash only allows 9 but if  $n usd it allows more
   - ```($*,$@)```all arguments  on command line
   - ```$*``` all args as one string
   - ``` read -p 'prompt' variable ``` takes input and assigns to a
   
   ### BASH ARRAYS: 

- Can be assigned by enclosing in paranthesis ()
- Access array elements the same way u do in  other languages

  ### OPERATORS:
- The same as in other languages

   ### FILE OPERATORS
- b checks if its a block special file
- c character special
- d directory exists
- e exists or not
- r read acccess or not
- w write or not
- x executed
- s checks size >0 true

### USERS AND GROUPS:
 - groups are a group of users 
 - add user using ``` useradd -p ``` 
 - add group using ``` groupadd -p ```
 - use id to check the group
 - sudoers users that can use sudo
 -