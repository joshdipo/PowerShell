- [Powershell](#powershell)
  - [Features](#features)
  - [Usage](#usage)
  - [Pipelines](#pipelines)
  - [Objects](#objects)
  - [Basic Commands](#basic-commands)
  - [Exercises](#exercises)
    - [Beginner](#beginner)
      - [**Exercise 1: File Operations/Searching/Basic Powershell Operators**](#exercise-1-file-operationssearchingbasic-powershell-operators)
      - [**Exercise 2: One-liners**](#exercise-2-one-liners)
      - [**Exercise 3: Windows Process Related**](#exercise-3-windows-process-related)
      - [**Practice Exercise 4: User Input**](#practice-exercise-4-user-input)
      - [Practice Exercise 5:  Programming using PowerShell](#practice-exercise-5--programming-using-powershell)
    - [Intermediate](#intermediate)
      - [**Problem 1: Service restart on multiple computers and logging**](#problem-1-service-restart-on-multiple-computers-and-logging)
      - [**Problem 2: Event Viewer**](#problem-2-event-viewer)

---

# Powershell

## Features

- Operates on objects, not text.
- Has `cmdlets` They are built on a common runtime rather than seperate executables. This provides a consistent experience in parameter parsing and pipeline behaviour.
- Has many types of commands
  - Native Executables
  - cmdlets
    - Named acording to the *verb-noun* structure
  - functions
  - scripts
  - aliases

## Usage

- *Tab* autocomplete - Typing the start of what you want an pressing 'Tab' will start cycling through all posible commands that start with what you have typed in.
- You can write multiple commands on the same line by seperating them with a `;`. (doesn't use objects)
- By putting `( )` around a command that is returning an object(s), you can change the object. For example, if you 'get' a process you can then call the `kill()` method on it, like you would call methods on objects in C# `(Process_name).kill()`

<br>

- Try to keep an object for as long as possible
- Always try to keep the number of objects down by filtering early on in a chain.
- Filter left, Format right

<br>

## Pipelines

A pipeline is a series of commands connected by pipeline operators `|`. Each pipeline operator sends the results of the preceding command to the next command.

```powershell
Command-1 | Command-2 | Command-3
```

`$_` represents the current pieline object. It cannot be used in the first command as there is no object returned to act on.

<br>

## Objects

When a cmdlet runs, it returns an object. Every action in powershell happens within the context of objects. As data moves from one command to the next, it moves as one or more identifiable objects.

An object is a collection of data and is made up of three types of data: the objects type, its methods, and its properties.

- The object type tells what kind of object it is.
  - An object representing a file is a **FileInfo** object.
- The object methods are actions that you can perform on the object.
  - **FileInfo** objects have a **CopyTo** method that you can use to copy the file.
- Object properties store information about the object.
  - **FileInfo** objects have a **LastWriteTime** property that stores the date and time that the file was most recently accessed.

<br>

## Basic Commands

```powershell
#-----------------------------#
# Help / 'More info' commands #
#-----------------------------#

# Help #

Get-Help -Name name_of_command
# Returns a list of commands 



get-verb
# Returns a list of all available verbs.



Get-Command
# Returns a list of all available commands.

Get-Command -Verb verb_name
# Returns a list of commands with a verb including the input.

Get-Command -Noun noun_name*
# Returns a list of commands with a noun including the input.

Get-Command -Verb verb_name -Noun noun_name*
# Returns a list of commands with both a matching verb and noun.

Get-Command -Name command_name*
# Returns a list of commands matching the input.



# More info #

get-alias command_name_alias
# Returns the full name for the command that you've provided the alias for.

get-alias -Definition command_name
# Returns the alias for the command that you've provided the name of.



returned_object(s) | get-member or gm
# Returns a list containing the type of object, and all events, methods, and properties that the object has.



Get-Command -ParameterType object_type
# Returns a list of all cmdlets that operate on this type.
```

```powershell
#--------------------------------#
# Files, Folders and Directories #
#--------------------------------#


Get-PSDrive
# Returns a list of all drives on this machine.

dir and ls
# Returns all files and folder in current directory.



cd path
# Change directory to provided path.

cd ..
# Go up a directory.



new-item or ni file_name.file_extension
# Create a new file in current directory.

cat file_path
# View the contents of a file.
```

```powershell
#--------#
# Basics #
#--------#


cls
# Clears the screen
```


```powershell
#-----------#
# Variables #
#-----------#


$variable_name = value
# Creates and / or assigns a value to a variable.


$variable_name
# Returns the contents of the variable.


$variable_name.property_name
# Returns the contents of the property for that variable. Properties can be nested.
```

```powershell
#----#
#    #
#----#



```

## Exercises

### Beginner

#### **Exercise 1: File Operations/Searching/Basic Powershell Operators**

1. Create a folder called *TestingPurpose* and 2 subfolders inside it; *SubFolder1*, *SubFolder2*

```powershell
md SubFolder1, SubFolder2
```

<br>

2. Create some test files inside these folders:
   
    *TypeAtTest1.txt, TypeATest2.txt ... TypeATest50.txt into SubFolder1*

    *TypeBTest51.txt, Purpose52Test2.txt .... TypeBTest100.txt into SubFolder2*

```powershell
cd 'C:\Users\Joshua.Dear\Documents\Practice\TestingPurpose\SubFolder1\'; $num = 1; while ($num -lt 51)
{
ni TypeATest${num}.txt;
$num += 1;
}; cd ..; cd SubFolder2; $num = 51; while ($num -lt 101)
{
if (($num % 2) -eq 1)
{
ni TypeBTest${num}.txt;
}
else
{
$num2 = $num -50;
ni Purpose${num}Test${num2}.txt;
};
$num += 1;
};
```

<br>

3. Move all files that have an odd number in their name to *SubFolder2*

```powershell
Get-ChildItem | ForEach {$file = $_.Name; $fileName = $_.Name; $file = $file -replace '[a-zA-Z]', ''; $file = $file.Replace('.', ''); if ([int]($file % 2) -eq 1) {Move-Item -Path ./${fileName} -Destination 'C:\Users\Joshua.Dear\Documents\Practice\TestingPurpose\SubFolder2\';};};
```

<br>

4. Move all files that have an even number in their name to *SubFolder1*

```powershell
Get-ChildItem | ForEach {$file = $_.Name; $fileName = $_.Name; $file = $file.Replace('.', ''); $file = $file -replace '[a-zA-Z]', ''; if(([int]$file % 2) -eq 0){Move-Item -Path ./${fileName} -Destination 'C:\Users\Joshua.Dear\Documents\Practice\TestingPurpose\SubFolder1\'};};
```

<br>

5. Rename folder *SubFolder1* to *EvenFilesContainer* and *SubFolder2* to *OddFilesContainer*

```powershell
Rename-Item .\SubFolder1\ .\EvenFilesContain; .\SubFolder2 .\OddFilesContainer
```

<br>

6. Prepare a list of all files that exist inside the *TestingPurpose* folder

    Example:
    
        MasterFile.txt:
        As of YYYYMMDD HH:MM files inside Testing Purpose are:
        C:\Users\Joshua.Dear\Documents\Practice\TestingPurpose\Purpose52Test2.txt
        .
        .
        C:\Users\Joshua.Dear\Documents\Practice\TestingPurpose\TypeBTest99.txt

```powershell
$list = Get-ChildItem -Recurse | where {$_ -isnot [System.IO.DirectoryInfo]};
$dateTime = Get-Date -Format "yyyy/MM/dd HH:mm";
New-Item MasterFile.txt; Set-Content MasterFile.txt "As of ${dateTime} files inside Testing Purpose are:
";
ForEach($item in $list){$nameText = $item.FullName; $nameText = $nameText -replace " ", ""; Add-Content MasterFile.txt "${nameText}"}
```

<br>

7. Delete all files that start with *TypeA*

```powershell
$list = Get-ChildItem -Recurse | where {$_ -isnot [System.IO.DirectoryInfo]}; foreach($item in $list){if($item.Name -like "TypeA*"){Remove-Item $item.FullName}}
```

<br><br>

#### **Exercise 2: One-liners**

1. Get all services which are stopped.

```powershell
Get-Service | where{$_.Status -eq "Stopped"} | Format-Table Status, Name
```

<br>

2. Get all services whose name starts with letter "A".

```powershell
Get-Service -Name "A*"

or

Get-Service | where{$_.Name -like "A*"}
```

<br>

3. Get all services which are set to start automaticlly (look for property StartType : Automatic).

```powershell
Get-Service | Where-Object{$_.StartType -eq "Automatic"} | Format-Table StartType, Status, Name
```

<br>

4. Restart-Service Winmgmt

```powershell
Restart-Service -Name Winmgmt
```

<br>

5. Export the service name and status into a text file.
   Example:

        "
        Service Name, Status
        Service A, Running
        Service B, Stopped
        "

```powershell
Get-Service | Format-Table @{Label="Service Name"; Expression={$_.Name}}, Status | Out-File .\Services.txt
```

<br>

6. Export the service name, StartType, and status into an HTML file.

```powershell
Get-Service | ConvertTo-Html -Property @{Label="Service Name"; Expression={$_.Name}}, StartType, Status | Out-File Table.htm
```

#### **Exercise 3: Windows Process Related**

1. Get all windows processes whose name starts with letter "A"

```powershell
Get-Process -Name "A*"
```

<br>

2. Get list of processes whose name is svchost and PM more than 100MB

```powershell
Get-Process -Name svchost | Where{$_.PM -gt 100000000}
```

<br>

3. Get Process Name, Process ID and handleCount whose PM is more then 100MB and CPU more than 1000s

```powershell
Get-Process | Where{$_.PM -gt 100000000 -and $_.CPU -gt 1000} | Format-Table Name, @{Label="Process ID"; Expression={$_.Id}}, HandleCount
```

4. Export the results of (3) to html and CSV format
   
```powershell
Get-Process | Where{$_.PM -gt 100000000 -and $_.CPU -gt 100} | Select-Object -Property Name, @{Label="Process ID"; Expression={$_.Id}}, HandleCount | Export-Csv -Path .\4-Table.csv;
Get-Process | Where{$_.PM -gt 100000000 -and $_.CPU -gt 100} | ConvertTo-Html -Property Name, @{Label="Process ID"; Expression={$_.Id}}, HandleCount | Out-File 4-Table.htm
```

<br>

#### **Practice Exercise 4: User Input**

Simple interest calculation on any principal amount involves variables like Principle amount, Interest rate and tenure for which you are calculating SI

Simple interest can be calculated by using below formula

SI = P * R * T / 100

where,

P = Principal amount

 R = Rate of Interest

T = Time(in years)

Write a PowerShell to take user inputs and show the results user

```powershell
$Prcp = Read-Host "Principal amount: ";
$RtI = Read-Host "Rate of interest: ";
$TmYrs = Read-Host "Number of years: ";
$SmpInt = [double]$Prcp * [double]$TmYrs * [double]$RtI/100;
echo $SmpInt
```

<br>

#### Practice Exercise 5:  Programming using PowerShell

Write a logic using nested loops(for loop or while loop) to draw the below pattern

    #
    ##
    ###
    ####
    #####
    ######
    #######
    ########
    #########
    ##########
    ###########

```powershell
for ($i = 0; $i -lt 11; $i++)
{
    for ($j = 0; $j -lt $i; $j++)
    {
        Write-Host "#" -n
    }
    echo ""
}
```

### Intermediate

#### **Problem 1: Service restart on multiple computers and logging**

Write a PowerShell script to read the computer names from a text file Then,

1. Stop a given service (say Print Spooler service ) and wait for 30 seconds after logging the status into a dedicated log file.
2. Ensure no child process is alive so that graceful stop of service can be confirmed
3. If there is any child process, kill it forcefully and log the information into the log file
4. After waiting for 30 seconds, start the service
5. Check for the service status, log into the log file and come out.

Next level,  You can see the above process might take around 2 minutes of time for gracefully restarting a service on 1 server.  using a single thread, It is not a good solution if we have to gracefully restart 5 services on 1000 machines(ETA: 10,000 minutes).

So, Use multithreading to improvise the solution. (hint: Invoke-Command or Start-Job might help you here)

```powershell

```

<br>

#### **Problem 2: Event Viewer**

Write a quick PowerShell script which,

-> Read multiple server names from a text file

-> Ask the user to specify which log they want to scan- like Application, System etc

-> Upon providing input, ask for event ID which they are looking for

Once this information is provided, Script should scan all the computers event logs for provided eventID and generate a nicely formatted CSV report whose headers should be:

 "MachineName","TimeGenerated","Source","Message"

 

Problem 3: Task Scheduler
There are scheduled tasks Important_Service_Restart and Important_Job_Processing running on 100 machines(you have their names in a text file). Tasks are scheduled to run on the daily basis and they are critical for your business.

You need to write a PowerShell script to run after 15 minutes of task scheduled time and collect the status into a CSV file(each task, each computer).

Next, Read the CSV file and get the name of servers where the last run of the task was failed.

Servers on which one task was failed, action should be sending an email to the support asking them to look into this urgently.

Servers on which both tasks were found in failed status, send an email to support and send a separate email to management informing them about the severity of the situation.

Next, Once your standard solution is ready, improvise your solution to decrease the overall script execution time(hint:; use multi threading)

 

Problem 4: Daily Backup
You have a folder in your computer(say C:\Important\My Coding Practice ).

 

Since you do some code changes on daily basis, you want to set up a daily backup using PowerShell.  Preferred time for backup is 10PM.  The format of backup should be .zip with the date appended in the name.

 

When you are in office, your preferred backup location is a shared folder which is accessible to you by UNC path(say \\XYZ_CORPS\share\associates\personal\).

 

But not all the time, you stay in office till 10 PM, and UNC path is not available outside of office environment. In that case, you have to take a local backup to C:\Archives. Whenever you are in office at the time of scheduled backup, your local backup should also be moved to UNC path as it is safer.

 

Your space in shared folder at work is limited, so you want to ensure that no more than last 30 backups are available at the backup directory.

Please find a solution using Windows PowerShell and Windows task Scheduler