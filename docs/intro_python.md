layout: page
title: Introduction to Python
permalink: /intro_python

# Introduction to Python
Python is a high-level, interpreted programming language known for its readability and versatility. It is widely used in various fields such as web development, data analysis, artificial intelligence, scientific computing, and more. Python's simple syntax allows beginners to learn programming concepts quickly while also providing powerful libraries and frameworks for advanced users.

## Why Learn Python?
- **Easy to Learn**: Python's syntax is straightforward and resembles natural language, making it accessible for beginners.
- **Versatile**: Python can be used for web development, data science, automation, machine learning, and more.
- **Large Community**: Python has a vast and active community, providing extensive resources, libraries, and frameworks.
- **Cross-Platform**: Python runs on various operating systems, including Windows, macOS, and Linux. We're obviously focusing on Linux here since O2 runs on Linux, but the knowledge is transferrable.
- **Interpreted Language**: Python code is executed line by line, which makes debugging easier.

## Getting Started with Python
To start using Python, you need to have it installed on your system. Most Linux distributions come with Python pre-installed. 

On O2, we want to do a bit of extra legwork first, and get an interactive session going via the slurm scheduler. First, we'll log in. Use your HMS ID, or training account credentials. You can do this by running in a terminal or other similar SSH client (MobaXterm, etc.):

```bash
ssh HMSID@o2.hms.harvard.edu
```

It will prompt for your password - note that you will receive no visual feedback as you type your password, so just type it out and hit enter. Depending on the network you are on, you may also receive a 2-factor authentication prompt. Please select the method you prefer to authenticate. Once you are logged in, you can start an interactive session with the following command:

```bash
srun --pty --mem=4G --time=02:00:00 -p interactive bash
```

You can now check if Python is installed by running the following command in your terminal:

```bash
python3 --version
```

On O2, Python 3 is available by default. However, we recommend loading the python module to ensure you are not tied to the system default version (which could change with OS updates). You can load the python module by running:

```bash
module load python/[VERSION]
```

On O2, the most recent version is `3.13.1`, so you would run:

```bash
module load python/3.13.1
```

On O2, you can see which versions are available to load by running:

```bash
module spider python
```

Once you've confirmed that Python is available, you can start the Python interactive shell by typing:

```bash
python3
```

This will open the Python interpreter, where you can type and execute Python code line by line. You'll know you're in the shell when you see the `>>>` prompt. To exit the Python shell, you can type `exit()` or press `Ctrl+D`.

This workshop will take a more holistic approach to Python, and we will be writing and modifying scripts in text files and executing them, rather than using the interactive shell. This is a more realistic workflow for data analysis and scientific computing, though the interactive shell is a great way to test out small snippets of code quickly.

## Writing Your First Python Script
You can create a simple Python script using any text editor. For this example, we'll use `nano`, a command-line text editor. Create a new file called `hello.py` by running:

```bash
nano hello.py
```

In the `nano` editor, type the following code:

```python
print("Hello, World!")
```

Save the file by pressing `Ctrl+O`, then press `Enter` to confirm. Exit the editor by pressing `Ctrl+X`.

To run your Python script, use the following command:

```bash
python3 hello.py
```

You should see the output:

```
Hello, World!
```

Congratulations! You've just written and executed your first Python script.

## Next Steps

Now that you've set up Python and run your first script, you're ready to explore more advanced topics such as variables, data types, control structures, functions, and libraries. In the following sections of this workshop, we will cover these topics in detail and provide hands-on exercises to reinforce your learning.

