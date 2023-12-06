# POSIX "Shell Command Language"  
IEEE Std 1003.1-2017 / Open Group Base Specifications Issue 7, 2018 edition  
https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html  


Grammar and Shell Commands (§2.9)
---------------------------------

### Basic Grammar (§2.9.1 - 2.9.3)

Due to `sh`'s complex history, its syntax can seems idiosyncratic, with
semicolons or quotation being required in the ostensibly unlikeliest of places.
What helped me, at least, was to appreciate the following hierarchy:

The basic grammar consists of simple commands (with optional variable
assignments and fd redirections), optionally combined into pipelines,
optionally combined into and-or lists, optionally combined into single-line
command lists, optionally combined into multi-line "compound" command lists.
These compound lists form the basis of the flow-controling compound commands,
summarized in the next section.

```
SIMPLE_COMMAND: one or more WORDS followed by a CONTROL_OPERATOR (see table)
                 - WORDs undergo WORD EXPANSION (see below). If any fields remain,
                   the first field is considered the command name and the remaining
                   fields are considered the arguments for the command.
                 - the WORD sequence may be preceded by VARIABLE ASSIGNMENTs: FOO=bar WORD
                 - the WORD sequence may be preceded or succeeded by REDIRECTIONs: 2>&1
PIPELINE:       one or more SIMPLE_COMMANDs separated by `|`
                 - the PIPELINE may begin with `!` in which case its return value
                   will be logically negated: if the last command returns 0, the
                   pipeline will return 1; if the last command returns non-zero,
                   the pipeline will return 0.
AND-OR_LIST:    one or more PIPELINEs separated by `&&` or `||`
LIST:           one or more AND-OR_LISTs separated by `;` or `&`
COMPOUND_LIST:  one or more LISTs separated by `<newline>` characters
```

| CONTROL_OPERATOR   | Function                                                   |
|:------------------:|------------------------------------------------------------|
|  `;` / `<newline>` | run the command (or list) sequentially                     |
|  `&`               | run the command (or list) asynchronously in a subshell     |
|  `\|`              | establish a pipeline between the first and second command  |
|  `&&`              | run the second command if the first succeeded              |
|  `\|\|`            | run the second command if the first failed                 |


--------------------------------------------------------------------------------

### Compound Commands (control flow) (§2.9.4)

Compound commands allow the structuring of COMPOUND_LISTs into common control
flow paradigms.  
POSIX sh offers: `(`, `{`, `for`, `case`, `if`, `while`, and `until`

#### `(`
```sh
( COMPOUND_LIST )
```
Run COMPOUND_LIST in a subshell; any changes to the environment (variables,
working directory...) will not affect the parent shell.

--------------------------------------------------------------------------------

#### `{`
```sh
{ COMPOUND_LIST CONTROL_OPERATOR }
```
Run COMPOUND_LIST in the current process. Note the required CONTROL_OPERATOR
(typically `;` or `<newline>`). I fail to see why a parser couldn't identify
the closing brace without a CONTROL_OPERATOR, but it doesn't--neither in the
spec, nor in the implementations.

--------------------------------------------------------------------------------

#### `for`
```sh
for NAME [in [ WORDS ] ; ] do COMPOUND_LIST done
```
Set NAME to each item in WORDS, in turn, and execute COMPOUND_LIST. If "WORDS"
is omitted (e.g., due to expansion), COMPOUND_LIST is not executed. If " `in`
WORDS " is omitted, it is equivalent to specifying " `in "$@"` ":

```sh
# Example ('in WORDS' omitted)
echo_words() { for word do echo $word ; done }
echo_words XYZ "abc 123"
```
...will output:
```
XYZ
abc 123
```

--------------------------------------------------------------------------------

#### `case`
```sh
case WORD in
    [(] PATTERN1 [ | PATTERN2] ... ) COMPOUND_LIST ;;
    [(] PATTERN3 [ | PATTERN4] ... ) COMPOUND_LIST ;;
```
Executes the COMPOUND_LIST corresponding to the first PATTERN that matches.  
PATTERNs are subject to: tilde expansion, parameter expansion, command
substitution, and arithmetic expansion (see below)  
Pattern matching details: ...

```sh
# Example (argument parsing)
while [ "$#" -gt 0 ]; do
    case "$1" in
        -h|--help)
            echo "Usage: foo [-h|--help] [-a|--option-a]..."
            exit
            ;;
        -a|--option-a)
            option_a=1
            ;;
        --)
            shift
            break
            ;;
        -*)
            echo "Error: Unknown option: $1" >&2
            exit
            ;;
        *)
            break
            ;;
    esac
    shift
done
```

--------------------------------------------------------------------------------

#### `if`
```sh
if COMPOUND_LIST then COMPOUND_LIST [ elif COMPOUND_LIST then COMPOUND_LIST ] [else COMPOUND_LIST ] fi
```
Execute the `if` COMPOUND_LIST; if the exit status is zero (success), execute
the corresponding `then` COMPOUND_LIST. If the exit status was non-zero,
perform the same logic on each optional `elif` branch in turn. If all are
non-zero, execute the optional `else` COMPOUND_LIST.

