# CRUD Operation using Console Application CLI, MongoDB, Logging, and Exception Handling

## 📘Overview

This article explains how to build a simple Python console app to manage user data using CRUD operations.
A CLI (Command-Line Interface) console application is a program that interacts with the user through text-based commands entered in the terminal or command prompt. While Flask is a lightweight web framework for building websites and APIs and FastAPI is a modern, high-performance framework for building fast, async APIs with automatic docs.

Create
Read
Update
Delete

We'll use:

Command Line Interface (CLI) for interaction    
MongoDB as our database 
Logging to keep track of actions    
Exception Handling to handle errors


## 🧰Tools Used

 Tool      -      Purpose                           
`pymongo`  -      To connect Python to MongoDB    
`argparse` -      To read commands from the terminal    
`logging`  -      To save logs of actions/errors     


# ⚙️ Setup 

## 1.✅ Install MongoDB

Install [MongoDB](https://www.mongodb.com/try/download/community) on your system and keep it running.

## 2.✅ Install Python Packages

```bash
pip install pymongo
```


## 👩‍💻 Full Code 

Create a file named `crud_app.py` and paste the below code:

```python

# crud_app.py

import argparse
import logging
from pymongo import MongoClient
```

## Setup logging
Logging in Python is used to track events that happen when your program runs.
Instead of using print() to debug or monitor your app, logging lets you:
- Save messages to a file
- Add timestamps, error levels, and more context
- Control what gets recorded (info, warnings, errors, etc.)

```python
import logging

logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logger = logging.info("App started")
```


## Connect to MongoDB
To connect a CLI (Command-Line Interface) to MongoDB using Python, you can use:
- The argparse module for CLI interaction
- The pymongo library to connect to MongoDB

## Exception Handling
Exception handling in Python is a way to deal with errors that happen while a program is running (called runtime errors or exceptions) — without crashing the program.

```python
try:
    client = MongoClient("mongodb://localhost:27017/")
    db = client["user_db"]
    users = db["users"]
    logging.info("Connected to MongoDB")
except Exception as e:
    logging.error(f"MongoDB Connection Error: {e}")
    print("Failed to connect to MongoDB. Exiting...")
    exit()

# Create user
def create_user(name, age):
    try:
        users.insert_one({"name": name, "age": age})
        logging.info(f"User '{name}' created.")
        print("User created successfully.")
    except Exception as e:
        logging.error(f"Create Error: {e}")
        print("Failed to create user.")

# Read users
def read_users():
    try:
        all_users = users.find({}, {"_id": 0})
        for user in all_users:
            print(user)
        logging.info("Read all users.")
    except Exception as e:
        logging.error(f"Read Error: {e}")
        print("Failed to read users.")

# Update user
def update_user(name, age):
    try:
        result = users.update_one({"name": name}, {"$set": {"age": age}})
        if result.modified_count:
            print("User updated successfully.")
            logging.info(f"User '{name}' updated.")
        else:
            print("User not found.")
    except Exception as e:
        logging.error(f"Update Error: {e}")
        print("Failed to update user.")

# Delete user
def delete_user(name):
    try:
        result = users.delete_one({"name": name})
        if result.deleted_count:
            print("User deleted successfully.")
            logging.info(f"User '{name}' deleted.")
        else:
            print("User not found.")
    except Exception as e:
        logging.error(f"Delete Error: {e}")
        print("Failed to delete user.")


# CLI setup
parser = argparse.ArgumentParser(description="Simple CLI CRUD App")
parser.add_argument("action", choices=["create", "read", "update", "delete"])
parser.add_argument("--name", help="User name")
parser.add_argument("--age", type=int, help="User age")

args = parser.parse_args()

# Handle actions
if args.action == "create":
    if args.name and args.age is not None:
        create_user(args.name, args.age)
    else:
        print("Please provide both --name and --age.")

elif args.action == "read":
    read_users()

elif args.action == "update":
    if args.name and args.age is not None:
        update_user(args.name, args.age)
    else:
        print("Please provide both --name and --age.")

elif args.action == "delete":
    if args.name:
        delete_user(args.name)
    else:
        print("Please provide --name.")
```

### Exception Handling using decorators

Instead of writing **try** and **except**, we can also use exceptional handling using decorator method.

```python
def handle_exceptions(func):
    @wraps(func)
    def wrapper (*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            return jsonify({"Error" : str(e) }), 500
    return wrapper
```

## How to Run the App

Open your terminal and run the following commands:

## ➕ Create User

```bash
python crud_app.py create --name Pranavi --age 28
```

## 📄 Read All Users

```bash
python crud_app.py read
```
## ✏️ Update User

```bash
python crud_app.py update --name Pranavi --age 29
```

## ❌ Delete User

```bash
python crud_app.py delete --name Pranavi
```

## 📂 What You'll See in app.log
```txt
INFO:root:Connected to MongoDB
INFO:root:User 'Pranavi' created.
INFO:root:Read all users.
INFO:root:User 'Pranavi' updated.
INFO:root:User 'Pranavi' deleted.
```


## ✅ Summary

We successfully created a console-based CRUD app using:

* MongoDB for data
* `argparse` for command-line options
* `logging` for saving logs
* Exception handling to manage errors

# Interview Questions
Q1. What is a CLI application?  
Q2. What are CRUD operations?   
Q3. What is MongoDB, and why is it used?    
Q4. How do you connect to MongoDB in Python?    
Q5. How do you insert data into a MongoDB collection?   
Q6. How do you fetch all users from MongoDB?    
Q7. What is argparse used for?  
Q8. How do you define a required command-line argument? 
Q9. How do you define an optional argument in CLI?  
Q10. Why is logging important in an application?    
Q11. How do you log messages in Python? 
Q12. What is exception handling?    
Q13. How do you catch a specific exception in Python?   
Q14. If a user enters the wrong data type (e.g., name as age), how will your code handle it?    
Q15. What happens if MongoDB is not running when the script starts? 
Q16. How will you test your CLI application?    
Q17. What's the difference between CLI, Flask, and FastAPI?     
Q18. Can you handle multiple MongoDB databases in the same Python app?