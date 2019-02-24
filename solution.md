# Solution

## Answer to Problem 1
Before executing the readInt() function, the studentId is saved into a register. After reading the entry, the get_transcript_entry function is executed, which returns grades[entry]. The value of entry variable is read from the user. If the value read is such that it is beyond the limits of the size of char array "grades", then overflow occurs and the value stored in the registers adjacent to those in the character array may be returned. In this way, the studentId could be exposed to the user.

## Answer to Problem 2
With static scoping and dynamic binding, (0, 0) is printed. When foo() is first called with arguments set_x and 1, then x is bound to the value 0 (global declaration on line # 1) at the time the closure for set_x is created. s(y) inside foo sets this global x to 1. But when print_x() is called inside foo, using the rules of static scoping, the local x bound to value 0 is used and hence the first 0 is printed. This local binding is also returned with print_x inside foo(). Subsequently, set_x(2) will set the global var x to 2, because of static scoping. When p() is executed, the print_x that was returned previously is executed. It refers to the local declaration of x that was returned inside foo, and prints 0 again. 
With dynamic scoping and shallow binding, the program would print (1, 2). When foo() is first called with arguments set_x and 1, then x is not bound to its global declaration on line # 1, because shallow binding is used. s(y) inside foo sets the local x to 1. This is printed by a call to print_x. When print_x is returned by foo, no binding is returned along with it. A consequent call to set_x(2) sets the local x to 2 when dynamic scoping is used. Therefore, p() will print 2 as the output.

## Answer to Problem 3
1. 4
   2

2. 4
   4

3. 5
   3

4. 5
   5
    

## Answer to Problem 4
def until(cond: => Boolean)(body: => Unit): Unit = {
  if (!cond) {
    body
    until(cond)(body)
  }
}
