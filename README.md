Download Link: https://assignmentchef.com/product/solved-cs0449-lab-2-type-sizes-and-console-i-o
<br>
<h2>Part 1: sizeof() is not how you get the length of an array</h2>

You don’t have to turn anything in for this part, but please do it. This stuff is important and I’ll expect you to know it in future labs/projects/lectures.

sizeof() is <em><strong>not</strong></em> how you get the length of an array. You cannot “get” the length of an array in C because arrays are not real.

There are two kinds of numerical types in C: integer types and floating-point types.

There are also <strong>signed</strong> integers (the default) or <strong>unsigned</strong> integers (which cannot represent negative numbers.) Java doesn’t have these unsigned integers!

Unlike Java, the sizes of the integer types in C <strong>are not fixed.</strong> They depend on the platform: what CPU and operating system you’re using.

sizeof() is <em><strong>not</strong></em> how you get the length of an array. You cannot “get” the length of an array in C because arrays are not real.

You can find out the size of a type using the sizeof operator. <em><strong>This is NOT a function.</strong></em> It operates at <strong>compile time</strong> and gives you a constant value saying how many bytes something takes up.

The sizes of the integer types in C follow these inequalities:

sizeof(char) == 1sizeof(char) &lt;= sizeof(short) &lt;= sizeof(int) &lt;= sizeof(long) &lt;= sizeof(long long)

<strong>char</strong><strong> is an integer type in C.</strong> You can use a char whenever you need a one-byte value or an array of one-byte values. Text characters are really a special case of one-byte value.

<h3>Now for practice</h3>

Make a copy of lab1.c as a starting point, and name it sizeof.c.

Inside main, put this code:

The (int) in there is to cast the result of sizeof to an int. Otherwise, you’ll get annoying warnings.

int x = 10;        printf(“sizeof(x) = %d
”, (int)sizeof(x));        printf(“sizeof(int) = %d
”, (int)sizeof(int));

Compile it like gcc -Wall -Werror –std=c99 -o sizeof sizeof.c

Run it like ./sizeof. <strong>What does it print?</strong>

Now extend it. Print the sizes of the following types:

<ul>

 <li>char</li>

 <li>short</li>

 <li>unsigned int</li>

 <li>long</li>

 <li>long long</li>

 <li>float</li>

 <li>double</li>

 <li>long double</li>

 <li>int*</li>

 <li>&amp;x (yes, write sizeof(&amp;x))</li>

 <li>int**</li>

 <li>double*</li>

 <li>char*</li>

</ul>

Make notes of what these print.

<ul>

 <li>Does unsigned change the size?</li>

 <li>What size are the pointers?</li>

 <li>Does the pointer type change the size?</li>

</ul>

<h3>Array variables are really weird</h3>

Okay: now to blow your mind some. Add this code.

char a[10];        int b[10];        int* c = b;

Now print sizeof(a), sizeof(b), and sizeof(c), sizeof(&amp;a), and sizeof(&amp;b).

What the hell is going on?

Array variables are the ones declared with brackets. C treats them very strangely. They’re kind of pointers but kind of not.

When you use sizeof() on <strong>an array variable,</strong> it tells you <strong>how many </strong><em><strong>bytes</strong></em><strong> it takes up.</strong> It does NOT tell you the length, at least not directly.

But when you use sizeof() on <strong>a pointer,</strong> it gives you <strong>the size of the pointer.</strong> It <em>never</em> gives you the length of the array that the pointer points to.

sizeof() is <em><strong>not</strong></em> how you get the length of an array. You cannot “get” the length of an array in C because arrays are not real. sizeof() is <em><strong>not</strong></em> how you get the length of an array. You cannot “get” the length of an array in C because arrays are not real. sizeof() is <em><strong>not</strong></em> how you get the length of an array. You cannot “get” the length of an array in C because arrays are not real.

<h3>Finally: 32-bit machines</h3>

Thoth is a 64-bit machine. But you can compile a 32-bit executable by using the -m32 flag to gcc, like so: gcc -Wall -Werror –std=c99 -m32 -o sizeof sizeof.c

<em>Now</em> run ./sizeof. Which numbers changed? Why do you think that is?

<h2>Part 2: Console I/O</h2>

Make a new file, lab2.c. Here’s some code to get you started.

Feel free to reuse these functions in future labs/projects.

#include &lt;stdio.h&gt;#include &lt;string.h&gt;#include &lt;ctype.h&gt; void get_line(char* buffer, int size) {        fgets(buffer, size, stdin);        int len = strlen(buffer);        // this is a little more robust than what we saw in class.        if(len != 0 &amp;&amp; buffer[len – 1] == ‘
’)               buffer[len – 1] = ‘ ’;} // returns 1 if the two strings are equal, and 0 otherwise.int streq(const char* a, const char* b) {        return strcmp(a, b) == 0;} // returns 1 if the two strings are equal ignoring case, and 0 otherwise.// so “earth” and “Earth” and “EARTH” will all be equal.int streq_nocase(const char* a, const char* b) {        // hohoho aren’t I clever        for(; *a &amp;&amp; *b; a++, b++) if(tolower(*a) != tolower(*b)) return 0;        return *a == 0 &amp;&amp; *b == 0;} int main() {         return 0;}

<h3>What your program will do</h3>

