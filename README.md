Fantomas
========

F# source code formatter, inspired by [scalariform](https://github.com/mdr/scalariform) for Scala, [ocp-indent](https://github.com/OCamlPro/ocp-indent) for OCaml and [PythonTidy](https://github.com/acdha/PythonTidy) for Python.

## Purpose
This project aims at formatting F# source files based on a given configuration.
Fantomas will ensure correct indentation and consistent spacing between elements in the source files.
We assume that the source files are *parsable by F# compiler* before feeding into the tool.
Fantomas follows the formatting guideline being described in [A comprehensive guide to F# Formatting Conventions](FormattingConventions.md).

## Use cases
The project is developed with the following use cases in mind:

 - Reformatting an unfamiliar code base. It gives readability when you are not the one originally writing the code. 
To illustrate, the following example

	```fsharp
	type Type
	    = TyLam of Type * Type
	    | TyVar of string
	    | TyCon of string * Type list
	    with override this.ToString () =
	            match this with
	            | TyLam (t1, t2) -> sprintf "(%s -> %s)" (t1.ToString()) (t2.ToString())
	            | TyVar a -> a
	            | TyCon (s, ts) -> s
	 ```
will be rewritten to

	```fsharp
	type Type = 
	    | TyLam of Type * Type
	    | TyVar of string
	    | TyCon of string * Type list
	    override this.ToString() = 
	        match this with
	        | TyLam(t1, t2) -> sprintf "(%s -> %s)" (t1.ToString()) (t2.ToString())
	        | TyVar a -> a
	        | TyCon(s, ts) -> s
	 ```

 - Converting from verbose syntax to light syntax. 
Feeding a source file in verbose mode, Fantomas will format it appropriately in light mode.
This might be helpful for code generation since generating verbose source files is much easier.
For example, this code fragment

	```fsharp
	let Multiple9x9 () = 
	    for i in 1 .. 9 do
	        printf "\n";
	        for j in 1 .. 9 do
	            let k = i * j in
	            printf "%d x %d = %2d " i j k;
	        done;
	    done;;
	Multiple9x9 ();;
	```	
is reformulated to 
	```fsharp
	let Multiple9x9() = 
	    for i in 1..9 do
	        printf "\n"
	        for j in 1..9 do
	            let k = i * j
	            printf "%d x %d = %2d " i j k
	
	Multiple9x9()
	```

 - Formatting F# signatures, especially those generated by F# compiler and F# Interactive.

For more complex examples, please take a look at F# outputs of [20 language shootout programs](tests/languageshootout_output) and [10 CodeReview.SE source files](tests/stackexchange_output).

## How to use
### Command line tool / API 
You can fork this repo and compile the project with F# 3.0/.NET framework 4.0. 
Alternatively, Fantomas is also available via [a NuGet package](https://nuget.org/packages/Fantomas/) which contains both the library and the command line interface.
For detailed guidelines, please read [Fantomas: How to use](Usage.md).

### Trying Fantomas online
[FantomasWeb](https://github.com/TahaHachana/FantomasWeb), implemented by [Taha Hachana](https://github.com/TahaHachana), is accessible at http://fantomasweb.apphb.com/.

### VS 2012 extension
[Ivan Towlson](https://github.com/itowlson) is developing Fantomas extension under [fsharp-vs-commands](https://github.com/itowlson/fsharp-vs-commands) project.

## Installation
The code base is written in F# 3.0/.NET framework 4.0. 
The solution file can be opened in Visual Studio 2012 and MonoDevelop or Xamarin Studio.
NuGet is used to manage external packages.
The [test project](src/Fantomas.Tests) depends on FsUnit and NUNit.
However, the [library project](src/Fantomas) and [command line interface](src/Fantomas.Cmd) have no dependency on external packages.

## Testing and validation
We have tried to be careful in testing the project.
There are 147 unit tests and 30 validated test examples, 
but it seems some corner cases of the language haven't been covered.
Feel free to suggests tests if they haven't been handled correctly.

## Limitations
Due to limited information in F# ASTs, beware of current drawbacks:

 - Inline comments and multiline comments are lost. Only XML doc comments are preserved.
 - Compiler directives are lost.

## Why the name "Fantomas"?
There are a few reasons to choose the name as such. 
First, it starts with an "F" just like many other F# projects. 
Second, Fantomas is my favourite character in the literature. 
Finally, Fantomas means "ghost" in French; coincidentally F# ASTs and formatting rules are so *mysterious* to be handled correctly.

## Getting involved
Would like to contribute? Read [the formatting conventions](FormattingConventions.md) and [the Usage notes](Usage.md) and discuss on [the issues](../../issues). You can fork the GitHub repository and send a pull request.

## Credits
We would like to gratefully thank the following people for their contributions.
 - [Eric Taucher](https://github.com/EricGT)
 - [Steffen Forkmann](https://github.com/forki)
 - [Jack Pappas](https://github.com/jack-pappas)

## License
The library and tool are available under Apache 2.0 license. 
For more information see the [License file](LICENSE.md).
