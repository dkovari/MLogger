# MLogger
A text-based log system for MATLAB. Log messages are time-stamped and formatted so they can easily be read by a human. Messages can be written to the log using a function, or using the included GUI. The GUI operates as a Callback-driven system so it can be open while other processes are running and still write to the log file.
#### Example Log File
```
# MLogger File --------------------------------------------------
# Created: 2016-07-25 09:20:15.012
#
# Date & Time           | Message Text
#========================================================================
2016-07-25 09:25:30.015 | Message1 Line 1
                        | Message1 Line 2
#------------------------------------------------------------------------
2016-07-25 09:25:32.237 | Message2 Line 1
                        | Message2 Line 2
                        | Message2 Line 3
#------------------------------------------------------------------------
```
## Including MLogger in a project
MLogger should be included in a MATLAB project as a package named "+MLogger".
### Adding as a submodule
```
git submodule add https://github.com/dkovari/MLogger/ +MLogger
#on older versions of git you will also need to pull the updates
git submodule update --init --recursive
```
If you need to update to use the latest version of MLogger then run:
```
git submodule update --init --recursive
```
### Adding as a subtree
```
...In you git repository directory...
# Create a remote connection to MLOGGER
git remote add -f mtdat https://github.com/dkovari/MLogger/

#Merge into local project
git merge -s ours --no-commit --allow-unrelated-histories MLogger/master

# Copy MATextras into project in a directory named +MLogger
git read-tree --prefix=+MLogger/ -u MLogger/master

# Commit the changes
git commit -m "Subtree merged mtdat to +MLogger"
```
To update the subtree to use the latest commit you need to manually pull changes.
```
git pull -s subtree MLogger master
```
## Usage
```matlab
logger = MLogger.Logger('YOUR FILE NAME');
%       If don't enter a name, a default name containg the date and
%       time is used.
logger.open(file);
%       Closes the currently file and opens a new file
%       specified by file.
logger.write(str);
%         writes str to the file
%         str can be a char array or a cell array containing
%         strings. Each element of the cell arrray is written
%         as a different line.
logger.ShowGUI();
%         opens the interactive GUI terminal
logger.close();
%         closes the current log file;
```
This class has a delete method, so it should close the file and GUI when it goes out of scope. However, becasue there's a callback for the figure that references the class, objects will live past their scope if the GUI is left open. In testing, it appears that MATLAB does delete the object and close the file once the GUI is closed. To ensure that everything closes when your program terminates you should call logger.close() before exiting.
