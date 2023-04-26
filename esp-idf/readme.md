create shortcut with
`"C:\Program Files\PowerShell\7\pwsh.exe" -noexit -command "& 'C:\Users\<username>\esp\esp-idf\export.ps1'"`

put in export.ps1 that was already created by the esp-idf setup:
in the beginning
`Set-Alias -Name python C:\Users\<username>\.espressif\python_env\idf5.0_py3.8_env\Scripts\python.exe`
at the end
`Start-Process -FilePath "C:\Program Files\VSCodium\VSCodium.exe"`
