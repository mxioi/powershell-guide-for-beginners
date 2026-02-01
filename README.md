# powershell-guide-for-beginners
There are so many guides out there to learn powershell. However this is cheat sheet from my personal learning journey of Power Shell


PowerShell Learning Cheat Sheet (Foundations)
This is a practical, mental‑model‑first guide based on everything I’ve learned so far. Use it as a reference, not something to memorise. It’s designed especially for moments when you’re not sure what a command does, how to use it, or what it outputs.

1. Core PowerShell Rule (Most Important)
PowerShell works with OBJECTS, not text.
    • Commands output objects
    • Pipes pass objects
    • You filter and select object properties
If you remember nothing else, remember this.

2. What Is a Command / Cmdlet?
A cmdlet (command‑let) is a built‑in PowerShell command that: - Follows Verb‑Noun naming (e.g. Get-Service, Start-Service) - Returns objects - Can be explored and learned interactively
Examples:
Get-Service
Get-Process
Get-ChildItem
Think: > “A cmdlet gives me objects I can work with.”

3. Your 3 Most Important Learning Tools
If you are unsure what a command does, how to use it, or what it outputs, always start here.
🔹 Get-Help
Get-Help Get-Service
Use when: - You don’t know what a command does - You’re not sure what parameters exist
Most useful forms:
Get-Help Get-Service -Examples   # BEST starting point
Get-Help Get-Service -Syntax
Get-Help Get-Service -Full
Get-Help Get-Service -ShowWindow # easier to read
Think: > “How do I use this command?”

🔹 Get-Member
Get-Service | Get-Member
Use when: - You don’t know what properties an object has - You don’t know what you can do with the object
This shows: - Properties (things the object has) - Methods (things the object can do)
Think: > “What does this object contain?”

🔹 Get-Command
Get-Command Get-Service
Use when: - You want to check if a command or function exists - You want to see if your function loaded correctly

4. Discovery Workflow (How to Learn Any Command)
Whenever you meet a new command, follow this exact flow:
Get-Thing
Get-Thing | Get-Member
Get-Help Get-Thing -Examples
Ask yourself: - What object is this? - What properties does it have? - What can I filter or select?

5. The Pipeline Mental Model
Command1 | Command2 | Command3
Means: > Take each object from Command1, pass it one‑by‑one into Command2, then into Command3.
PowerShell pipelines pass objects, not text.

6. $_ — The Current Object
Inside a pipeline scriptblock:
Where-Object { $_.Property -eq 'Value' }
$_ means: > “The object currently flowing through the pipeline.”
Mentally replace $_ with: > “this object”

7. The Golden Filtering Pattern
You will use this everywhere:
Command |
Where-Object { $_.Property -Operator Value }
Examples:
Get-Service | Where-Object { $_.Status -eq 'Running' }
Get-Service | Where-Object { $_.Name -in 'Spooler' }

8. Common Comparison Operators
Operator	Meaning
-eq	equals
-ne	not equal
-in	value is in a list
-contains	list contains value
-and	logical AND
-or	logical OR

9. Arrays (Lists)
Single value:
'Spooler'
Multiple values:
@('Spooler','WSearch')
Used with -in:
Where-Object { $_.Name -in @('Spooler','WSearch') }

10. Selecting Output (Formatting Last)
Always filter before selecting:
Get-Service |
Where-Object { $_.Status -eq 'Running' } |
Select-Object Name, Status
Think: > Filter → Shape → Display

11. Functions (Basic Structure)
function Verb-Noun {
    [CmdletBinding()]
    param(
        [string[]]$Name
    )

    # logic here
}
Rules: - Verb‑Noun naming - Parameters become variables - [CmdletBinding()] = advanced function

12. Parameters vs Variables
    • Parameter: input from the user
    • Variable: data you create/store internally
Example:
Get-ServiceStatusByName -Name 'Spooler'
Inside the function:
$Name  # already exists, passed in

13. if ($Name) — Truthiness
This means: > “Did the user supply a value for $Name?”
PowerShell truth rules: - Non‑empty string → TRUE - $null → FALSE - Empty array → FALSE

14. foreach Loop
foreach ($item in $collection) {
    # do something with $item
}
Meaning: > Take each object in the collection, one at a time.

15. -WhatIf and Safe Actions
Some cmdlets support ShouldProcess.
Start-Service Spooler -WhatIf
In functions:
[CmdletBinding(SupportsShouldProcess)]

if ($PSCmdlet.ShouldProcess($target)) {
    # action
}
Think: > “Preview before making changes.”

16. Error Handling
try {
    Do-Something -ErrorAction Stop
}
catch {
    Write-Warning $_.Exception.Message
}
Rules: - -ErrorAction Stop makes errors catchable - $_ in catch = error object

17. Script Scope vs Session Scope (Important)
To load a function from a script:
. .\script.ps1
(dot + space)
This imports functions into your session.

18. One‑Liner Example You Built
Get-Service |
Where-Object { $_.Name -in 'Spooler' } |
Select-Object Name, Status
This is clean, correct PowerShell.

Final Mental Checklist
Before writing PowerShell, ask: 1. What command gives me the object I need? 2. What object am I dealing with? 3. What properties does it have? (Get-Member) 4. Do I need to filter? 5. Do I need to loop? 6. Am I formatting too early?
