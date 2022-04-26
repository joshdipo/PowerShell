- [Powershell](#powershell)
  - [Features](#features)
  - [Usage](#usage)
  - [Pipelines](#pipelines)
  - [Objects](#objects)
  - [Basic Commands](#basic-commands)
  - [Exercises](#exercises)
    - [Beginner](#beginner)
      - [Exercise 1: File Operations/Searching/Basic Powershell Operators](#exercise-1-file-operationssearchingbasic-powershell-operators)
      - [Exercise 2: One-liners](#exercise-2-one-liners)

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

`$_` represents the current pieline object.

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



new-item file_name.file_extension
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

#### Exercise 1: File Operations/Searching/Basic Powershell Operators

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

#### Exercise 2: One-liners

1. Get all services which are stopped.

```powershell
Get-Service | where{$_.Status -eq "Stopped"} | Format-Table Status, Name
```

<br>

2. Get all services whose name starts with letter "A".

```powershell

```

3. Get all services which are set to start automaticlly (look for property StartType : Automatic).
4. Restart-Service Winmgmt
5. Export the service name and stattus into a text file.
   Example:
   "
    Service Name, Status
    Service A, Running
    Service B, Stopped
   "
6. Export the service name, StartType, and status into an HTML file.