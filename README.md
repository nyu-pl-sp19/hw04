# Homework 4 (20 Points)

The deadline for Homework 4 is Friday, March 1, 6pm. The late
submission deadline is Thursday, March 7, 6pm.

## Getting the code template

Before you perform the next steps, you first need to create your own
private copy of this git repository. To do so, click on the link
provided in the announcement of this homework assignment on
Piazza. After clicking on the link, you will receive an email from
GitHub, when your copy of the repository is ready. It will be
available at
`https://github.com/nyu-pl-sp19/hw04-<YOUR-GITHUB-USERNAME>`.  
Note that this may take a few minutes.

* Open a browser at `https://github.com/nyu-pl-sp19/hw04-<YOUR-GITHUB-USERNAME>` with your Github username inserted at the appropriate place in the URL.
* Choose a place on your computer for your homework assignments to reside and open a terminal to that location.
* Execute the following git command: <br/>
  ```git clone https://github.com/nyu-pl-sp19/hw04-<YOUR-GITHUB-USERNAME>.git```<br/>
  ```cd hw04```

Put your answers to problems 1-3 into the text file `solution.md`.
The code template for solving Problem 4 is provided in the file

```
src/main/scala/pl/hw04/hw04.scala
```

relative to the root directory of the repository. Follow the
instructions in the notes for Class 2 to import the project into
InteliJ (or use your other favorite IDE or editor to work on the assignment).


## Submitting your solution

Once you have completed the assignment, you can submit your solution
by pushing the modified files to GitHub. This can be done by
opening a terminal in the project's root directory and executing the
following commands:

```bash
git add .
git commit -m "solution"
git push
```

You can replace "solution" by a more meaningful commit message.

Refresh your browser window pointing at
```
https://github.com/nyu-pl-sp19/hw04-<YOUR-GITHUB-USERNAME>/
```
and double-check that your solution has been uploaded correctly.

You can resubmit an updated solution anytime by reexecuting the above
git commands. Though, please remember the rules for submitting
solutions after the homework deadline has passed.


## Problem 1: Activation Records (4 Points)

Consider the following code excerpt from a university system that
allows users to query the system for grade entries in a student's
grade transcript:

```c
int get_transcript_size(int student_id);
void load_transcript(int student_id, char* grades, int size);
int read_int();

char get_transcript_entry(int student_id, int entry) {
  int size = get_transcript_size(student_id);
  char grades[size];
  load_transcript(student_id, grades, size);
  return grades[entry];
}

int main() {
  int student_id = 42; // not to be revealed to the user
  int entry = read_int();
  char grade = get_transcript_entry(student_id, entry);
  printf("Requested grade: %c\n", grade);

  return 0;
}
```

The code assumes three functions for which we only provide the
signatures above:

* a function `get_transcript_size` that, given a
  student's internal ID, returns the number of entries in the
  student's grade transcript.
* a function `load_transcript` that loads the grades in
  a student's transcript from a database into a preallocated array
  `grades` of the appropriate size.
* a function `read_int` that asks the user to input
  an integer number into the system.

The system is supposed to never reveal the internal ID of the student
whose grades are queried by the user (here 42). However, the code has
a potential security problem. That is, a malicious user could
manipulate the system into revealing the internal ID of the student
via the print statement in function `main` by entering appropriate
`int` values into the system via the function `read_int`.

Use your knowledge about how the compiler-generated calling sequences
manage actual parameters and local variables of functions on the stack
to explain the problem. Assume that the functions
`get_transcript_size`, `load_transcript`, and `read_int` are all
implemented correctly. 


**Hint:** you do not have to guess the actually value that the
user needs to provide in order to retrieve the student ID as it will
be compiler/architecture specific. Instead, explain the general idea
of the exploit. Note that you are not allowed to change the
program. The only way you can interact with the system is by entering
integer values via the function `read_int`.

## Problem 2: Deep vs. Shallow Binding (6 Points)

Consider the following Scala program:

```scala
var x = 0

def set_x(v: Int): Unit {
  x = v
}

def foo(s: Int => Unit, y: Int): () => Unit = {
  var x = 0
  
  def print_x(): Unit = println(x)

  s(y)
  print_x()
  print_x
}

val p = foo(set_x, 1)
set_x(2)
p()
```

1. What does this program print with static scoping and deep binding
   semantics (i.e. Scala's standard semantics)?  Explain.
   
2. What would this program print if Scala were to use dynamic scoping
   and shallow binding semantics? Explain.

## Problem 3: Parameter Passing Modes (8 Points)

Consider the following program:

```scala
def params(x: Int, y: Int) {
  x = y + y;
  println(x);
}

var z = 1;
params(z, {z := z + 1; z});
println(z);
```

What does this program print if we make the following assumptions about
the parameter passing modes for the parameters `x` and `y` of
`params`:

1. both `x` and `y` are call-by-value parameters;
2.  `x` is call-by-reference and `y` is call-by-value;
3. `x` is call-by-value and `y` is call-by-name;
4. `x` is call-by-reference and `y` is call-by-name;

It is enough if you provide the outputs of the two `println` calls for
each variant. You don't need to explain your results.

Clarification: Scala does not actually allow assignments to parameters
and does not support parameters with call-by-reference semantics
directly. So you can't execute the above code in Scala as given. We
here consider a hypothetical language that behaves like Scala in every
way except for the above differences. In particular, the arguments
passed to parameters with passing modes following a strict evaluation
strategy are evaluated in left-to-right order at the call site.

## Problem 4: Implementing Custom Control-Flow Constructs (2 Points)

Call-by-name parameters are particularly useful when one wants to
implement custom control-flow constructs. Here, we use this idea to
extend Scala with a new loop construct

```scala
until (cond) { 
  body
}
```

This loop construct should behave like a `while` loop in Scala, except
that the loop condition is negated: the loop exits when `cond` becomes
`true` rather than `false`. That is, the above code should behave
exactly like the following while loop:

```scala
while (!cond) {
  body
}
```

We can implement `until` as a function that takes the loop condition
`cond` and the loop body `body` as call-by-name parameters:

```scala
def until(cond: => Boolean)(body: => Unit): Unit = {
  ???
}
```

Note that we here take advantage of another useful feature of Scala:
multiple parameter lists. The two parameters `cond` and `body` are
passed to `until` in separate parameter lists. This allows us to call
`until` as if it were a loop construct that was built into the Scala
language. For instance, with the correct implementation of `until`,
the following code will print all values from `1` to `10`:

```scala
var x = 0

until (x == 10) {
  x = x + 1
  println(x)
}
```

Here, the expression `x == 10` will be passed to `cond` and the block
expression `{ x = x + 1; println(x) }` will be passed to `body`.

Provide an implementation of `until` that has the desired behavior.
Do not use a while loop for the implementation of `until`. Instead,
use recursion to implement the looping behavior.

