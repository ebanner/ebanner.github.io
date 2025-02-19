---
layout: post
title:  "How I Bastardize Make"
---

I use make for a quick and easy way to write alias shell scripts to run code for a project e.g.

```
run:
    python3 main.py
```

This way I don't have to remember how to run a project each time - I just call make (similar to `docker run`)

# Configure

Sometimes I need to run some initialization code to set up my shell to run the code e.g.

```shell
source venv/bin/activate
```

My first attempt was to do something like this:

```
run:
    python3 main.py

configure:
    source venv/bin/activate
```

While this seems nice, you can't just run `make configure` because `make` launches a subshell.

Instead you need to run the following:

```shell
$ source <(make configure)
```

and set up the Makefile this way:

```
run:
    python3 main.py

configure:
    @echo source venv/bin/activate;
```

This has the Makefile echo out the configure commands and you can source them
into your shell.

Set it up as an alias in your `.zshrc`:

```bash
alias configure='source <(make configure)'
```

That way you can just run:

```shell
$ configure
```

and then:

```shell
$ make

 * Serving Flask app 'main'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 164-893-481
 ```

 Voila!
