---
published: true
title: How to make MongoDB run as a Windows service
layout: post
---
Running <a href='https://www.mongodb.org/'>MongoDB</a>  on a Windows system is quite easy using the Windows executable available under the downloads sections, but keeping a terminal open just for the MongoDb Server to run can be a waste of window space.

Which is why today I will be showing you how make MongoDB run as a Windows service.

<b>Step 1</b>: Create a dbpath and log path folder
<p>Assuming you already have MongoDB installed on your windows machine, you will create a db and log folder in a directory of your choice, if you do not already have it.<br>
In my case I have <br>
<b><code> C:\data\db</code></b> as the dbpath <br>
<b><code> C:\data\log</code></b> as the log path
 <p>

<b>Step 2:</b> Create a configuration file

<p>Open a Command Prompt as an Administrator, <br>
Press the win key, type cmd the press <b>Ctrl + Shift + Enter </b>  to do that.<br>

Open your favorite text editor, create a new text file and enter the following:<br>
<code><b>
systemLog:<br>
&nbsp;&nbsp;&nbsp;destination: file <br>
&nbsp;&nbsp;&nbsp;path: 'your log path created earlier' <br>
storage: <br>
&nbsp;&nbsp;&nbsp;dbPath: 'your dbpath created earlier'
</b> </code><br>

Save the text as mongodb.cfg inside the folder you created earlier.
<p>
<b>Step 3:</b> Install MongoDB service and start service<br>
cd to the <i>bin</i> directory where MongoDB was installed
In the command prompt enter the following
<b><code>
mongod --config "path to your mongodb.cfg file" --install <br>
net start mongod
</code></b>
<br><br>
After which MongoDB will start running as a service, to test you can run <b>mongo</b> and it will connect to the mongod server without having to keep any terminal running the server open