# Intro to Python

![](.gitbook/assets/python.svg)

> 🔗 [Python Docs](https://docs.python.org/3/)
>
> 🔗 [The Python Standard Library](https://docs.python.org/3/library/index.html)
>
> 🔗 [Python Cheatsheet](https://www.pythoncheatsheet.org/)
>
> 🔗 [Python Tutorial - W3Schools](https://www.w3schools.com/python/default.asp)
>
> 🔗 [VsCode Linux setup](https://code.visualstudio.com/docs/setup/linux#_debian-and-ubuntu-based-distributions)

```bash
mkdir ~/tcm/peh/python
nano first.py
# or with Sublime, VsCode installed
subl first.py
code first.py
```

- Example of Python script
  - **`#!/bin/python3`** - first line
  - Use **`#`** for comments

```python
#!/bin/python3

# Print string <--- this is a comment
print("Hello, world!")
```

```bash
# eventually make the script runnable
chmod +x first.py
# it must containg "#!/bin/python3" as first line, to run it with
./first.py

# Or run it with python3
python3 first.py
```

---

## [Strings](https://docs.python.org/3/tutorial/introduction.html#strings)

```bash
nano strings.py
```

```python
#!/bin/python3

# Print string
print("Hello, World!") 	 # Single quotes
print('Hello, World!')	 # Double quotes
print("""Multiple lines
string!""")				 # Triple quotes = Multiple lines
print("Hello "+"World!") # Concatenate strings
print("First line.\nSecond line.")	  # New line
print('This isn\'t a long string.')   # \' to escape the single quote..or use double quotes

# Creation
my_string = "Hello, World!"

# Accessing individual characters by indexing
print(my_string[0])
# output = 'H'

# Concatenation
greeting = 'Hello' + ' ' + 'World!'

# Length of the string
print(len(my_string))

# Slicing
# Extract a substring from a string using slicing, specifying the start and end indices.
substring = my_string[7:12]
print(substring)
# output = 'World'

# Methods to manipulate and transform strings
print(my_string.upper())
# output = 'HELLO, WORLD!'
```

![](.gitbook/assets/2023-06-28_00-17-23_123.png)

---

## [Math](https://docs.python.org/3/library/math.html?highlight=math#module-math)

```bash
nano math.py
```

```python
#!/bin/python3
import math # Module with various math functions and constants

# Operators
print(10 + 10)  # Addition
print(10 - 10)  # Subtraction
print(10 * 10)  # Multiplication
print(25 / 10)  # Division with remainder - float
print(25 // 10) # No remainder division - returns the quotient as an integer
print(25 % 10)  # Modulo - returns the remainder of division
print(10 ** 5)  # Exponentiation

# Using math functions
print(math.sqrt(500))  # Square root
print(math.pow(2, 10)) # Raise a to power of b
print(math.sin(math.pi/2)) # Calculate sine of pi/2 (in radians)
```

![](.gitbook/assets/2023-06-27_19-33-02_117.png)

---

## [Variables](https://www.w3schools.com/python/python_variables.asp) & Methods

➡️ A **variable** is a dynamic container for storing data values.

➡️ A **method** (or **function**) is a block of code which runs when called and performs specific operations. It can have return values, perform actions, accept parameters, etc.

```bash
nano vars.py
```

```python
#!/bin/python3

# Variables assignment with different data types
age = 22		# integer
height = 69.5 	# float
name = "Sys"	# string
is_true = True	# boolean

# Variable usage
print(int(age))
print(int(22.7))
print("Hello, " + name + ". Is your height " + str(height) + " cm?")
age += 1	# Add 1 to age variable
print(age)
if is_true:
    print("The condition is true \n")	# is_true boolean is "True"

# Built-in method
quote = "The quieter you become, the more you can hear."
print(quote.upper())  # upper-case method
print(quote.lower())  # lower-case method
print(quote.title())  # title-case method
length = len(quote) # Count characters
print("Quote length:", length)
print('\n')
```

![](.gitbook/assets/2023-06-27_20-09-27_118.png)

---

## [Functions](https://www.w3schools.com/python/python_functions.asp)

➡️ A **function** is a reusable block of code that performs a specific task.

- defined with the `def` keyword
  - **`def function_name():`**
- accepts parameters

❗ **[Indentation](https://www.w3schools.com/python/gloss_python_indentation.asp) is very important in Python**, since it's used to indicate a **block of code**.

```bash
nano func.py
```

```python
#!/bin/python3

# Function with parameters
def greet(name):
    print("Hello, " + name + "!")
# Function call with an argument that is assigned to the "name" param
greet("Sys")

age = 22

# Function without parameters
def who_am_i():
    name = "Sys"    # Local variable
    age = 55        # Local variable
    print("My name is " + name + " and I am " + str(age) + " years old.\n")
who_am_i()
print(age)  # Not the function's "age" variable

# Parameters
print('Add 100:')
def add_one_hundred(num):
    print(num + 100)
add_one_hundred(400)    # Call function with an argument

print('Add 2 numbers:')
def add(x,y):
    print(x + y)
add(400,300)

print('Multiply 2 numbers:')
# Return statement
def multiply(x,y):
    return x * y    
multiply(5,5)   # Can be assigned to a variable or called
print(multiply(5,5))
result = multiply(9,9)
print(result)

print('Square root:')

def square_root(x):
    print(x ** .5)
square_root(64)

print('This is a new line:')
# New line function
def nl():
    print('\n')
nl()
```

![](.gitbook/assets/2023-06-27_20-37-26_121.png)

---

## [Booleans](https://www.w3schools.com/python/python_booleans.asp) & [Operators](https://www.w3schools.com/python/python_operators.asp)

➡️ **Boolean expressions** are expressions that evaluate to either `True` or `False`, used in conditional statements and logical operations.

➡️ **Relational operators** are used to compare values and create boolean expressions.

```bash
nano oper.py
```

```python
#!/bin/python3

# Boolean expressions
print("Boolean expressions:")

bool1 = True
bool2 = 2*3 == 6	# == Check if two values are equal
bool3 = False
bool4 = 3*3 != 9	# != Check if two values are not equal

print(bool1,bool2,bool3,bool4)	# Output: True, True, False, False
print(type(bool1))	# Outputs the class 'bool'

bool5 = "True"
print(type(bool5))	# Outputs the class 'str'


# Relational and Boolean operators
print('\n')
print("Relational and Boolean operators:")
greater_than = 7 > 5		# greater_than = True
less_than = 5 < 7			# less_than = True
greater_than_equal_to = 7 >=7
less_than_equal_to = 7 <= 7
# All above statements are true

test_and = True and True 	#True
test_and2 = True and False  #False

test_or = True or True 		#True
test_or2 = True or False 	#True

test_not = not True 		#False

# Relational operators - Compare values
x = 5
y = 10
print("x =",x)
print("y =",y)
print("x == y")
print("",x == y)   # Output: False
print("x < y")
print("",x < y)    # Output: True


# Boolean expressions and Logical operators
print('\n')
print("Boolean expressions:")
print("x < y and y > 0")
print("",x < y and y > 0)	# Output: True - if both operands are True

print("x < y or y < 0")
print("",x < y or y < 0)	# Output: True - if at least one operand is True

print("not (x == y)")
print("",not (x == y))		# Output: True - negates the value of the operand
```

![](.gitbook/assets/2023-06-28_00-16-16_122.png)

---

## [Conditional statements](https://www.w3schools.com/python/python_conditions.asp)

➡️ **Conditional statements** allow the execution of different actions based on certain conditions. Logical math conditions can be used in **`if statements`** and **`loops`**.

```bash
nano conditions.py
```

```bash
#!/bin/python3

# Conditional Statements
def icecream(money):
	if money >= 3:
		return "Enjoy yourself an icecream!"
	else:
		return "No icecream for you!"

print(icecream(5))	# 5$ > 3 - if statement is true
print(icecream(1))	# 1$ < 3 - if statement is false

print('\n')

# Multiple conditions
def alcohol(age, money):	# Function with multiple parameters
	if (age >= 18) and (money >= 5):	# Conditional statement, relational operators (boolean)
		return "You're getting a Pina Colada!!!"
	elif (age >= 18) and  (money < 5):
		return "You need more money for a drink."
	elif (age < 18) and (money >= 5):
		return "You are too young kid!"
	else:
		return "Hey kid, you don't have money nor the age for an alcoholic drink."

print(" Age+Money ok:")
print(alcohol(18,5))
print(" Age ok, but no Money:")
print(alcohol(18,4))
print(" Money ok, but no Age:")
print(alcohol(16,6))
print(" No Age, No Money:")
print(alcohol(17,3))


print('\n')
print('IF-ELSE')
# "if - else" Statement
x = 5
if x > 0:
    print("x is positive")	# Executed if x > 0 is TRUE
else:
    print("x is not positive")	# Executed if x > 0 is FALSE

print('IF-ELIF-ELSE')
# Multiple Conditions "if - elif - else" Statement 
x = 0
if x > 0:
    print("x is positive")	# Executed if x > 0 is TRUE
elif x < 0:
    print("x is negative")	# Executed if x > 0 is FALSE and x < 0 is TRUE
else:
    print("x is zero")		# Executed if both conditions are FALSE 
```

![](.gitbook/assets/2023-07-01_19-37-07_128.png)

---

## [Lists](https://www.w3schools.com/python/python_lists.asp)

➡️ **Lists** are used to store multiple *items* in a single variable.

- `list = ["item1", "item2", "item3"]`
- **items** are ordered, changeable and indexed (first item index starts at `[0]`)

```bash
nano lists.py
```

```python
#!/bin/python3

# LISTS
print("LISTS")
fruits = ["apple", "banana", "orange"]

# Mutable Lists
fruits[1] = "grape"     # Modifying an element
fruits.append("kiwi")   # Adding an element to the end
fruits.remove("apple")  # Removing an element
print(fruits)
print('\n')

# List Operations
fruits = ["apple", "banana", "orange"]
fruits2 = ["grape", "kiwi"]

print("Fruits: ",fruits)
print("Fruits2: ",fruits2)
combined = fruits + fruits2		# Concatenation
print("Combined: ",combined)	# Output: ["apple", "banana", "orange", "grape", "kiwi"]

# Length
print("Fruits list length is:",len(fruits))	# Output: 3 (elements in a list)

# Slicing
sublist = fruits[1:3]
print("Sublist: ",sublist)	# Output: ["banana", "orange"]

# List Iteration
print('\n')
print("Fruits list content by iteration:")
for fruit in fruits:
    print(fruit)	# Output: "apple", "banana", "orange"


# MOVIES
print('\n')
print("MOVIES")
movies = ["Kickboxer", "The Grandmaster", "Ip Man", "Rocky", "Ong Bak", "Rurouni Kenshin Origins"]
print("Movies list is: ",movies)
print(movies[1])	# Returns the second item
print(movies[0])	# Returns the first item
print(movies[1:3])	# Returns 2 items starting from index 1
print(movies[1:])	# Returns all items starting from index 1 to end of list
print(movies[:3])	# Returns everything before index 3
print(movies[-1])	# Returns last item

print("Movies list length is:",len(movies))

movies.append("The Raid 2")
print(movies)

movies.insert(2,"Enter The Dragon")
print(movies)

movies.pop() # Remove the last item
print(movies)

movies.pop(0) # Remove the first item
print(movies)

new_movies = ["Creed III", "John Wick"]
all_movies = movies + new_movies
print("All movies list:",all_movies)

# Two dimentional lists
print('\n')
print("GRADES")
grades = [["Ron", 55], ["Harry", 66], ["Luna", 87]]
rons_grade = grades[0][1]
print(rons_grade)
grades[0][1] = 76
print(grades)
```

![](.gitbook/assets/2023-07-01_20-20-58_129.png)

---

## [Tuples](https://www.w3schools.com/python/python_tuples.asp)

➡️ **Tuples** are used to store multiple *items* in a single variable, similar to a list but immutable (cannot be modified once created).

- `tuple = ("item1", "item2", "item3")`
- items are ordered, indexed and unchangeable

```bash
nano tuples.py
```

```python
#!/bin/python3

# Tuples
fruits = ("apple", "banana", "orange")
print(fruits[0])
print(fruits[2])

#fruits[1] = "grape"     # TypeError: 'tuple' object does not support item assignment

# Tuple Operations
fruits = ("apple", "banana", "orange")
fruits2 = ("grape", "kiwi")

combined = fruits + fruits2
print("Tuples combined:",combined)

print("Fruits tuple length is:",len(fruits))

subtuple = fruits[1:3]
print("Subtuple:",subtuple)
```

![](.gitbook/assets/2023-07-01_20-38-59_131.png)

---

## [Looping](https://www.w3schools.com/python/python_for_loops.asp)

➡️ **Looping** allows to repeat a block of code multiple times, iterating over a sequence and performing repetitive tasks.

- [`for` loop](https://www.w3schools.com/python/python_for_loops.asp): *iterate over a sequence and execute a set of statements for each item in a list, tuple, string, set, dictionary*
- [`while` loop](https://www.w3schools.com/python/python_while_loops.asp): *execute a set of statements as long as a condition is true*

```bash
nano loops.py
```

```python
#!/bin/python3

# Looping

# "for" loops - start to finish of an iterate
print("FOR Loop:")

fruits = ["apple", "banana", "cherry"]
for fruit in fruits:	# iterate over each item in "fruits" list
    print(fruit)
	
# "while" loops - execute as long as true, until false
print("\nWHILE Loop:")

count = 0
while count < 10:	# as long as true, code will be executed
    print(count)
    count += 1

# "break" statement - stop the loop prematurely
print("\nBrake:")
fruits = ["apple", "banana", "cherry"]
for x in fruits:
  if x == "banana":
    break
  print(x)

# "continue" statement - skip current iteration and move to the next one
print("\nContinue:")
fruits = ["apple", "banana", "peach"]
for y in fruits:
	if y == "banana":
		continue
	print(y)
```

![](.gitbook/assets/2023-07-03_10-55-13_142.png)

---

## [Advanced strings](https://www.w3schools.com/python/python_strings_methods.asp)

```bash
nano strings.py
```

```python
#!/bin/python3

# Advanced Strings

my_name = "sysElement"	# this string is immutable
print(my_name[0])	# first letter
print(my_name[-1])	# last letter

sentence = "The quieter you become, the more you can hear."
print("Sentence:\n",sentence)
print(sentence[:3])		# Print everything before the index 3
print("Sentence split:\n",sentence.split())	# Delimiter - default is a "space"

sentence_split = sentence.split()
sentence_join = ' '.join(sentence_split)
print("Sentence join:\n",sentence_join)

# Character escaping with \
quote = 'The master said: "The quieter you become, the more you can hear."'
print(quote)
quote = "The master said: \"The quieter you become, the more you can hear.\""
print(quote)

# Strip
too_much_space = "             master       "
print(too_much_space.strip(),"\n")	# Default delimiter is "space"

# Case insensitive
print("A" in "Apple")	# TRUE
print("a" in "Apple")	# FALSE

letter = "A"
word = "Apple"
print(letter.lower() in word.lower())	# TRUE

# String Formatting

name = 'Alice'
age = 30
print("My name is %s and I'm %d years old." % (name, age))
# Output: My name is Alice and I'm 30 years old.

movie = "Minions"
print("My favorite movie is {}.".format(movie))	# String format method
print("My favorite movie is %s." % movie)	# % formatting
print(f"My favorite movie is {movie}.")		# string literal
```

![](.gitbook/assets/2023-07-03_10-54-56_141.png)

---

## [Dictionaries](https://www.w3schools.com/python/python_dictionaries.asp)

➡️ **Dictionaries** are used to store, retrieve and manipulate data based on `key:value` pair values.

- `dictionary = {"item1":value, "item2":value, "item3":value}`

```bash
nano dict.py
```

```python
#!/bin/python3

# Dictionaries - key/value pairs {}

# Creation and access
# The values can be of any data type
employees = {"Finance": ["Bob", "Linda", "Tina"], "IT": ["Gene", "Alex", "Louise"], "HR": ["Jimmy", "John"]}
print(employees)

# Modification
employees['Legal'] = ["Mr.Tex"]		# Add new key:value pair
print(employees)

employees.update({"Sales": ["Andie", "Ollie"]})		# Add new key:value pair
print(employees)

del employees["HR"]		# Delete a key:value pair
print(employees,"\n")


cocktails = {
    "Mojito": 6,
    "Long Island": 9,
    "Lemon Drop": 8
} # drink is key, price is value
print("Cocktails Dictionary:\n",cocktails)
print("Mojito price is:",cocktails["Mojito"],"\n")

# Update a value
cocktails['Long Island'] = 10
print(cocktails)

print("Long Island price is:",cocktails.get("Long Island"),"\n")

# Iteration
print("Cocktails Dictionary Lenght is:",len(cocktails))

print("\nCocktails Dictionary contains:")
for key in cocktails:
	print(key, cocktails[key])
```

![](.gitbook/assets/2023-07-03_11-25-49_144.png)

---

## [Modules](https://www.w3schools.com/python/python_modules.asp)

➡️ **Modules** are like code libraries, containing a set of functions to include in the application.

- reside in external Python files/libraries
- `import <module>` 

```bash
nano modules.py
```

```python
#!/bin/python3

# Modules Importing

#import math			# Import module
#result = math.sqrt(25)
#print(result)

from math import sqrt	# Import specific functions or variables
result = sqrt(25)		# Use function without prefixing module name
print(result)

import math as m 		# Import module as an alias
result = m.sqrt(25)
print(result)

from math import *		# Import all functions and variables
result = sqrt(25)		# Use functions without prefixing module name
print(result)


import sys 				# System functions and parameters
from datetime import datetime as dt 	# Import with alias 
print(sys.version)
print(dt.now())
```

![](.gitbook/assets/2023-07-03_11-35-32_145.png)

---

## [Sockets](https://docs.python.org/3/library/socket.html)

➡️ **Sockets** are used to send messages across a network, enabling programs to establish connections, send and receive data over various network protocols.

Primary `socket` module methods and functions are:

- `socket()`
- `.bind()`
- `.listen()`
- `.accept()`
- `.connect()`
- `.connect_ex()`
- `.send()`
- `.recv()`
- `.close()`

```bash
nano s.py
```


```python
#!/bin/python3
import socket

### e.g. 1 - TCP SERVER
# Create a TCP socket
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# AF_INET is for IPv4 - Address family
# SOCK_STREAM is for the TCP port - Socket type

# Create a UDP socket
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# SOCK_DGRAM is for UDP port

# Bind the socket to a specific address and port
server_address = ('localhost', 1234)	# localhost = 127.0.0.1
tcp_socket.bind(server_address)

# Listen for incoming connections
tcp_socket.listen(5)	# 5 is the "backlog" parameter
# "backlog" = number of unaccepted connections that the system will allow before refusing new connections

while True:
    # Accept a client connection
    client_socket, client_address = tcp_socket.accept()		# Return value is a pair (conn, address)

    # Receive and send data
    data = client_socket.recv(1024)		# Read data sent by client - 1024 = max buffer size

    client_socket.send(b"Received: " + data)	# Return the number of bytes sent

    # Close the client socket
    client_socket.close()
```

![TCP Server](.gitbook/assets/2023-07-04_17-08-01_151.png)

```bash
nano s2.py
```

```python
#!/bin/python3
import socket

### e.g. 2 - Connect to a port
HOST = '127.0.0.1'	# localhost loopback address
PORT = 5555

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST,PORT))
```

- Listen to the `5555` port with `nc` and launch the `s2.py` Python script.
  - the socket will connect to the listening port

```bash
nc -nvlp 5555
```

![](.gitbook/assets/2023-07-04_17-14-16_152.png)

---

## e.g. Port scanner

- Port scanning of an IP address

> This port scanner is a proof-of-concept not optimized script, that checks for open ports on a specified target IP within a given range.

```bash
nano scanner.py
```

```python
#!/bin/python3

import sys
import socket
from datetime import datetime

# Define target
if len(sys.argv) == 2:
	target = socket.gethostbyname(sys.argv[1]) #Translate hostname to IPv4
else:
	print("Invalid amount of arguments.")
	print("Syntax: python3 scanner.py <ip>")

#Add a pretty banner
print("-" * 50)
print("Scanning target "+target)
print("Time started: "+str(datetime.now()))
print("-" * 50)

try:
	for port in range(50,85):
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		socket.setdefaulttimeout(1)
		result = s.connect_ex((target,port)) #returns an error indicator - if port is open it throws a 0, otherwise 1
		if result == 0:
			print("Port {} is open".format(port))
		s.close()

except KeyboardInterrupt:
	print("\nExiting program.")
	sys.exit()
	
except socket.gaierror:
	print("Hostname could not be resolved.")
	sys.exit()

except socket.error:
	print("Could not connect to server.")
	sys.exit()
```

- (ChatGPT) Enhanced script

```python
#!/bin/python3

import sys
import socket
from datetime import datetime

def print_banner(target):
    # Prints a banner with the target information and current time.
    print("-" * 50)
    print(f"Scanning target {target}")
    print(f"Time started: {datetime.now()}")
    print("-" * 50)

def validate_arguments(args):
    # Validates the number of arguments and returns the target address.
    if len(args) != 2:
        print("Invalid number of arguments.")
        print("Syntax: python3 scanner.py <ip>")
        sys.exit(1)
    return args[1]

def scan_ports(target, start_port=50, end_port=85):
    # Scans the ports in the given range on the target IP address.
    try:
        for port in range(start_port, end_port + 1):
            with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
                socket.setdefaulttimeout(1)
                result = s.connect_ex((target, port))  # Returns 0 if port is open, otherwise 1
                if result == 0:
                    print(f"Port {port} is open")
    except KeyboardInterrupt:
        # Handles user interrupt (Ctrl+C)
        print("\nExiting program.")
        sys.exit(0)
    except socket.gaierror:
        # Handles errors related to resolving the hostname
        print("Hostname could not be resolved.")
        sys.exit(1)
    except socket.error:
        # Handles general socket errors
        print("Could not connect to server.")
        sys.exit(1)

def main():
    # Main function to drive the script.
    target = validate_arguments(sys.argv)
    target_ip = socket.gethostbyname(target)  # Translate hostname to IPv4
    print_banner(target_ip)
    scan_ports(target_ip)

if __name__ == "__main__":
    main()
```

- Try to scan an internal LAN IP

```bash
python3 scanner.py 192.168.31.2

# Response
--------------------------------------------------
Scanning target: 192.168.31.2
Time started: 2024-06-29 15:19:30.407598
--------------------------------------------------
Port 53 is open
```

![](.gitbook/assets/2024-06-29_15-22-24_524.png)

---

## [User input](https://docs.python.org/3/library/functions.html#input)

➡️ `input()` function is used to take input from the user via the console and return it as a string (by default). Proper validation and error handling is necessary for specific data types and invalid inputs.

```bash
nano input.py
```

```python
#!/bin/python3

# Input
name = input("Enter your name: ")   # Prompt
print("Hello, " + name + "!")   # Print using the entered name

age = input("Enter your age: ")
age = int(age)  # Convert the input string to an integer
print("You will be " + str(age+1) + " next year.")

# Calculator
x = float(input("Give me a number: "))
o = input("Give me an operator: ")
y = float(input("Give me yet another number: "))

if o == "+":
	print(x + y)
elif o == "-":
	print(x - y)
elif o == "/":
	print(x / y)
elif o == "*":
	print(x * y)
elif o == "**" or 0 == "^":
	print(x ** y)
else:
	print("Unknown operator.")
```

![](.gitbook/assets/2024-06-29_15-49-25_525.png)

---

## [Read & Write files](https://docs.python.org/3/tutorial/inputoutput.html#tut-files)

➡️ To **read** from a file, open it in read mode using `open("filename", "r")`. Then, use `read()`, `readline()`, or `readlines()` to access its contents.

➡️ To **write** to a file, open it in write mode using `open("filename", "w")`. Then, use the `write()` method to add content to the file.

➡️ To append content to an existing file, open it in append mode using `open("filename", "a")`. Then, use the `write()` method to add content.

```bash
nano readwrite.py
```

```python
#!/bin/python3

# READ
# Open the file in read mode
months = open('months.txt', "r")

# Read the entire content
content = months.read()
print(content)

# Read a single line
line = months.readline()
print(line)

# Read all lines
lines = months.readlines()
print(lines)

# Close the file
months.close()

# Alternative way to properly close the file even if exception occurs
with open('months.txt', "r") as months:
    content = months.read()
    print(content)


# WRITE
# Open the file in write mode
days = open("days.txt", "w")

# Write content to the file
days.write("Hello, World!\n")
days.write("This is a new line.")

# Close the file
days.close()


# APPEND
# Open the file in append mode
days = open("days.txt", "a")

# Append content to the file
days.write("\nMonday")

# Close the file
days.close()
```

![read](.gitbook/assets/2024-06-29_16-20-39_526.png)

![write & append](.gitbook/assets/2024-06-29_16-27-44_528.png)

---

## [Classes & Objects](https://docs.python.org/3/tutorial/classes.html)

➡️ A **class** is a blueprint for creating objects, defining their attributes and methods. It serves as a template for creating instances with similar characteristics and behaviors.

➡️ An object is an instance of a class, created using the class as a blueprint. Each object has its own attributes and can use the class's methods. You create objects by calling the class like a function.

```bash
nano Employees.py
```

```python
class Employees:
    # This method initializes the attributes of the class
    def __init__(self, name, department, role, salary, years_employed):
        self.name = name
        self.department = department
        self.role = role
        self.salary = salary
        self.years_employed = years_employed

    # This method checks if the employee is eligible for retirement
    def eligible_for_retirement(self):
        if self.years_employed >= 20:
            return True
        else:
            return False
```

```bash
nano ouremployees.py
```

```python
#!/bin/python3

from Employees import Employees  # Import the Employees class from the Employees module

# Create two instances of the Employees class
e1 = Employees("Bob", "Sales", "Director of Sales", 100000, 20)
e2 = Employees("Linda", "Executive", "CIO", 150000, 10)

# Print the name of the first employee
print(e1.name)

# Print the role of the second employee
print(e2.role)

# Check if the first employee is eligible for retirement and print the result
print(e1.eligible_for_retirement())
```

![](.gitbook/assets/2024-06-29_17-45-17_530.png)

---

