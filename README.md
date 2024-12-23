# Ex-3-RECOGNITION-OF-A-VALID-ARITHMETIC-EXPRESSION-THAT-USES-OPERATOR-AND-USING-YACC
# Date:19/10/2024
# AIM
To write a yacc program to recognize a valid arithmetic expression that uses operator +,- ,* and /.
# ALGORITHM
1.	Start the program.
2.	Write a program in the vi editor and save it with .l extension.
3.	In the lex program, write the translation rules for the operators =,+,-,*,/ and for the identifier.
4.	Write a program in the vi editor and save it with .y extension.
5.	Compile the lex program with lex compiler to produce output file as lex.yy.c. eg $ flex arth.l
6.	Compile the yacc program with yacc compiler to produce output file as arth.tab.c. eg $ bison –d arth.y
7.	Compile these with the C compiler as gcc arth lex.yy.c arth.tab.c -lfl.
8.	Enter an arithmetic expression as input and the tokens are identified as output.
# PROGRAM
arth.l file
```
%{
#include "arth.tab.h" // Include the header file generated by Bison
#include <stdio.h> // Include for printf
%}

%%

"=" {
    printf("\nOperator is EQUAL");
    return '=';
}

"+" {
    printf("\nOperator is PLUS");
    return PLUS;
}

"-" {
    printf("\nOperator is MINUS");
    return MINUS;
}

"/" {
    printf("\nOperator is DIVISION");
    return DIVISION;
}

"*" {
    printf("\nOperator is MULTIPLICATION");
    return MULTIPLICATION;
}

[a-zA-Z][a-zA-Z0-9]* {
    printf("\nIdentifier is %s", yytext);
    return ID;
}

[ \t]+ {
    // Ignore whitespaces and tabs
}

. {
    printf("\nUnknown character: %c", yytext[0]);
    return yytext[0];
}

\n {
    return 0; // Return 0 for newlines to terminate parsing
}

%%

int yywrap() {
    return 1; // Return 1 to signal end of input
}
```
arth.y file
```
%{
#include <stdio.h>
#include <stdlib.h> // For exit() function
int yylex(void);
void yyerror(const char *s);
%}

/* Token declarations */
%token ID PLUS MINUS MULTIPLICATION DIVISION

%%

/* Grammar rules */
statement: ID '=' E {
    printf("\nValid arithmetic expression\n");
    $$ = $3; // Result of the expression
}
;

E: E PLUS ID {
    printf("\nAddition operation\n");
}
| E MINUS ID {
    printf("\nSubtraction operation\n");
}
| E MULTIPLICATION ID {
    printf("\nMultiplication operation\n");
}
| E DIVISION ID {
    printf("\nDivision operation\n");
}
| ID {
    printf("\nIdentifier encountered\n");
}
;

%%

/* Main function */
extern FILE* yyin;

int main() {
    yyin = stdin; // Use standard input for reading
    printf("Enter an arithmetic expression (e.g., x = y + z):\n");
    do {
        yyparse();
    } while (!feof(yyin));
    return 0;
}

/* Error handling function */
void yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
}
```

# OUTPUT
![WhatsApp Image 2024-11-26 at 15 01 50_db1d2e2d](https://github.com/user-attachments/assets/38eb7810-b926-4271-b5d4-b20f9b9fcdda)


# RESULT
A YACC program to recognize a valid arithmetic expression that uses operator +,-,* and / is executed successfully and the output is verified.
