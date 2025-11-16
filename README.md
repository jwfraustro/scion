# scion
**The Self-Escalating R-Shell Installer for GreyHack**

Designed for the [Arborist Suite](https://github.com/jwfraustro/arborist) rshell manager.

Scion is a lightweight, modular tool for automating the process of creating and escalating R‑Shells in Grey Hack.
It can be used as a standalone component or as part of a larger automation tool (such as [Rootstock](https://github.com/jwfraustro/rootstock) or [Arborist](https://github.com/jwfraustro/arborist)).

Scion will:
* Detect whether the provided shell already has root permissions
* Automatically escalate permissions using [Chainsaw](https://github.com/jwfraustro/chainsaw) if necessary.
* Install Metaxploit on the remote system through a temporary RCE stub.
* Start an rshell connection ("graft") to any targeted IP/port.

**Note:** Scion is not designed for command-line usage and must be incorporated in a calling script.

## What's Scion?
A [scion](https://en.wikipedia.org/wiki/Grafting) is a *young shoot* in a tree and the starting point of your automated rshell system.

It can be grafted onto a Rootstock hub for management by the Arborist system, or used standalone.

```
Scion    Scion    Scion   Scion <--- RShells
    \    /          \     /
   Rootstock        Rootstock   <--- Hubs (Optional)
       \__\          _/_/
          \\        //
           \\      //
           Arborist             <--- RShell Manager (Optional)
              |:|
           ../|:|\.-.
```

## Installation
It is highly recommended that you clone this repository and use the [Greybel](https://github.com/ayecue/greybel-js) CLI tool or the [Greybel VSCode extension](https://github.com/ayecue/greybel-vs) to build the Scion source and import it into Grey Hack.

The [chainsaw](https://github.com/jwfraustro/chainsaw) dependency is provided as a git submodule with this repository.

To clone the entire repository including submodules, you may use the following:

```
git clone http://github.com/jwfraustro/scion --recurse-submodules
```

I really don't suggest downloading or copying the source code manually, as it may lead to headaches when it comes to sorting out dependencies.

## Usage
Scion is designed to be controlled by another script and **does not run from the command line**.

To use Scion, you must import the `scion.src` module into your Grey Hack script. You will need to instantiate a `Scion` object, providing it with an existing shell object to the remote system and an rshell IP address and port for it to "graft" to.

### Basic Example
```miniscript
import_code("scion.src")

scion = new Scion
scion.rshell_ip = "1.1.1.1"     // your own IP, or a hub IP
scion.rshell_port = 1222
scion.init(remote_shell) // a pre-existing shell object on the target system
scion.graft()
```

### How it works
1. Scion checks if the shell it has been provided hs root permissions.
2. If not, it will attempt to find any writeable directory and use Chainsaw to escalate permissions.
3. Once root permissions are confirmed, Scion will create a temporary RCE stub to download and install Metasploit on the target system.
4. Finally, Scion will start an rshell connection to the provided IP/port.

## Support Notice
Scion is provided as-is. I no longer play Grey Hack, and future maintenance is unlikely. I cannot guarantee that Scion will work with future versions of Grey Hack. This project is simply provided as a reference for those interested.

Fun fact: Scion, Rootstock, and Arborist were used to create a network of over 7000 rshell connections in the creation of my (now defunct) GreyHack Network Map project.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.