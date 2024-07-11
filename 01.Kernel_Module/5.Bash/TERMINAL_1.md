# Terminal 🖥️
# What's a terminal?
A terminal is an application in the userspace. *terminal is also a text area that the shell puts meaning to what's written on it. shell translates/understands the commands*.

# Terminal life cycle🍼
1. open terminal
2. write commands
3. execute commands
4. get result
5. close terminal

# Open terminal phase
when you open the terminal application, it runs another application called shell; shell is a child process to the terminal.
The shell read some **.files** to *get settings* and *environment variables* in its default values.

 ```mermaid
flowchart LR
subgraph Parent Process
A(Terminal)
end
subgraph Child Process
B(shell)
end
subgraph .files
C(.bashrc)
D(.profile)
E(.bashaliases)
F(others..)
end

A-->B
B-->D
D-->C
C-->E
C-->F
```

# What's a Shell? 🐚
A shell is an abstract name for a scripting language, there's three types 
1. bash.
2. Zsh.
3. PowerShell.

# Environment Variable Commands👾
-  use `env` to load environment variables.
- use `echo $$` to get process id of shell.
- use `ps -aux | grep "terminal"` to get process id of terminal.
- use `$Var=<value>` to change value of environment variable.
- use `Var=<value>` to create a new environment variable *locally*.
- use `export Var=<value>` to  create a new environment variable *globally*.
- use `unset Var` to delete environment variable.
- use `echo $SHELL` to know the type of shell.

# What makes environment variable permanent?♾️
if we used `export var=<value>` it will disappear once i close the session, if we want to make it permanent do these steps:
1. make a new env. variable 
![Pasted image 20240530183314](https://github.com/Reemaa828/Linux_11_5/assets/112731236/5cde5248-d023-40f5-9d91-80c3388d3700)
2. if you opened a new session you will not find it
![Pasted image 20240530183444](https://github.com/Reemaa828/Linux_11_5/assets/112731236/505d13fb-970a-481a-8cee-8b504c587476)
3. to make env. variable always available we have to add it in any of .files
![Pasted image 20240530185004](https://github.com/Reemaa828/Linux_11_5/assets/112731236/72ef88e0-6727-454a-bfab-19587804f5d5)
4. **voila!!** 
![Pasted image 20240530185131](https://github.com/Reemaa828/Linux_11_5/assets/112731236/6673dca0-64f6-44eb-9377-cfcc29018c6f)

# What's difference between source and a normal run?
source: it loads all the file content into parent process (terminal). 
![Pasted image 20240530195751](https://github.com/Reemaa828/Linux_11_5/assets/112731236/67be267a-8a8a-4941-9767-c4572995f40d)


