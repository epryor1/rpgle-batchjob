# Submit a program to Batch

This is a mini tutorial for submitting a job to batch with RPGLE. Rpgle has a learning curve and I wanted to contribute to the community by including things it took me a while to work out. This technique is useful for automating tasks for later use or to let a user continue working while the program runs in the background. 

My examples will include the main program, the display, and a CL program to direct the program. Some portions will have filler code or values but will retain the functionality. This program is created with the intention of using user input for a SQL query, but I won't include that part as I consider that another topic, and this topic can be used however it works for you.

This tutorial will cover displays, CL programming, and input validation.

## RPG Code

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install foobar.

starting with the header:
```
**FREE

ctl-opt option(*nodebugio: *nounref) dftActGrp(*no) alwnull(*usrctl);
```
Variables:
```
dcl-s strDt date;
dcl-s curDt date;
dcl-s dtNum packed(8:0);

dcl-ds display;
   f3 ind pos(3);
   f9 ind pos(9);
   allowf9 ind pos(10);
   date ind pos(11);
   change ind pos(12);
end-ds;

dcl-pi pgmName;
  @mode char(3);
  @strdat char(8);
end-pi;
```
1. strDt will hold our date when converting back and forth between 
    the CL and our main program
2. we will need the current date for validation of the date input.
3. dtNum is what I will use to convert the date form a char back to a 
   number and back to a date for input in the SQL(not included).
4. our display data structure holds the indicators that communicate 
   with the display. 
5. f3 will control the program exit, f9 is used for submitting the 
   program, allowf9 is used in validation, if the user input 
   validates, then the user is allowed to press f9, date will let the 
   users know if their input is incorrect, and change will reset the 
   process if one of the screen fields is changed.
6. this section is our procedure interface. It's populated with a 
   mode. Which will drive what the program does and when. And our 
   date in character format. This is how the program gives and takes 
   parameters to communicate with the CL.

```
// *INZSR
   begsr *inzsr;
     curDt = %date;
     
     if @mode = 'STR';
       clear @strdt;
  
       open display;
       get_parameters();
       close display;

       select;
         when @mode = 'END';
           *inlr = *on;
           leavesr;
         when @mode = 'SBM';
           *inlr = *on;
           leavesr;
       endsl;
     endif;       
   endsr;
```
*INZSR is the first section of the program to run. So we use this to grab all the values we need for the program. Any declarations are also included if needed.
1. curDt is initialized with todays date.
2. when we run our program we begin with STR and that drives the 
   program forward with this select statement.
3. 



