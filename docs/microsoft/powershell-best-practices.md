---
title: Powershell Best Practices
date: 2019-02-05
categories:
    - Powershell
tags:
    - Powershell
---    

# Powershell Best Practices

An aggregation of various recommendations

* Start script with a standard set of comments such as name, date, purpose, etc. 
* Use #Requires to set minimum PS version required, modules, etc. 
* Use Set-StrictMode -Version Latest to prevent the use of unitialised variables (similar to Option Explicit in VBScript) 
* Use simple but meaningful variable names.  Camel Case is "the best practice" 
* Use code signing 
* Don't use aliases, use full cmdlet anmes and named parameters 
* Don't use backticks 
* Filter left, format right 
* Add .exe for any external commands/apps being called 
* Use structured error handling (with Try, Catch, Finally/Trap) 
* Use cmdletBinding and add support for -Whatif, Verbose, Debug 
* Use Advanced Functions 
* Use Verb-Noun naming for functions, etc.  For the Verbs, use a standard verb (Get-Verb lists these) 
* Use standard parameter naming (ie. ComputerName is good, Machine or Server is bad) and set to a default value if relevant/possible (ie. $ComputerName = $ENV:ComputerName) 
* A function should do  thing 
* A function should return an object or an array of objects, not formatted text 
* Avoid using the Return keyword.  Functions automatically return output to the calling process via the pipeline 
* Use write-output over write-host (write-host only writes to the host, not the pipeline) 
* Use comment-based help 
* Use Test-Connection to check if a target is online 

* Use full cmdlet names 
* Use named parameters 
* Avoid write-host 
* Use CIM cmdlet (not WMI cmdlet) 
* For Verb-Noun, use a singular form 
* Use approved verbs 
* Use Whatif/Confirm parameters 
* Use custom folding regions 
* Use Set-StrictMode 
* Avoid Out-Null 
* Specify the extension for applications (ie. .exe) 
* Execute scripts with -NoProfile Parameter 
* Check parameters with PSBoundParameters (PSBoundParamters is a hashtable of parameters passed to a script of function) 
* Use #Requires 

* Use approved verbs for commands (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7.1)
* Don’t use special characters in cmdlet names (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#cmdlet-names-characters-that-cannot-be-used-rd02)
* Don’t use common parameter names for your parameters (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/common-parameter-names?view=powershell-7.1)
* Support “Confirm” reqests.  If the cmdlet modifies the system, it should allow the use of confirmation (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.* 1#support-confirmation-requests-rd04)
* Support “Force” parameter for interactive sessions (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#support-force-parameter-for-interactive-sessions-rd05)
* Document the objects that are written to the pipeline/outputted by your cmdlet (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#document-output-objects-rd06)
* Derive from the Cmdlet or PSCmdlet Classes (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#derive-from-the-cmdlet-or-pscmdlet-classes-rc01)
* Specify the Cmdlet Attribute (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#specify-the-cmdlet-attribute-rc02)
* Override an Input Processing Method (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#override-an-input-processing-method-rc03)
* Specify the OutputType Attribute (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#specify-the-outputtype-attribute-rc04)
* Do Not Retain Handles to Output Objects (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#do-not-retain-handles-to-output-objects-rc05)
* Handle Errors Robustly (https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/required-development-guidelines?view=powershell-7.1#handle-errors-robustly-rc06)

## References

* https://blogs.technet.microsoft.com/pstips/2014/06/17/powershell-scripting-best-practices/
* http://powershell-guru.com/best-practices/