--------------------------------------------------------------------------------

#### `while`
```sh
while COMPOUND_LIST do COMPOUND_LIST done
```
Run the antecedent COMPOUND_LIST. If the return value is zero (success), run
the consequent COMPOUND_LIST and start over; otherwise (return value is
non-zero), processing of the `while` immediately completes.

--------------------------------------------------------------------------------

#### `until`
```sh
until COMPOUND_LIST do COMPOUND_LIST done
```
Same as `while` but the success/failure check for the antecedent COMPOUND_LIST
is inverted -- i.e., `until COMPOUND_LIST ...` = `while ! COMPOUND_LIST ...`

--------------------------------------------------------------------------------

Special Built-Ins (§2.14)
-------------------------

```sh
# flow related
break [n]       # exit from the [n]th-enclosing for, while, or until loop
continue [n]    # return to the top of the nth enclosing for, while, or until loop
return [n]      # exit from a function or dot-script with return value "n"
                # if n is not specified, return value is that of the last command
exit [n]        # exit shell with return value "n", traps on EXIT executed first
trap            # TODO
:               # do nothing ("pass")

# variables
set                   # TODO. cf $-
unset [-f] name       # unset the variable (or, with -f, the function) "name"
export name[=word]    # set the export flag for a variable
readonly name[=word]  # set the readonly flag for a variable (cannot change/unset)
shift [n]             # shift the positional parameters n slots (default 1)
                      # 1 = 1+n, etc., with $# updated to the new no. of pos. params

# misc
times               # print accumulated times for shell and its children (cf times(1))
.                   # source commands from a file
exec [cmd] [redir]  # replace current shell with command "cmd", with redirections "redir"
                    # if no command is specified, redirections are added to current process
                    # e.g., exec 3< readfile / exec 4> writefile
eval                # construct a command by concatenating and expanding arguments
```

### eval example
```sh
baz=bar bar=foo x='$'$baz
echo '$x'
eval echo '$x'
eval eval echo '$x'
```

**Output:**
```output
$x
$bar
foo
```


Parameters and variables (§2.5)
-------------------------------

```
Positional parameters: $1 .. $n (use braces for >9: ${10})
Special parameters:
   $*: expands to $1 .. $n (where n = $#) -- all concatenated together
   $@: expands to "$1" .. "$n" (where n = $#) -- grouping preserved
   $#: expands to the number of positional parameters (0 is not counted)
   $?: exit status of the most recent PIPELINE
   $$: PID of the current shell [cf. $PPID]
   $!: PID of the most recent command executed in the background (&)
   $-: option flags that sh was invoked with or set with "set"
   $0: name of the shell or shell script
Shell variables: $HOME, $IFS, $PPID...
```

### Illustrating the differences between `$*` and `$@`
```sh
star_vs_at_expansion() {
    echo 'Using $* unquoted:' # concatenates with $IFS, then resplits every word:
    for arg in $*; do         # arg1 / arg2 / with / spaces / arg3
        echo "$arg"
    done
    echo 'Using $* quoted:'   # concatenates with $IFS, then doesn't split:
    for arg in "$*"; do       # arg1    arg2 with spaces arg3
        echo "$arg"
    done
    echo 'Using $@ unquoted:' # same as $*, resplits every word
    for arg in $@; do
        echo "$arg"
    done
    echo 'Using $@ quoted:'   # retains original groupings:
    for arg in "$@"; do       # arg1 /    arg2 with spaces / arg3
        echo "$arg"
    done
}
star_vs_at_expansion "arg1" "   arg2 with spaces" "arg3"
```

Expansion (§2.6)
-----------------

```
    tilde                 ~[user]
    parameter             ${expression}
    command substitution: $(expression) executed in a subshell
    arithmetic:           $((expression))
```

### (§2.6.2) parameter expansion: ${expression}
```
${parameter-word} - (use a default value) if parameter is unset, substitute word
${parameter=word} - (assign a default value) if parameter is unset, assign it word
${parameter+word} - (use an alt. value) if parameter is unset, substitute null; otherwise, substitute word
${parameter?word} - (throw an error) if parameter is unset, throw an error; word is a custom error message
                    but see "unset variable handling" below
    optionally, a ":" can prefix the -=?+ operators, making null values behave the same as unset

${#parameter}: the number of characters in the value of $parameter
```

```sh
unset paramater
echo "${parameter-"default value"}"    # output: default value
echo "${parameter="new value"}"        # output: new value [plus assignment happens]
echo "${parameter+"alternate value"}"  # output: alternate value
```

```sh
# unset variable handling (set -u)

$ unset FOO
$ echo $-
smi             # bash: himBH
$ echo $FOO

$ set -u
$ echo $-
usmi            # bash: himuBH
$ echo $FOO
sh: FOO: parameter not set
```


## bash
Bash extensions.
* [[ conditional expressions ]]

## zsh
Zsh extensions.