# Assignment 09

Solved by the group Recursive Rebels.

## Exercises

PLC: 10.1, 10.2 and 10.3.

## Run instructions

```{}
fslex --unicode <LexerSpecification>.fsl
fsyacc --module <ParserName> <ParserSpecification>.fsy
dotnet fsi -r FsLexYacc.Runtime.dll ...
```

On MacBook

```{}
mono ~/bin/fsharp/fslex.exe --unicode <LexerSpefication>.fsl
mono ~/bin/fsharp/fsyacc.exe --module <ParserName> <ParserSpecification>.fsy
dotnet fsi -r FsLexYacc.Runtime.dll ...
```

## Handin details

Handin file name:

- BPRD-09-AMDH-EMNO-MBIA.zip

Files to handin:

- 
