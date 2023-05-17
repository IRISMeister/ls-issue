# ls-crash
Ls crashes immedialtly when class contains non ascii character.

It happens on Windows10 + WSL2 + VSCode WSL extention ( https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl ).

If I do the same on Windows10, without WSL2, it works fine.

# Symptom
[A class without Japanese letter](src/User/ASCII.cls) works as expected. For example, it shows you related document tip when you hover over %Status.
If you open a [class](src/User/JP.cls) which contains Japanese letter, LS crashes.  The same seems to apply to, say, [Germany](src/User/DE.cls).

```
[Error - 10:52:19] The InterSystems Language Server server crashed 5 times in the last 3 minutes. The server will not be restarted. See the output for more information.
```

Full output from InterSystems Language Server is [here](./full-dump.txt).

# How to reproduce
```
Open wsl2 terminal and then,
$ git clone https://github.com/IRISMeister/ls-issue.git
$ cd ls-issue
$ docker compose up -d
$ code .
open src/User/ASCII.cls.  Hover ovwe to %Status and see if doc pops up.
Then, open src/User/JP.cls (or DE.cls) and it will crash LS.
```

If you import and compile them, you can run it correctly.

```
$ docker compose exec iris iris session iris "##class(ASCII).Run()"
a
$ docker compose exec iris iris session iris "##class(JP).Run()"
あ
$ docker compose exec iris iris session iris "##class(DE).Run()"
ü
$
```
Username and pasword is SuperUser / SYS .
