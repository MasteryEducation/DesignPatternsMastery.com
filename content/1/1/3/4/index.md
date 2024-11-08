---
linkTitle: "1.3.4 Writing and Executing Your First Program"
title: "Writing and Executing Your First Program: A Beginner's Guide to Software Development"
description: "Learn how to set up your development environment, write your first 'Hello, World!' program in Python and JavaScript, and execute it successfully."
categories:
- Software Development
- Programming Basics
- Beginner's Guide
tags:
- Python
- JavaScript
- Hello World
- Programming
- IDE Setup
date: 2024-10-25
type: docs
nav_weight: 134000
---

## 1.3.4 Writing and Executing Your First Program

Embarking on your journey in software development is an exciting venture. Writing and executing your first program is a significant milestone that marks the beginning of understanding how computers interpret and execute the instructions you provide. In this section, we'll guide you through setting up your development environment, writing a simple "Hello, World!" program in both Python and JavaScript, and executing it. By the end of this chapter, you will have the skills to write, run, and troubleshoot basic programs, setting a solid foundation for more complex programming tasks.

### Setting Up the Environment

Before you can write and execute your first program, you need to set up your development environment. This involves installing the necessary software and choosing a code editor that suits your needs.

#### Installing Python

Python is a versatile and widely-used programming language, known for its readability and simplicity. To install Python, follow these steps:

