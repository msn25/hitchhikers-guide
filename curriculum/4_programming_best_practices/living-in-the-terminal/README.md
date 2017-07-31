# Living in command land, or:<br/>how I learned to stop worrying and love the terminal

## Where to start?
Navigating online (stackoverflow) and offline (--help & the man pages).


Tip! The (DuckDuckGo.com)[https://duckduckgo.com] search engine returns StackOverflow answers as the first-hit-preview ;) 

## Basic commands
`mv <move_me>`
`rm <remove_me>`

### Caveats for git users
`git mv <move_me>`
`git rm <remove_me>`

### What's in my folder?
`ls`
`ls -l`
`tree`
`tree -L 2`
`tree -hs`

### What's in my file?
`head -n10 $f`
`tail -n10 $f`
`tail -n10 $f | watch -n1`
`tail -f -n10 $f`

Counting words and lines (`wc` == "word count")...
`wc $f`

### Where is my file?
`find -name "<lost_file_name>" -type f`
`find -name "<lost_dir_name>" -type d`

## Functions
We can write functions in shell scripts as well!
The syntax looks like this...
```
function_name(args) {
    function_body
}
```

You can even define shell functions inside your ~/.bashrc profile when a simple alias just won't do...
For example, run a jupyter notebook remotely through an SSH tunnel and forward the connection to your localhost:
`jupyter_local() { ssh -i ~/.ssh/<key>.pem -NfL "$1":localhost:"$2" <user>@<host>; }`

Then we can just write...
`jupyter_local 8888 8889`
...to run a jupyter server on `<host>` (@ port 8888) and view it on our local machine (@port 8889)


## Working remotely via SSH
When working via SSH, a connection interruption can terminate your running scripts, lose your environment varaibles and lose your command history! D: There's a way to avoid this. Actually there's two: `screen` and `tmux` are two programs that allow you to run a terminal session remotely on a remote server which you can interact with from your own machine via SSH. So if you ever lose connection to the server, your terminal session is still running - you just have to log back into it. You can also run multiple independent terminal sessions on the same server this way.

### tmux<br/>(aka: terminal multiplexer)
```
# ssh into the server
ssh user@IP
# Open a tmux session
tmux
# List existing sessions
tmux ls
# Attach (a) to a target session (-t #)
tmux a -t 1
# Create a new pane
Ctrl+b+c
# Rename the current pane
Ctrl+b+,
# Kill the current pane
Ctrl+b+x
```

If you search "tmux cheatsheet" on (DuckDuckGo.com)[https://duckduckgo.com], the preview search result reveals some more useful commands [:


## Bonus points
### Rogue terminals
We all make mistakes. Sometimes we make mistakes in infinite loops.
What do we do when "Ctrl+C" is not enough?
`top` or `htop` allow us to see what processes are running on our computer. (cf. System Monitor @ Mac)
Every process has an ID (`pid`) which we can use to send a `kill` command to it.

```
ps -ef | grep badprocess | awk '{print $2}' | kill `xargs $1`
```

Sometimes badprocess spawns other badprocess processes... so we can loop over them all.
```
ps -ef | grep badprocess | awk '{print $2}' | for f in `xargs $1`; do kill $f; done
```

### Parallel programming (sort of)
Run parallel processes on a multi-core system using GNU parallel. Typically, High-Performance Computing clusters have multi-cores (think quad-quad-quad-core), but running your script on the HPC is not enough to exploit it. What if you could run your script multiple times across each of the cores?

### Custom prompts
You can customise your command prompt by changing the $PS1 variable.