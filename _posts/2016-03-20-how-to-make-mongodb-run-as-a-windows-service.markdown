---
published: true
title: How to make MongoDB run as a Windows service
layout: post
---
Running <a href='https://www.mongodb.org/'>MongoDB</a>  on a Windows system is quite easy using the Windows executable available under the downloads sections, but keeping a terminal open just for the MongoDb Server to run can be a waste of window space.

Which is why today I will be showing you how make MongoDB run as a Windows service.

### Step 1: Create a dbpath and log path folder
Assuming you already have MongoDB installed on your windows machine, you will create a db and log folder in a directory of your choice, if you do not already have it.
In my case I have

```bash
 C:\\data\\db #as the dbpath
 C:\\data\\log #as the log path
```

### Step 2: Create a configuration file

Open a Command Prompt as an Administrator,
Press the win key, type cmd the press <b>Ctrl + Shift + Enter </b>  to do that.

Open your favorite text editor, create a new text file and enter the following:

```bash
 systemLog:
   destination: file
   path: 'your log path created earlier'
 storage:
   dbPath: 'your dbpath created earlier'
```

Save the text as mongodb.cfg inside the folder you created earlier.

### Step 3: Install MongoDB service and start service
`cd to the bin` directory where MongoDB was installed
In the command prompt enter the following

```shell
 mongod --config "path to your mongodb.cfg file" --install
 net start mongod
```

After which MongoDB will start running as a service, to test you can run <b>mongo</b> and it will connect to the mongod server without having to keep any terminal running the server open