1. **Download Python:**
   - Visit the official [Python website](https://www.python.org/downloads/).
   - Choose the latest version compatible with your operating system (Windows, macOS, or Linux).
   - Download the installer.

2. **Install Python:**
   - Run the downloaded installer.
   - On Windows, ensure you check the box that says "Add Python to PATH" before clicking "Install Now."
   - Follow the installation prompts to complete the setup.

3. **Verify Installation:**
   - Open a command line interface (Command Prompt on Windows, Terminal on macOS/Linux).
   - Type `python --version` and press Enter. You should see the installed Python version number.

#### Installing Node.js

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. It allows you to run JavaScript code outside of a web browser.

1. **Download Node.js:**
   - Go to the [Node.js official website](https://nodejs.org/).
   - Download the LTS (Long Term Support) version for your operating system.

2. **Install Node.js:**
   - Run the installer and follow the setup instructions.
   - Ensure that Node.js is added to your system PATH.

3. **Verify Installation:**
   - Open your command line interface.
   - Type `node --version` and press Enter. You should see the installed Node.js version number.

#### Choosing an IDE or Code Editor

An Integrated Development Environment (IDE) or code editor is essential for writing and managing your code. Here are a few popular choices:

- **Visual Studio Code (VS Code):**
  - A free, open-source code editor with support for debugging, syntax highlighting, and extensions.
  - Download from [Visual Studio Code](https://code.visualstudio.com/).

- **PyCharm:**
  - An IDE specifically designed for Python development, offering powerful tools for code analysis and debugging.
  - Available in both a community (free) and professional (paid) edition.
  - Download from [JetBrains PyCharm](https://www.jetbrains.com/pycharm/).

- **Sublime Text:**
  - A lightweight and fast code editor with a clean interface and powerful features.
  - Download from [Sublime Text](https://www.sublimetext.com/).

Once you have installed your preferred code editor, you're ready to write your first program.

### Writing the Program

Let's start with the classic "Hello, World!" program, a simple yet fundamental exercise in learning any programming language.

#### Writing "Hello, World!" in Python

1. **Open Your Code Editor:**
   - Launch your chosen code editor (e.g., VS Code, PyCharm).

2. **Create a New File:**
   - In the editor, create a new file and save it with a `.py` extension, for example, `hello_world.py`.

3. **Write the Code:**

```python
print("Hello, World!")
```

- **Explanation:**
  - `print()` is a built-in function in Python that outputs text to the console.
  - The text inside the parentheses, `"Hello, World!"`, is a string literal that will be displayed when the program runs.

4. **Save the File:**
   - Ensure you save your changes before proceeding to execution.

#### Writing "Hello, World!" in JavaScript

1. **Open Your Code Editor:**
   - Use the same or another code editor to write JavaScript code.

2. **Create a New File:**
   - Save a new file with a `.js` extension, for example, `hello_world.js`.

3. **Write the Code:**

```javascript
// JavaScript program
console.log("Hello, World!");
```

- **Explanation:**
  - `console.log()` is a function in JavaScript used to print messages to the console.
  - `"Hello, World!"` is the string that will be printed.

4. **Save the File:**
   - Again, ensure you save your work.

### Executing the Program

Now that you've written your first program, it's time to execute it and see the output.

#### Running the Python Program

1. **Open Command Line Interface:**
   - Navigate to the directory where you saved your `hello_world.py` file using the `cd` command.

2. **Execute the Program:**
   - Type `python hello_world.py` and press Enter.

3. **Interpreting Output:**
   - You should see `Hello, World!` printed to the console.
   - If you encounter any errors, check the syntax and ensure the file is saved correctly.

#### Running the JavaScript Program

1. **Open Command Line Interface:**
   - Navigate to the directory containing your `hello_world.js` file.

2. **Execute the Program:**
   - Type `node hello_world.js` and press Enter.

3. **Interpreting Output:**
   - The console should display `Hello, World!`.
   - Verify the syntax if errors occur, and ensure Node.js is correctly installed.

### Troubleshooting

Even simple programs can encounter issues. Let's look at some common errors and how to fix them.

#### Common Errors in Python

- **SyntaxError:**
  - This occurs if there's a typo or incorrect syntax.
  - Double-check your code for missing parentheses or quotation marks.

- **IndentationError:**
  - Python relies on indentation for defining blocks of code.
  - Ensure consistent use of spaces or tabs.

#### Common Errors in JavaScript

- **ReferenceError:**
  - This happens when trying to use a variable or function that hasn't been declared.
  - Ensure all variables and functions are correctly defined.

- **SyntaxError:**
  - Similar to Python, this occurs due to incorrect syntax.
  - Check for missing semicolons or brackets.

### The Satisfaction of Running Your First Program

Executing your first program is a rewarding experience. It marks the beginning of your journey in programming, where you transform ideas into functioning code. Experiment with modifying the "Hello, World!" message to see how changes affect the output. Try adding new lines of code or creating new variables to deepen your understanding.

### Encouragement to Experiment

Programming is a creative process. Don't hesitate to experiment with the code you write. Change the text in your "Hello, World!" program, add new features, or explore other functions in Python and JavaScript. The more you practice, the more confident you'll become in your programming abilities.

### Conclusion

You've successfully written and executed your first program in both Python and JavaScript. This foundational knowledge is crucial as you continue to explore more complex programming concepts and design patterns. Remember, every expert was once a beginner. Keep practicing, stay curious, and enjoy the journey of learning to code.

## Quiz Time!

{{< quizdown >}}

### What is the purpose of the `print()` function in Python?

- [x] To output text to the console
- [ ] To read input from the user
- [ ] To execute a loop
- [ ] To define a new function

> **Explanation:** The `print()` function in Python is used to display text or other outputs to the console.

### How do you execute a Python script from the command line?

- [x] `python script_name.py`
- [ ] `run script_name.py`
- [ ] `execute script_name.py`
- [ ] `start script_name.py`

> **Explanation:** To run a Python script, you use the command `python script_name.py` in the command line.

### What is the correct syntax to print "Hello, World!" in JavaScript?

- [x] `console.log("Hello, World!");`
- [ ] `print("Hello, World!");`
- [ ] `echo "Hello, World!";`
- [ ] `System.out.println("Hello, World!");`

> **Explanation:** In JavaScript, `console.log()` is used to print messages to the console.

### What should you do if you encounter a `SyntaxError` in Python?

- [x] Check for typos or incorrect syntax
- [ ] Ignore it and continue coding
- [ ] Restart your computer
- [ ] Reinstall Python

> **Explanation:** A `SyntaxError` indicates a mistake in the code syntax, such as a missing parenthesis or quotation mark, and should be corrected.

### Which of the following is a popular code editor for writing Python and JavaScript?

- [x] Visual Studio Code
- [ ] Microsoft Word
- [x] PyCharm
- [ ] Adobe Photoshop

> **Explanation:** Visual Studio Code and PyCharm are popular code editors for programming, whereas Microsoft Word and Adobe Photoshop are not suited for coding.

### What command is used to check the installed version of Python?

- [x] `python --version`
- [ ] `python --check`
- [ ] `python --info`
- [ ] `python --details`

> **Explanation:** `python --version` is used to verify the installed version of Python.

### Which error occurs if you try to use an undeclared variable in JavaScript?

- [x] ReferenceError
- [ ] TypeError
- [x] SyntaxError
- [ ] RangeError

> **Explanation:** A `ReferenceError` occurs when a script attempts to reference a variable that is not declared.

### What is the file extension for a Python script?

- [x] .py
- [ ] .js
- [ ] .html
- [ ] .java

> **Explanation:** Python scripts use the `.py` file extension.

### In which environment can you run JavaScript code outside of a web browser?

- [x] Node.js
- [ ] Python
- [ ] Java
- [ ] HTML

> **Explanation:** Node.js is a runtime environment that allows JavaScript to be executed outside of a web browser.

### True or False: Indentation is important in Python.

- [x] True
- [ ] False

> **Explanation:** Indentation is crucial in Python as it defines the structure and flow of the code.

{{< /quizdown >}}

By following the steps outlined in this section, you have taken the first step into the world of programming. Keep experimenting, learning, and building your skills as you continue your journey in software development.
