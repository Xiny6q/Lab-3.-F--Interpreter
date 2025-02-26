Download link :https://programming.engineering/product/lab-3-f-interpreter/


# Lab-3.-F--Interpreter
Lab #3. F- Interpreter
General Information

Check the Assignment tab of Cyber Campus

Skeleton code (Lab3.tgz) is attached together with this slide

Submission will be accepted in the same post, too

Deadline:

Late submission deadline: (-20% penalty)

Delay penalty is applied uniformly (not problem by problem)

Please read the instructions in this slide carefully

This slide is a step-by-step tutorial for the lab

It also contains important submission guidelines

If you do not follow the guidelines, you will get penalty


Skeleton Code Structure

Copy Lab3.tgz into CSPRO server and decompress it

This course will use cspro2.sogang.ac.kr (don’t miss the 2)

Don’t decompress-and-copy; copy-and-decompress

Directory Structure of FMinus

Skeleton code structure under src/ is same as before

AST.fs: Syntax definition of the F- language

FMinus.fs: You have to implement the semantics here

Types.fs: Type definitions needed for semantics

Main.fs: Main driver code of the interpreter

Lexer.fsl, Parser.fsy: Parser (you don’t have to care)

Do NOT fix any source files other than FMinus.fs

jschoi@cspro2:~/Lab3$ cd FMinus/ jschoi@cspro2:~/Lab3/FMinus$ ls FMinus.fsproj src testcase jschoi@cspro2:~/Lab3/FMinus$ ls src

AST.fs FMinus.fs Lexer.fsl Main.fs Parser.fsy Types.fs


F- Language Syntax

Similar to the extended version of F- in our lecture slide

The whole program is an expression

→

| true | false

|

| −

| + | −

| < | > | = | <>

| if then else

| let = in

| let = in

| let rec = in

| fun →

|

Expression


F- Language Semantics

Relation ⊢ ⇓ defines the evaluation of expression

We use the same semantic domain to our lecture slide

∈=→,∈=+++


⊢ ⇓

⊢ true ⇓

⊢ false ⇓

⊢ ⇓ ( )

⊢1⇓1

⊢1⇓1

⊢2⇓2

⊢1⇓1 ⊢2⇓2

⊢ − 1 ⇓ − 1

⊢1+2⇓1+2

⊢1−2⇓1−2

⊢1⇓1

⊢2⇓2

<

⊢1⇓1

⊢2⇓2

≥

⊢1<2⇓

1

2

⊢1<2⇓

1

2

⊢1⇓1

⊢2⇓2

>

⊢1⇓1

⊢2⇓2

≤

⊢1>2⇓

1

2

⊢1>2⇓

1

2


F- Language Semantics

Relation ⊢ ⇓ defines the evaluation of expression

We use the same semantic domain to our lecture slide

∈=→,∈=+++

⊢1⇓1 ⊢2⇓2(

=

2

= ) ∨ (

=

= )

⊢1=2⇓

1

1

2

⊢1⇓1 ⊢2⇓2(

=

≠

=

)∨(

=

≠

=

)

⊢1=2⇓

1

1

2

2

1

1

2

2

⊢1⇓1 ⊢2⇓2(

=

2

= ) ∨ (

=

= )

⊢1<>2⇓

1

1

2

⊢1⇓1 ⊢2⇓2(

=

≠

=

)∨(

=

≠

=

)

⊢1<>2⇓

1

1

2

2

1

1

2

2


F- Language Semantics

Relation ⊢ ⇓ defines the evaluation of expression

We use the same semantic domain to our lecture slide

∈=→,∈=+++

⊢1⇓⊢2⇓

⊢ 1 ⇓

⊢ 3 ⇓

⊢ if 1 then 2 else 3 ⇓

⊢ if 1 then 2 else 3 ⇓

⊢1⇓1↦1⊢2⇓2

↦ ⟨ , 1, ⟩

⊢ 2 ⇓

⊢ let = 1 in 2 ⇓ 2

⊢ let = 1 in 2 ⇓

↦ ⟨ , , 1, ⟩ ⊢ 2 ⇓

⊢ let rec = 1 in 2 ⇓

⊢ fun → ⇓ ⟨ , , ⟩


F- Language Semantics

Relation ⊢ ⇓ defines the evaluation of expression

We use the same semantic domain to our lecture slide

∈=→,∈=+++

(Application of non-recursive function)

⊢1⇓ , ,′ ⊢2⇓ ′[ ↦ ]⊢ ⇓


⊢1 2⇓

(Application of recursive function)

⊢ 1 ⇓ , , , ′ ⊢ 2 ⇓ ′[ ↦ ][ ↦ ⟨ , , , ′⟩] ⊢ ⇓


⊢1 2⇓


Implementing Semantics

To complete the interpreter of F-, you must implement the semantics of F- language in FMinus.fs file

You have to implement only one function: evalExp()

Execution of program = Evaluation of the expression

Type definition of Env and Val are provided in Types.fs

If the semantics of program is not defined, your interpreter must raise UndefinedSemantics exception defined in Types.fs

let rec evalExp (exp: Exp) (env: Env) : Val =

…

This part is given for you. let run (prog: Program) : Val =

evalExp prog Map.empty


Building and Testing

In testcase directory, tc-* and ans-* files are provided

After compiling the interpreter with the dotnet build -o out command, you can run program written in F- language

The interpreter will print the evaluation result of the program


jschoi@cspro2:~/Lab3/FMinus$ cat testcase/tc-1 let x = 10 in

…

jschoi@cspro2:~/Lab3/FMinus$ dotnet build -o out

…

jschoi@cspro2:~/Lab3/FMinus$ ./out/FMinus testcase/tc-1 13

This result must match with the

content of ans-1 (expected output)


Tip: Printing AST

In F- language, you may feel confused about how the input program is parsed into AST

For example, is “f x + 1” parsed into (f x) + 1 or f (x + 1)?

You can temporarily add the following printfn() to print out the program AST (don’t forget to erase it before the submission)

let run (prog: Program) : Val =

printfn “%A” prog

evalExp prog Map.empty

jschoi@cspro2:~/Lab3/FMinus$ cat parsing-test f x + 1

jschoi@cspro2:~/Lab3/FMinus$ ./out/FMinus parsing-test Add (App (Var “f”, Var “x”), Num 1)

…


Self-Grading Script

If you think you have solved all the problems, you can run check.py as a final check

‘O‘: Correct, ‘X‘: Incorrect, ‘E‘: Unhandled exception in your code

‘C‘: Compile error, ‘T‘: Timeout (maybe infinite recursion)

If you correctly raise UndefinedSemantics exception for an invalid program, it will be graded as ‘O‘ (not ‘E‘)

If you raise UndefinedSemantics for valid program, it is ‘X‘

jschoi@cspro2:~/Lab3$ ls FMinus check.py config jschoi@cspro2:~/Lab3$ $ ./check.py [*] FMinus : OOOO


Actual Grading

I will use different test case set during the real grading

So you are encouraged to run you code with your own test cases (try to think of various inputs)

Some students ask me to provide more test cases, but it is important to practice this on your own

You will get the point based on the number of test cases that you pass

100 point in total (but recall that each lab has different weight)


Submission Guideline

You should submit only one F# source code file

FMinus.fs (from Lab3/FMinus/src/FMinus.fs)

If the submitted file fails to compile with skeleton code when I type “dotnet build“, cannot give you any point

Submission format

Upload this file directly to Cyber Campus (do not zip them)

Do not change the file name (e.g., adding any prefix or suffix)

If your submission format is wrong, you will get -20% penalty
