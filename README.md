- [Powershell](#powershell)
  - [Basic Commands](#basic-commands)

---

# Powershell

- Operates on objects, not text.
- Has `cmdlets` They are built on a common runtime rather than seperate executables. This provides a consistent experience in parameter parsing and pipeline behaviour.
- Has many types of commands
  - Native Executables
  - cmdlets
    - Named acording to the *verb-noun* structure
  - functions
  - scripts
  - aliases



## Basic Commands

```ps
#----------------------#
# 'More info' commands #
#----------------------#

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


get-alias command_name
# Returns the full name for the command that you've provided the alias for


get-member
```

```ps
#--------------------------------#
# Files, Folders and Directories #
#--------------------------------#


dir
# Returns all files and folder in current directory


cd path
# Change directory to provided path

cd ..
# Go up a directory
```

```ps
#--------#
# Basics #
#--------#


cls
# Clears the screen
```


```ps
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

```ps
#----#
#    #
#----#



```