So I can run my own PowerShell script...

`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Process`

The **Process** scope only affects the current PowerShell session. When the PowerShell session is closed, the value is deleted.

<https://docs.microsoft.com/en-gb/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7>
