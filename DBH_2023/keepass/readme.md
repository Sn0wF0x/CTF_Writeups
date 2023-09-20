Warning: This is a German CTF so Stuff in English might not make Sense from Time to Time, I will try to explain it.
===================================================================================================================


CTF: KeePass

Description: tldr; Try to Crack the Password Database

Files included: passwords.kdbx, KeePass.dmp

Hint: none

==================================================================================================================

You start with two files:

/passwords.kdbx
/KeePass.dmp

All you literally have to do is searching for "Keepass Password Dumper" 

https://github.com/vdohney/keepass-password-dumper

And do The Setup for it:

Setup

    Install .NET (most major operating systems supported).
    Clone the repository: git clone https://github.com/vdohney/keepass-password-dumper or download it from GitHub
    Enter the project directory in your terminal (Powershell on Windows) cd keepass-password-dumper
    dotnet run PATH_TO_DUMP

This will give you the Flag but you need the 2 Last Digits of it

In this case its "<3"

Finished! 
