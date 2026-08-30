- ## Why Julia
	- Terse syntax
	- Easy to write, similar to mathematics
	- Syntactic sugar
	- Less boilerplate
- ## Installing Julia
	- Installation instructions for Windows, Linux and macOS can be found at [julialang.org/downloads](https://julialang.org/downloads/)
		- This will install the [Juliaup](https://github.com/JuliaLang/juliaup) installation manager, which will automatically install Julia and help keep it up to date. The command `juliaup` is also installed. To install different julia versions see `juliaup --help`.
		- `juliaup` is what the Julia project recommends on every platform, and it is what we use; it installs Julia for one user, under `~/.juliaup`
		- distribution packages (`apt`, `snap`) also exist, and install Julia systemwide, which is occasionally what you want on a shared machine; they tend to lag behind, so if you use one, check `julia --version` against the current release before assuming a package will work
	- Remark that `juliaup` keeps Julia up to date by itself; this is convenient, but it also means the version under you can change between one session and the next, and a package that worked may stop working. We will see in the next lectures how an environment records the exact versions a piece of work needs, which is the answer to this problem
- ## The Julia REPL and initial setup
	- The basic way of interfacing with Julia is the REPL, the **Read-Eval-Print Loop**
	- At a first glance, it looks like a fancy calculator
	- Before going further, I would like to install and setup the system in a way that I found comfortable, and may work for you too
	- write `julia` on the command line, I will teach you two ways of installing packages
		- ### The package prompt
			- the first one opens the shell package manager, press `]`
			- the prompt color changes to blue, we are in **Package mode**, it is used to install packages into an environment; to get back from the pkg prompt to the REPL, just press `backspace`
			- type `add OhMyREPL`, which is a package that colors the output of the REPL so it is easier to read it; remark that the REPL and the shells in Julia support `TAB`-autocomplete
		- ### Explicitly importing Pkg
			- another way to install packages is by using the `Pkg` package, that allows us to install packages, change environments and so on from inside Julia
			- in the REPL, the one with the `julia >` prompt, write `import Pkg`
			- this means that we are bringing the `Pkg` name in scope inside the REPL (but not its subnames)
			- Now, we can call `Pkg.add("BenchmarkTools")`; this installs a package to do Benchmarks
			- Since we only imported the `Pkg` name in scope, we need to specify the function using the dot notation
			- I use this form when I want to avoid polluting my namespaces; this matters less in Julia than in many languages, not because of types as such but because multiple dispatch lets two packages export a function of the same name and still be told apart by their arguments, but it remains a good practice
			- Instead of `import` we could have used `using Pkg` that imports the name `Pkg` and all the names exported by `Pkg`
			- We install one last package `Pkg.add("Revise")`, this package allows Julia to recompile functions on the fly when we change them in a loaded package
	- Now, for me it is a good practice to start these packages as Julia starts, therefore, we make a file
		- `nano ~/.julia/config/startup.jl` on linux
		- on windows we should do the same in `C:\Users\USERNAME\.julia\config\startup.jl` with text editor
		- add `using Revise, OhMyREPL, BenchmarkTools` and save the file `CTRL-O + CTRL-X`
		- Documentation on the startup file https://docs.julialang.org/en/v1/manual/command-line-interface/#Startup-file
- ## Installing VSCode
	- Probably VSCode is the best thing that Microsoft has done in many years (maybe forever)
		- https://code.visualstudio.com/
	- On Ubuntu
		- `snap install code`
		- On windows, we just go to the page above and download it
	- Install the Julia extension, that is going to take care of the interface between VSCode and Julia
		- If you have any problem, probably is the fact that the extension is not able to find the julia executable
		- With `juliaup` the executable is `~/.juliaup/bin/julia`; with a distribution package it is wherever that package puts it, and `which julia` will tell you
- ## First steps in VSCode/Julia
	- We make a directory, I called it `~/Code/HSI`
	- Good practice: when we work on a new project, whether its coding a package or experiments, it is good to make a new environment, so you avoid loading unnecessary packages for your actual work
	- Open VSCode, open folder, go to the folder you created
	- Press `F1` and start the Julia REPL
	- To create a new environment in the directory
	- `import Pkg; Pkg.activate("./")`
	- in the REPL we install the `Plots` package, by using `Pkg.add("Plots")`
	- Now we also change the environment for VSCode, by clicking on Julia env in the bottom state menu and choosing our working directory; this is to make VSCode aware of the packages we installed in the environment
	- Remark that using environments comes with zero cost: julia only downloads and compile one copy of the package that is then made available to the environments
- ## Keyboard shortcuts worth knowing
	- The **command palette** is the one to remember: every command in VSCode can be found there by name, so you never have to memorise the rest
		- `F1` works on every platform; `Ctrl+Shift+P` on Windows and Linux, `Cmd+Shift+P` on macOS
		- type `Julia` in it to see everything the extension can do
	- The Julia extension binds the **same keys on every platform**; there are no separate macOS bindings. On a Mac, `Alt` is the `Option` (`⌥`) key, and `Ctrl` stays `Ctrl`, it does not become `Cmd`
		- | what it does | keys |
		  |---|---|
		  | Start the Julia REPL | `Alt+J` `Alt+O` |
		  | Restart it (after changing an environment, say) | `Alt+J` `Alt+R` |
		  | Stop it | `Alt+J` `Alt+K` |
		  | Run the current line or selection | `Ctrl+Enter` |
		  | Run it and move to the next line | `Shift+Enter` |
		  | Run the current cell | `Alt+Enter` |
		  | Interrupt a computation that is taking too long | `Ctrl+C` |
		  | Change the active environment | `Alt+J` `Alt+E` |
		  | Show documentation for what is under the cursor | `Alt+J` `Alt+D` |
		  | Show the plot pane | `Alt+J` `Alt+P` |
		- the `Alt+J` combinations are chords: press `Alt+J`, release, then press the second combination
	- In the REPL itself, three keys change the prompt, and they are worth practising
		- `]` enters package mode, `?` enters help, `;` enters shell mode, and `backspace` on an empty line returns to `julia>`
- ## Compiler vs Interpreter
	- ### Compiled Languages (C, C++, Fortran, …)
		- A **compiler** translates your entire program (written in a high-level language) into **machine code** once, before execution.
		- After compilation, you can run the program many times without recompiling.
		- Advantages:
			- Runs very fast (since the machine code is already prepared).
		- Disadvantages:
			- Slower development cycle: you must compile before running, and errors are often reported only after the compilation step.
			- Code tends to be more rigid: you must carefully specify types and follow strict rules.
	- ### Interpreted Languages (Python, Perl, …)
		- An **interpreter** reads your program line by line and executes it directly, without producing a separate machine-code file.
		- Every time you run the program, the interpreter does this translation again.
		- Advantages:
			- Faster to test ideas: just write and run.
			- Error messages are often easier to understand.
			- More flexibility in coding style.
		- Disadvantages:
			- Slower execution, because every run involves the overhead of interpretation.
	- ### Julia: Just-In-Time (JIT) Compilation
		- Julia takes a **hybrid approach**.
		- The **first time** you run a function, Julia compiles it into optimized machine code using LLVM (a compiler backend).
		- That compiled version is then stored in memory (and sometimes on disk), so **subsequent runs are as fast as C or Fortran**.
		- Advantages:
			- Interactive experience like Python (you can write and test quickly).
			- Performance close to compiled languages once functions are compiled.
		- Trade-off:
			- The first run of a function may feel slower because of the compilation step (“time-to-first-plot problem” is a famous example).
- ## Types
	- An important concept, when dealing with programming is the concept of **type**
	- A **type** is a label that tells Julia (and you) what kind of data something is, like “an integer number,” “a decimal number,” or “a complex number.”
	- In practice a **type** describes both the *kind of data* (like integers, floating-point numbers, strings, arrays, functions) and how Julia should store it in memory and choose the right method to run when you call a function.
	- In Julia, everything has a type. Types tell Julia how to represent data in memory and which version of a function to run (this is called **multiple dispatch**).
	- Types are arranged in a hierarchy, and this is what lets us write a function once and use it with numbers of many kinds
		- ```julia
		  julia> Float64 <: AbstractFloat <: Real <: Number
		  true
		  julia> supertype(Float64)
		  AbstractFloat
		  ```
		- the operator `<:` reads "is a subtype of"; `Float64 <: Real` says that every `Float64` is a `Real`
	- The distinction that matters is between **concrete** and **abstract** types
		- a concrete type, such as `Float64` or `BigFloat`, describes how a value is actually laid out in memory, and values have concrete types
		- an abstract type, such as `Real` or `Number`, has no layout of its own; it exists to group the concrete types under it, so that we can say what a function accepts without saying exactly which representation we mean
		- ```julia
		  julia> isconcretetype(Float64), isconcretetype(Real)
		  (true, false)
		  ```
	- This is why we will be able, in the next lectures, to take a function written for ordinary floating point numbers and run it on numbers that carry a rigorous error bound with them; the function was never written for `Float64` in particular, it was written for anything that behaves like a number
- ## First use of Julia
	- In the REPL now we can use Julia to make some simple computations
	- We can use Julia as a simple calculator
		- ```julia
		  julia> 1.0 + 1.0
		  2.0
		  ```
	- It is worth starting to think about types
		- ```julia
		  julia> typeof(1.0)
		  Float64
		  ```
		- ```julia
		  julia> typeof(1)
		  Int64
		  ```
		- ```julia
		  julia>  typeof(1.0+0.3*im)
		  ComplexF64 (alias for Complex{Float64})
		  ```
	- As you can see Julia implicitly identifies the types of the objects: Julia is a typed language and under the hood it uses types to identify and dispatch the right function
	- What is the difference between Julia and Python?
		- **Just in time** compilation
		- We define a function
			- ```julia
			  T(x) = 4*x*(1-x)
			  T (generic function with 1 method)
			  ```
		- What is going to happen when we run this function?
			- ```julia
			  julia> T(0)
			  0
			  julia> @code_warntype T(0)
			  MethodInstance for T(::Int64)
			    from T(x) @ Main REPL[8]:1
			  Arguments
			    #self#::Core.Const(T)
			    x::Int64
			  Body::Int64
			  1 ─ %1 = (1 - x)::Int64
			  │   %2 = (4 * x * %1)::Int64
			  └──      return %2
			  ```
		- When we ran this function Julia identified that the input was an integer, checked if it had available all the functions needed to compile this function with an integer input and compiled a version of this function and stored it in memory (recently, it also stores them on disc, to avoid recompilation); this means that the **the first time** we ran a function Julia is going to incur in overhead, but the **second time** it is going to have access to a compiled and often optimized version of the function which is going to be really fast
		- Functions that start with an `@` are called **macros** and are functions that act on Julia source code before compilation. The macro `@code_warntype` is used to analyse the Julia code and identify eventual type instabilities
		- The macro `@time` computes the runtime of a function
		- In the REPL, we can access the help prompt by pressing `?`
		- We ran the example in the help
- ## Declaring more complex functions in julia
	- We open a file and add the following code
		- ```julia
		  function orbit(T, x0::Float64, N)
		     orb = zeros(Float64, N)
		     x = x0
		     for i in 1:N
		        orb[i] = x
		        x = T(x)
		     end
		     return orb
		  end
		  ```
	- We would like to be able not to worry about the input type, but types are something we can work with in Julia, to do so we use the `typeof` function
		- ```julia
		  function orbit(T, x0, N)
		     orb = zeros(typeof(x0), N)
		     x = x0
		     for i in 1:N
		        orb[i] = x
		        x = T(x)
		     end
		     return orb
		  end
		  ```
	- This is similar to what is achieved in C++ by using templates, but it is simpler to use
- ## Multiple dispatch, and why the generic version works
	- When we write `x = T(x)` inside `orbit`, Julia has to decide which `*` and which `-` to run, and it decides by looking at the types of the arguments; a function in Julia is a name with several **methods** attached to it, and the method is chosen by the types of all the arguments, not only the first
	- We can see how many methods a common operation has
		- ```julia
		  julia> length(methods(*))
		  ```
		- there are hundreds; multiplying two `Float64` and multiplying two matrices are different pieces of code sharing one name
	- This is the mechanism behind the generic `orbit` we just wrote. We never told it what kind of number to use: `zeros(typeof(x0), N)` makes storage of whatever kind `x0` is, and `T(x)` selects the arithmetic that belongs to that kind
	- The consequence is worth stating plainly, because the rest of the school depends on it: to compute the same orbit in a different arithmetic we do not modify `orbit`, we pass it a different number
- ## Plotting the histogram of an orbit
	- We would like to plot the histogram of an orbit, to visualize the frequency of visits of the dynamical system  $T:[0,1]\to[0,1]$, $T(x)=4x(1-x)$, in different parts of the interval
	- Since we already have implemented the orbit function this is quite easy
	- We need to install the Plots package (it takes some time)
		- ```julia
		  Pkg.add("Plots")
		  using Plots
		  ```
	- Now, we define our dynamical system and compute an orbit that starts at the point $x_0=0.1$
	- It is worth remarking that this is a numerical experiment
		- ```julia
		  T(x) = 4*x*(1-x)
		  v = orbit(T, 0.1, 10000)
		  plt = histogram(v, normalize = :pdf, label = "Empirical", bins = 100)
		  plt
		  ```
	- In this case the density distribution of orbits is known
	  $$g(x)=\frac{1}{\pi \sqrt{x(1-x)}}$$
	- ```julia
	  g(x) = 1/(pi*(sqrt(x*(1-x))))
	  plot!(plt, g, 0, 1, color=:orange, label = "Density")
	  ```
	- The command plot! allows us to plot on top of an already made plot
- ## How much of that orbit was real?
	- We have just plotted ten thousand points and compared them with a density, and the agreement is good; it is worth asking whether the orbit we computed is the orbit we intended to compute
	- `BigFloat` is a floating point type of arbitrary precision, and it is in Julia by default, so we need install nothing. We run the **same function**, with the same starting value, changing only the kind of number we hand it
		- ```julia
		  setprecision(BigFloat, 256)
		  a = orbit(T, 0.1, 120)             # ordinary floating point, 53 bits
		  b = orbit(T, BigFloat(0.1), 120)   # the same code, 256 bits
		  typeof(a), typeof(b)
		  (Vector{Float64}, Vector{BigFloat})
		  ```
		- note that the two arrays have different types, and that we did not write a second `orbit` to obtain the second one
	- Comparing them term by term, the two agree at first and then stop agreeing
		- ```
		  n= 10   difference = 5.4e-15
		  n= 20   difference = 1.3e-11
		  n= 30   difference = 8.5e-09
		  n= 40   difference = 4.8e-06
		  n= 50   difference = 1.2e-02
		  n= 55   difference = 4.8e-01
		  ```
		- by `n = 54` the two disagree by more than `0.1`; one says `x = 0.2144`, the other says `x = 0.0523`, and the interval has length one, so they agree about nothing at all
	- While the difference is small it grows by about a factor of two at every step, which is what it means for this map to be chaotic; starting from the smallest error a `Float64` can carry, about `1e-16`, some fifty doublings are enough to reach the size of the whole interval
	- After that it stops growing, and this is worth noticing in the numbers above: at `n = 55` the difference is `0.48` and at `n = 60` it is `0.34`. Nothing is diverging, and nothing can, since both orbits remain in `[0,1]` for ever; the difference simply saturates at the size of the interval and then fluctuates
	- Saturation is not a consolation. A difference of order one on an interval of length one means the two computations agree about nothing, and there is no way to tell from the numbers themselves which of them, if either, is near the true orbit
	- Two conclusions follow
		- the orbit we plotted is not the orbit of `0.1`; after about fifty steps it is the orbit of some other point we cannot name
		- computing with more digits does not repair this, it only postpones it; the `BigFloat` orbit is wrong too, a few hundred steps later
	- The histogram, on the other hand, was not obviously wrong, and this is a real phenomenon rather than luck: the statistics of these orbits are stable even though the orbits are not. Saying that precisely, and proving it for a given map on a computer, is what the rest of the school is about, and it requires arithmetic that carries its own error bounds rather than arithmetic that discards them
- ## Summary of the lecture
	- We installed Julia and VScode
	- We installed some basic packages
	- We used the REPL and discussed types, the hierarchy they form, and multiple dispatch
	- We implemented a somewhat more complicated function
	- Made a small discussion on generic code
	- Plotted a histogram
	- Saw that the orbit behind it was not the one we asked for, and that more precision does not fix it
