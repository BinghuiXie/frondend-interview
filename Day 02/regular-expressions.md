.       - Any Character except a new line  
\d      - Digit (0-9)  
\D      - Not a Digit  
\w      - Word Character (a-z, A-Z, 0-9, _)  
\W      - Not a word Character  
\s      - whitespace ( space, tab, newline )  
\S      - Not Whitespace  

match invisible positions before or after characters
\b      - word boundary  
\B      - Not a word boundary  
^       - beginning of a string  
$       - end of a string    

[]      - match one of the characters in the square brackets  
[^]     - match characters not in the brackets  
()      - match a group of characters  
|       - either or  

Quantifiers:  
\*       - 0 or more  
\+       - 1 or more  
?        - 0 or one  
{3}      - exact number  
{3,4}    - range of number, { min, max }  
