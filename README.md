# Dingus

Dingus is a simple tool by and for the folks at Assemble to ease management of environment variables. Dingus supports two ways of applying environment variables (through the `print` and `session` subcommands), whichever you'd prefer is up to you. In the process of doing so, Dingus will set and increment the `DINGUS_LEVEL` environment variable so that it's possible to track the number of nested sessions you might be in.

### Requirements

Dingus is written in the Rust and is available on [crates.io](https://crates.io) to build from source. It is also available as a Homebrew Tap.

##### Homebrew Installation

```sh
brew tap davidarmstronglewis/dingus
brew install dingus
```

### Using Dingus

Dingus has some nice built in help messages in case you forget, but here's a quick tutorial regardless.

This file should exist at `~/.config/dingus/example_1.yaml` with the following contents:

```yaml
HELLO: Hello World!
MULTI_LINE: "Hello there,
How are you?"
```

This file should exist at `~/.config/dingus/example_2.yaml` with the following contents:

```yaml
HELLO: Hello, Dingus Session!
```

#### Implicit Config Files

__As of version 0.4.2__ Dingus will search upwards, recursively, for a `.dingus` Yaml file if no `--config` file is specified. Just don't commit it to source control if you have secrets to keep.


#### Dingus List Example

__As of version 0.4.0__ it's easy to see what config files you have available. Try running `dingus list` or `dingus ls` to see options you can supply to the `--config` parameter of the Print and Session subcommands. 

#### Dingus Print Example

Run `dingus print -c example_1`. See how `dingus` found the `example.yaml` file we created, read its contents, and printed out a command? That command can be piped into `source -` to set those variables directly in your current shell session. Neat, huh? `dingus` knows what shell you're running by looking at your `$SHELL` variable and printing out a command for that shell's syntax. I've only tested this in the `fish` and `bash` shells, so I don't know if I've got it right for all shells (actually, I know I haven't). If this doesn't work properly for you let me know what sytax I should be using for your shell and I'll toss it in there.

The full command to apply the variables to your shell is `dingus print -c example_1 | source -`. Normally it's discouraged to pipe anything into `source -` since it can open up remote code execution vulnerabilities, but you're not doing this on a production server so it's cool (right?).

Check it out: `echo $HELLO` and `echo $MULTI_LINE` both contain the values you set in the file `~/.config/dingus/example_1.yaml`.

#### Dingus Session (Shell) Example

In case you don't want to pollute your current shell session with environment variables, `dingus` also supports opening a new session for you. By default, `dingus` will use whatever command your `$SHELL` variable refers to and assume that's a valid shell to place you into.

Try running `dingus session -c example_2`. You're now in a new shell session. Try `echo $HELLO`. Yep, we've applied the variables from `~/.config/dingus/example_2.yaml`, which are now all accessible. Also available are any variables you set before entering the Dingus session, so if just ran the example in our "Dingus Print Example" section you'll find that `$MULTI_LINE` is still available.

__As of version 0.3.7__ Dingus will also accept `shell` when trying to invoke this subcommand. The semantics were close enough that it made sense to alias the two.

