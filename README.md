# 20131079 Compiler Project 1

## Main Idea of Your Solution
1. Treat **token(ID, NUM)** like a **STRING** to handle *C-style identifiers*!
2. Whenever expression is reduced, make new temp variables and duplicate them to **top of the stack($$)**
3. Print the result(3-address code) using temp variables

## Code Description
* **Assignment.l**:
  1. Ignore white-space (' ', '\t').
  2. When the token is *C-style identifier*, return ID and set **yylval.str** to token-value using **strdup()** because we treat it as a string. Note that since we use strdup(), we must deallocate tokens later due to memory leak.
  3. Likely, when the token is number, return NUM and set **yylval.str**. Note that it is a string, not an integer.
  4. Otherwise, Just deliver characters to *yacc*.

* **Assignment.y**:
  - **Declarations**
	+ Declare counter(to number temp variables) and buffer(to assign temp varialbes as a string).
	+ **%union**: just one variable to treat as a string.
	+ All terminals and non-terminals are string type.
	+ Declare associativity and precedence to handle ambiguity.

  - **Grammar rules**
	+ **S0**: To handle multiple statements(inputs)
	+ **S**:
	  1. **ID = E \n**: Print "ID = E" where E is presented as a temp variable.
	  2. **ID = E \n** and **E \n**: Deallocate token(ID) and temp variables(E).
	  3. **\n**: Just print new line to format output
	+ **E**:
	  1. **+** to **/**: Print the result of allocation of new temp variable to production head using corresponding production rule(reduction). After that, write temp variable to buffer using **sprintf()** and duplicate to top of the stack($$) using **strdup()**. Finally, deallocate already used expressions($1, $3).
      * Likely in token case, since we use strdup(), we must deallocate useless expressions after using them due to memory leak.
	  2. **UMINUS**: Similar to above using "%prec UMINUS".
	  3. **( E )**, **ID**, **NUM**: copy the value(string)

  - **Auxiliary functions**
    + Not changed (default functions)

## Detailed Algorithm
* The algorithm is already described in above.

* To sum up briefly,
  1. *Lex* tokenize the input and deliver tokens to *yacc* as a string even if token is number
  2. After reduction, make new temp variable produced by grammar rule and duplicate it to top of the stack($$)
  3. Print all the reduction result and assignment result using temp variables
  4. Finally, deallocate useless expressions whenever they are used in production

## anything else...
Someone may be unclear because of *strdup()* and *free()*.
Actually, I did too very much!

First, the reason why I use these functions is that I treat tokens as a **string**.
Because we must handle ID as a *C-style identifiers*, not just a simple character.
Also, we don't need to handle NUM as a integer because we don't do any mathematical calculation using number.
We just generate 3-address code using tokens and therefore we can treat *NUM* as a **string**.
However, string operations are little tricky in C because it is not primitive data type like char or int.
I use *strdup()* function to duplicate the value of *yytext* to *yylval.str*, and the value of buffer(temp variable) to top of the stack(**$$**).
Because *strcpy* doesn't allocate new memory and therefore make **segmentation fault** or **syntax error** in *yacc*.
Since *strdup()* dynamically allocate new memory, we must deallocate the memory when we don't need to store them anymore.
So I call *free()* function in some grammar rules to do that.

* If you have any question, contact me (kbs9409@unist.ac.kr)
