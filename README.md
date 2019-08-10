# Assignment 2: Shell with History - CMPT 300

In this project we develop a simple UNIX shell. The shell accepts user commands.


## Additional files
   * discussion.txt used to communicate tasks and workload


## Design Decisions & Project Issues:

### 3. Creating Child Process

#### Design Decisions:
1. Forking child
    * return child pid in order to correctly

2. Running execvp commands
    * run inside of child (check if in child if childpid == 0)
    * only wait for child if not in background

### 4. Internal Commands

#### Design Decisions:

1. Implement exit, pwd, cd
    * run all these commands if they occur before execvp

	* exit: 
		- checks for command 'exit' through read_command function 
	
		- checks if match in parent process and exits, killing all chrilden process

	* pwd: (print working drive) 
		- checks for command 'pwd' through read_command function, then checks if match in parent process

		- issues getcwd() command and writes out to terminal the current working directory

	* cd: (change directory) 
		- checks for command 'cd' through read_command function, then checks if match in parent process

		- checks if the directory exists, if not issues error mssg, else switches to that directory

	* Enter key: 
		- if enter key is only pressed in command line, sets the enter key flag to true and skips loop to the start through continue command, after resetting enter key command.


### 5. Creating a History Feature

#### Design Decisions:
1. Add command to history
    * modulus current history count by 10, in order to overwrite correct part of history array
    
2. retrieve command (copy into buffer, and tokenize)
    * Retrieve Previous Command !!:
        - use the current history count and modulus by 10 to find the command before current in history array
    * Retrieve nth Command in !n :
        - use atoi to extract the number from the input
        -modulus that number to find it in the array
    
3. printing the last ten commands to the screen.
    * find starting point by using modular division on current history number.
    * print next 10 numbers. modulus by 10 every loop incase exceeds by 10.

4. Functions:
	
	* print_history():
		- void function that is set up to print the last 10 commands over a modulus of 10, with access to 2d array of history.

	* add_history(): 
		- adds individual tokenized commands to buffer from read_command in various processes, and is called multiple times throughout the program.

### 5.2 Signals

#### Design Decisions:

  1. register a custom signal handler for the SIGINT signal:

		- utilized signal() function found on man signal, pass in type of signal, which is only SIGINT, and the function to call handle_SIGINT

  2. Have the signal handler display the previous ten user commands:

		- handle_SIGINT, simply calls print_history() which prints last 10 commands in history 2d global array, and resets a flag for SIGINT

  3. re-display the command-prompt before returning:

		- This is achieved through having a boolean type flag set to true unless SIGINT signal is detected in main, in which case the prompt reapears by continuing the main loop. 

### Testing:

	* tested using output files for 'history' and 'errors' provided by prof

	* utilized valgrind commands: 'valgrind --tool=memcheck ./shell' as recommended by T.A.

	* utilized valgrind command: 'valgrind --leak-check=yes ./shell' found on valgrind manual website.


### Project Issues:
   * Not too many issues. Used stack overflow when confused about dynamic memory allocation and pointers.
   * Occasionally confused by sample output files, but clarified on google groups.
   * unfamiliarity with forks and child processes led to reviewing notes and youtube videos
   * merge conflicts resolved with some hassle
   * clarity regularly required regarding correct exact error phrase to use in code.

## How to run tests

	* simply issue commands 'make shell' 
	* followed by './shell'

## Authors

    Sarbjot Singh - sarbjot-14{301313399}

    Gurkanwar Sidhu - gss28{301290044)