This program will calculate how much the user weighs on various planets in our solar system. Here’s how your program will work:

<ol>

 <li>Ask them what planet they want to visit.</li>

 <li>If they typed exit, use break; to exit the loop.</li>

 <li>If they typed earth, call them silly or something.</li>

 <li>Otherwise,

  <ol>

   <li>Get the scaled weight for that planet using your planet_to_weight function (see below).</li>

   <li>If it returned a value less than 0, that means it’s <strong>not a planet,</strong> so say so.</li>

   <li>Otherwise, tell them how much they’d weigh there.

    <ul>

     <li>“%.2f” would be a nice format.</li>

    </ul></li>

  </ol></li>

 <li>Go back to step 1.</li>

</ol>

Here’s how it looks when I interact with my program:

[thoth ~/private/cs0449/lab2]: ./lab2Uh, how much do you weigh? 250What planet do you wanna go to (‘exit’ to exit)? marsYou’d weigh 95.00 there.What planet do you wanna go to (‘exit’ to exit)? JUPITERYou’d weigh 635.00 there.What planet do you wanna go to (‘exit’ to exit)? plutoThat’s not a planet.What planet do you wanna go to (‘exit’ to exit)? earthuh, you’re already there, buddyWhat planet do you wanna go to (‘exit’ to exit)? exit[thoth ~/private/cs0449/lab2]:

<h3>Reading a number</h3>

First we’ll read a number, the user’s weight (sorry if that’s sensitive info…).

<ol>

 <li>Use the get_line function to ask the user for their weight.</li>

</ol>

2.   printf(“How much do you weigh? “);3.   char input[100];4.   get_line(input, sizeof(input)); // notice the sizeof!

We used sizeof(input) so we don’t have to repeat the 100. Also, that avoids mistakes if you change the size of the input array.

<ol start="5">

 <li>Use sscanf to parse the number out of the string. You use it like this:</li>

</ol>

6.   int weight;7.   sscanf(input, “%d”, &amp;weight); // DON’T FORGET THE &amp; or it’ll crash.

sscanf means string scan formatted. It’s like printf backwards. It can parse values out of a string.

How this works is by <strong>handing off a pointer to the </strong><strong>weight</strong><strong> variable</strong> to sscanf. Then it looks in the input string for an integer (the “%d” tells it to do that), and it puts that value into weight indirectly.

<strong>Test it out, see if it works.</strong> Never write a whole program at once. Compile early, compile often. Print out the weight variable to see if it parsed correctly.

<h3>Making a function</h3>

The const means “I only want to read the string from this variable, I promise I won’t change it.” We’ll talk about it later in the term.

<strong>Before </strong><strong>main</strong><strong>,</strong> make a function with this signature:

float weight_on_planet(const char* planet_name, int user_weight)

This function takes the name of a planet and a weight, and returns:

<ul>

 <li><strong>what you’d weigh</strong> on that planet, or</li>

 <li><strong>-1 if it’s not a planet.</strong>

  <ul>

   <li>Like Pluto.

    <ul>

     <li>Which is not a planet.

      <ul>

       <li>It’s a dwarf planet.

        <ul>

         <li>It’s a Kuiper Belt Object.

          <ul>

           <li>It’s not a planet.

            <ul>

             <li>Okay?

              <ul>

               <li>Okay.</li>

              </ul></li>

            </ul></li>

          </ul></li>

        </ul></li>

      </ul></li>

    </ul></li>

  </ul></li>

</ul>

Here is a table of relative gravity strengths on the seven non-earth planets in our solar system:

<table>

 <thead>

  <tr>

   <td><strong>Planet</strong></td>

   <td><strong>Gravity</strong></td>

  </tr>

 </thead>

 <tbody>

  <tr>

   <td>Mercury</td>

   <td>0.38</td>

  </tr>

  <tr>

   <td>Venus</td>

   <td>0.91</td>

  </tr>

  <tr>

   <td>Mars</td>

   <td>0.38</td>

  </tr>

  <tr>

   <td>Jupiter</td>

   <td>2.54</td>

  </tr>

  <tr>

   <td>Saturn</td>

   <td>1.08</td>

  </tr>

  <tr>

   <td>Uranus</td>

   <td>0.91</td>

  </tr>

  <tr>

   <td>Neptune</td>

   <td>1.19</td>

  </tr>

 </tbody>

</table>

Use streq_nocase to check which planet it is, e.g.

if(streq_nocase(planet, “mars”)) {        // it’s mars.}

The “case-insensitivity” of this function means they can type e.g. “mars”, “Mars”, “MARS” and it’ll all work the same way.

Remember, if the planet is not on this list, return -1.

TEST IT OUT. Call it from main with a few values and see what it returns. See if it behaves how you expect. Test, test, test. Testing your own code BEFORE you use it will save you so much trouble.

<h3>Loopydoop</h3>

Make an infinite loop. true is not a thing in C by default, so you can write while(1) to make an infinite loop.

In that loop, you’ll be <strong>reading a line of input from the user</strong> and then <strong>using </strong><strong>streq_nocase</strong><strong> to see what they typed in.</strong> You’ll have to check for “exit” and “earth” specifically. Then, if it’s neither of those, use weight_on_planet. <strong>Go look at the program description above!</strong>