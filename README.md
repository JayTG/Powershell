# Powershell Examples
[Sample Scripts Documentation](https://docs.microsoft.com/en-us/powershell/scripting/samples/sample-scripts-for-administration?view=powershell-7.2)

## Excute Script 
```powershell 
.\<script>
``` 

## Output
```powershell
Write-Host "Hello World"

Write-Output "Hello World"
```

Write-Host outputs data directly to the host and not along the pipeline 

Write-Output has its output continue down the pipeline 

## User input
```powershell
$name = Read-Host "What is your name"
```
## Password Creation
Create a password
```powershell
$pass = Read-Host "Enter Password" -AsSecureString
```
```-AsSecureString``` to avoid displaying to screen and storing securely 

Output the password
```powershell
[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringTOBSTR($pass))
```
# Powershell Exercises
## Practice Exercise 1: File Operations/Searching/Basic PowerShell Operators

1.)	Create a folder TestingPurpose and 3 Subfolders inside it SubFolder1, SubFolder2

**Answer:** 
``` powershell
  mkdir TestingPurpose
  cd TestingPurpose
  mkdir SubFolder1
  mkdir SubFolder2
```

2.)	Create some test files inside these folders:

TypeATest1.txt, TypeATest2.txt  … TypeATest50.txt into SubFolder1

TypeBTest51.txt, Purpose52Test2.txt … TypeBTest100 into SubFolder2

Needless to say that you have to use logic for creating these files. Not one by one

**Answer:**
```powershell
1..50 | foreach {New-Item -path ~/TestingPurpose/SubFolder1/TypeATest$_.txt}

1..100 | foreach {New-Item -path ~/TestingPurpose/SubFolder2/TypeBTest$_.txt}
```

3.)	Move all files which have an odd number in its name to SubFolder2

**Answer:** 
```powershell
1..50 | % {if($_ % 2 -eq 1) {Move-Item -path ~/TestingPurpose/SubFolder1/TypeATest$_.txt ~/TestingPurpose/SubFolder2/TypeATest$_.txt}}
```
4.)	Move all files which have even number in its name to SubFolder1

**Answer:**
```powershell
1..100 | % {if($_ % 2 -eq 0) {Move-Item -path ~/TestingPurpose/SubFolder2/TypeBTest$_.txt ~/TestingPurpose/SubFolder1/TypeBTest$_.txt}}
```

5.)	Rename folder SubFolder1 to EvenFilesContainer and SubFolder2 to OddFilesContainer

**Answer:**
```powershell
Rename-Item C:\Users\username \TestingPurpose/SubFolder1 C:\Users\username\TestingPurpose/EvenFilesContainer

Rename-Item C:\Users\username \TestingPurpose/SubFolder2 C:\Users\username\TestingPurpose/OddFilesContainer
```

6.)	Prepare a list of all files currently existing inside folder TestingPurpose

Example: MasterFile.txt:
As of YYYYMMDD HH: MM files inside Testing Purpose are:
C:\testingPurpose\EvenFilesContainer\TypeBTest2.txt
.
.
C:\testingPurpose\OddFilesContainer\TypeATest99.txt

**Answer:** 
```powershell
Get-ChildItem -Recurse  ~/TestingPurpose
```

7.)	Delete all files which start with TypeA

**Answer:**
```powershell
Get-ChildItem -Recurse  ~/TestingPurpose | Where-Object Name -Like '*`TypeA*'| ForEach-Object { Remove-Item -LiteralPath $_.Name}
```

## Practice Exercise 2:  Windows Service Related

**Write PowerShell One-liners for:**

1.)	Get all services which are stopped

**Answer:**
```powershell
Get-Service | Where-Object {$_.Status -eq "Stopped"}
```

2.)	Get all services whose name starts with letter "A"

**Answer:**
```powershell
Get-Service -Name "A*"
```

3.)	Get all services which are set to start automatically (look for property StartType : Automatic)

**Answer:** 
```powershell
Get-Service | Select-Object -property StartType, Name| Where-Object {$_.StartType -eq "Automatic"}
```


4.)	Restart-Service Winmgmt

**Answer:** 
```powershell
Restart-Service -Name winmgmt
```

5.)	Export the service name and status into a text file.

**Answer:** 
```powershell
Get-Service | Select-Object -property Name, Status| Out-File .\Service.txt
Get-Content Service.txt    
```

6.)	Export the service name, StartType, and status into an HTML file.

**Answer:**
```powershell
Get-Service | Select-Object -property Name, StartType, Status| Out-File .\Service2.html
```

## Practice Exercise 3:  Windows Process Related

**Write PowerShell One-liners for:**

1.) Get all windows processes whose name starts with letter "A"

**Answer:** 
```powershell
Get-Process -Name "A*"
```

2.) Get list of processes whose name is svchost and PM more than 100MB

**Answer:** 
```powershell
Get-Process -Name "*svchost*" | Where-Object {$_.PM -GT 100MB}
```
3.) Get Process Name, Process ID and handleCount whose PM is more then 100MB and CPU more than 1000s


**Answer:** 
```powershell
Get-Process | Where-Object {$_.PM -GT 100MB} | Where-Object {$_.CPU -GT 1000}
```
4.) Export the results of (3) to html and CSV format


**Answer:**
```powershell
Get-Process | Where-Object {$_.PM -GT 100MB} | Where-Object {$_.CPU -GT 1000} | Out-File ./Process.html
Get-Process | Where-Object {$_.PM -GT 100MB} | Where-Object {$_.CPU -GT 1000} | Out-File ./Process.csv
```
## Practice Exercise 4:  User Input
Simple interest calculation on any principal amount involves variables like Principle amount, Interest rate and tenure for which you are calculating SI
Simple interest can be calculated by using below formula

SI = P * R * T / 100

P = Principal amount

R = Rate of Interest

T = Time(in years)

Write a PowerShell to take user inputs and show the results user

**Answer:**
```powershell
$P = Read-Host "Enter Principal amount"
$R = Read-Host "Enter Rate of Interest"
$T = Read-Host "Enter Time(in years)"
$S = $P*$R*$T/100
Write-Host "Simple interest equals: " $S
```


 

