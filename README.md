Background

The Game of Life begins with a rectangular grid of cells, each of which is either “alive” or “dead”. The alive/dead state of each cell is updated according to the following rules:

    If a live cell has fewer than 2 neighbors, it dies (underpopulation).
    If a live cell has more than 3 neighbors, it dies (overpopulation).
    If a dead cell has exactly 3 neighbors, it becomes alive (reproduction).

Important: Test your understanding of the rules right now. Try to apply the rules to the following grid, where a . denotes a dead cell, and an O denotes a living cell.

......
..O...
...O..
.OOO..
......

Write down your answer, and then check against the below…






Answer:

before | after
_______________
...... | ......
..O... | ......
...O.. | .O.O..
.OOO.. | ..OO..
...... | ..O...

If you got it right, congratulations, you are ready to move on to the programming details. If not, re-read the rules until the above before/after is reconciled with your understanding.

Note also that the grid is finite, but to avoid having any walls or boundaries, we’ll glue opposite edges together to form a torus, kind of like some of those old school video games. So, each cell will have exactly 8 neighbors. To illustrate, here are the neighbors (marked with X) of the topmost, leftmost cell, (marked with an O):

OX...X
XX...X
......
......
XX...X

Thus we would have the following before/after:

before | after
_______________
...... | ...O..
...... | ......
...O.. | ......
....O. | ..O.O.
..OOO. | ...OO.

Programming Details

The directory warm-up/ has a single source file which has a boolean representation of a glider configuration for the cells hard-coded into the source (it’s the same configuration as tests/0). You have two objectives for this warm up:

    Read an integer (let’s call it n) from the command line
    Apply the Game of Life rules n times to the hard-coded grid, and print the result (as . and O characters) to stdout (e.g. using cout).

That is, if someone runs ./warmup 7, it should advance the grid 7 times, and print out the final result once.

Note that you should print the grid using the same characters as the example files I’ve given: use . (period) for dead and O (upper case letter “O”, not the digit zero!) for alive.
An incremental approach

This may seem like common sense to many of you, but it may nevertheless be worth pointing out: don’t try to do the whole thing at once!1 As a first step, make your warm-up program apply the rules just once and print the result to stdout. If you do it right, you’ll wind up with exactly the same thing as what is in tests/1.

Once you’ve got that working, try to loop the application of the rules some fixed number of times (maybe 6 or 7, which will help you test the wrap-around). Again, you can check your answer against the corresponding file in tests/ (e.g. running ./warmup 7 should produce the exact same output as what you’ll find in the file ../tests/7).

If you have trouble, perhaps consult the main readme’s hints section regarding vectors and modular arithmetic (the other hints are for later).

OK! At this point, all that remains is to read the integer from the command line.
Reading command line arguments

In most of our programs up until now, we have used a main function with the following prototype:

int main();

Notice the absence of any parameters. To get access to the full command line that was used to invoke your program, we must instead use the following prototype:

int main(int argc, char** argv);

We adopt the conventional names argc and argv for the parameters, which have a nice mnemonic: argc stands for “argument count” and argv stands for “argument values”. However, the char** datatype might look a little cryptic! Fortunately, I can give you the gist of it right now, and it won’t hurt you to leave this part a little mysterious. (If you really want to know what the char** is about, you can read ahead to l4.pdf.)

Pointer details aside, this is very straightforward: argv[i] gives you the i-th argument, and argc tells you how many arguments there are. (Perhaps think of char** argv like a vector<string> argv.)

Note: your program’s name (the compiled binary that was run) is included in the arguments at position 0.2 For example consider the following invocation of some program named hello.

$ ./hello wow yay "omg programming is so fun"

In this case, you would have the following values for argc and argv:

argc    = 4
argv[0] = ./hello
argv[1] = wow
argv[2] = yay
argv[3] = omg programming is so fun

Note: by default, the arguments are separated by spaces. If you want a single argument to contain spaces, you can surround it with quotes, as shown above.

To illustrate further, here is a very simple program that just prints all its command line arguments:

#include <stdio.h>

int main(int argc, char** argv)
{
    printf("argc\t= %i\n",argc);
    for (int i = 0; i < argc; i++) {
        printf("argv[%i]\t= %s\n",i,argv[i]);
    }
    return 0;
}

For our purposes, we will expect a command line invocation like the following:

$ ./warmup 7

This should compute 7 generations and print the result. Given what we just learned, we know that argv[1] should be 7 in this case. But there are some subtle points yet. For one, argv[1] might not be meaningful if someone runs ./warmup with no arguments. You should check for this, and perhaps just compute and print a single generation. The second issue is that argv[1] has datatype char* (a C-style string), and not int. So we have to convert it. For this, I leave you to the man page of the library function atoi, which has the following prototype:

int atoi(const char* s);

If it still isn’t clear, you can see an example usage of atoi in gol.cpp.

Note: if too many arguments are given, you can just ignore them, or print some error message and exit if you’d prefer.
Testing

If you want to be really sure you’ve got the warm-up right, I’ve left you a test script which runs your program with different inputs and compares against the files in the ../tests/ directory. To use it, run the following from the warm-up/ directory:

$ make
$ ./test-warmup.sh
