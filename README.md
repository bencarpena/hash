# hash

This is a tool to "hash" a string using ToBase64String Method. 
It augments the hash by incorporating a user-specified key to encode and decode via a custom subroutine that uses ToCharArray method. 
The final output is rendered as a string and uses a bespoke String-to-Binary and Binary-to-String function.

This tool is the commandline utility version whereas its predecessors are web and desktop apps.

### Other notes:
Encoding is the process of transforming a set of Unicode characters into a sequence of bytes. 
Decoding is the process of transforming a sequence of encoded bytes into a set of Unicode characters.'

### Compile and packaging:
- C# Compiler from Terminal: csc Program.cs
- Execute using mono runtime from Terminal (ECMA Common Language Infrastructure): mono Program.exe 


### References:
- https://docs.microsoft.com/en-us/dotnet/api/system.text.asciiencoding?view=net-5.0
- https://docs.microsoft.com/en-us/dotnet/api/system.convert.tobase64string?view=net-5.0

### Author:
- @bencarpena
