# Technical Interview: Linux Administration

**1. What is your favourite unix command line editor?**

My favorite Unix command line editor is Nano. I prefer this text editor for its ease of use and simplicity, which allows for efficiently accomplishing the task at hand. 

**2. Please give a description of the following unix commands and give examples of how they would be used on the command line and specify some of the options:**

**a. pwd**
> Description: Print working directory – Writes the full pathname of the current working directory.  
> Example Use Case: /home/username

```bash
pwd -P 	#displays the path without any symbolic links
pwd -L	#displays the path with symbolic links
```

**b. ln**

> Description: Creates a symbolic link to a file, can either be a hard or soft link.

```bash
ln –s /original/file   soft/link/file
ln original/file   hard/link/file
```

**c. ant**

> Description: Apache Ant – A tool for automating software builds 

```bash
ant –buildfile test.xml dist	# Runs ant using test.xml on the target “dist”
```

**d. sed**

> Description: A stream editor for filtering and transforming text

```bash
sed –n 10,20p file.txt	# Show lines 10-20 of file.txt
sed '/./,$!d' file.txt	# Delete all leading white space
```

**e. chmod**

> Description: Change file mode bits

```bash
chmod a+x filename	# Add execution permissions to file
chmod 777 filename	# Anybody can read, write, execute
```

**f. ps**

> Description: Report a snapshot of the current processes

```bash
ps aux --sort=-pcpu,+pmem		# Display process sorted by cpu / memory usage
ps U root				            # Display all process owned by user root
```

**g. kill**

> Description: Send a signal to a process, (TERM by default)

```bash
kill –L 	  # List the available signal choices into a table
kill -9 -1	# Kill all processes you can kill
```

**h. scp**

> Description: Secure copy (remote file copy program)

```bash
scp username@from_host:file.txt /local/directory/		            # Copy file from remote host to locat directory
scp -r /local/directory/ username@to_host:/remote/directory/		# Copy file from local host to a remote directory
```

**i. ssh**

> Description: OpenSSH SSH Client (remote login program)

```bash
ssh remote_username@remote_host		# Connect to a remote system
```

**j. crontab**

> Description: Maintain crontab files for individual users

```bash
crontab -l 		# Manage crontab tasks for the current user
crontab –e		# Edit crontab entries 
crontab –r		# Remove crontab entries 
```

**k. wc**

> Description: Print newline, word, and byte counts for each file 

```bash
wc testing.txt      # Returns the number of lines, words and bytes in file
wc -l testing.txt   # Returns the number of lines in file
wc -w testing.txt   # Returns the number of words in a file 
wc –L testing.txt   # Returns the length of the longest line in a file
```

**3. How would you accomplish the following tasks on the command line?**

**a. Switch to the Super User to perform administrative functions**

Type in 
```bash
sudo <command name> 
```
to switch to the super user, provide the super user password if promoted.

**b. Get the last 500 lines of a log file.**

```bash
tail -500 log.txt
```

**c. Compare the differences between two files.**

```bash
diff file1.txt file2.txt
```

**d. Compare the differences between all files in two directories.**

```bash
diff –r directory1 directory2
```

**e. Find the process that is using the most CPU.**

```bash
ps --sort=-pcpu | head -n 2
```

**4. Explain what DNS is.**

The Domain Name System (DNS) converts website names into IP addresses by accessing a system of linked DNS servers.  DNS requests are often cached to improve performance, and third party DNS servers can sometimes offer improved performance. 

**5.	What is an SSH reverse tunnel and when would you use it?**

A SSH reverse tunnel allows systems behind a firewall to be accessed from the outside world.  SSH reverse tunneling would be used to bypass a firewall.

***

**6. What is the difference between**

**a. Megabytes**

A megabyte is 1,000,000 bytes, using the base 10 system. 

**b. Mebibytes**

A Mebibyte is 1,048,576 bytes (1024 kibibytes *1024), using the base 2 system. The binary prefix “mebi” lets us know that this unit is a power of 2 rather than 10.  

**c. Gigabytes**

A gigabyte is 1,000,000,000 bytes, using the base 10 system. (1000 megabytes).

**d. Gibibytes**

A Gibibyte is 1,073,741,824 bytes (1024 mebibytes * 1024), using the base 2 system. The binary prefix “Gibi” lets us know that this unit is a power of 2 rather than 10.  

**e. Megabits**

A megabit is equal to 1,000,000 bits of information.  Note that this unit is measured in bits rather than bytes and is equal to 125 kilobytes of information. (1,000,000 / 8 bits per byte = 125,000 bytes)















