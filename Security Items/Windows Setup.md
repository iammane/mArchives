# Windows Setup Guide

## Intro

Putting this together to highlight setting up a Windows machine to be used for ethical hacking / penetration testing **without** needing to install a VM in software such as Virtual Box or VMware.

The main justification for something like this is primarily speed and ease of use. There are tradeoffs though and anything that needs to be pointed out will be. This serves as a personal guide more than anything else but please feel free to use or suggest improvements!

## Initial Steps

First follow the steps here [Install WSL | Microsoft Learn](https://learn.microsoft.com/en-us/windows/wsl/install). This should be done prior to anything else. Run whatever updates are needed, reboot, just ensure the Windows Subsystem for Linux is installed and ready to go.

Once WSL is installed you have to install the distro of your choosing. You can actually do this in the Windows Store or from an admin PowerShell prompt (not covered here). For my purposes I'm installing the Kali Linux distro but truthfully any will work fine (Ubuntu, etc). Find your particular flavor in the Microsoft Store and click install. 

After the installation is completed you can run it - you'll be prompted to set up a username and password. It can match your Windows credentials, but it doesn't have to necessarily.

You should at this point be looking at a Linux prompt (from this point forward we'll assume Kali). Congratulations! 🎉 The initial setup is complete!

## Installation Items

The WSL version of Kali comes relatively bare bones. Luckily (in my testing at least) most tools can be installed separately and even ones with GUIs work! Before diving into any extra tools be sure to run a quick `sudo apt update` and `sudo apt upgrade`. Once that's set you'll want the following:

### Linux Installs
1. [Git](https://git-scm.com/) - It's possible Git is not installed by default. When I first wrote this guide it wasn't, but I've recently run through these steps and it was already there. In the event it's not then run `sudo apt install git` and you're off to the races.
2. [Legion](https://github.com/GoVanguard/legion) - This tool is a good combo of NMAP scanning + Hydra, brute forcing, screen shotting. It's a nice one stop shop with a GUI. `sudo apt install legion`. The cool part about this one is that once you invoke it you actually get the graphical window on your Windows desktop. **It must be run with sudo, ie, `sudo legion`**
3. [Spiderfoot](https://github.com/smicallef/spiderfoot) - This is a pretty decent tool for OSINT gathering. In all honesty, I haven't had a lot of luck with it, but I suspect it's mostly in my usage (or lack of understanding). `sudo apt install spiderfoot`.
4. [Go](https://go.dev/doc/install) - Many tools are written in Go, along with Python. Python is likely installed already, and also is likely already up to some flavor of version 3. However, by default, Go is not installed. Run the following to install and configure:

```
sudo apt install -y golang
export GOROOT=/usr/lib/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
# This may or may not be required, but the below will reload .bashrc
source .bashrc 
```
5. Verify that Python 3.x is installed. There may still be some tooling requiring Python 2.7, if at all possible find alternatives. If Python 3 is not installed for any reason simply run `sudo apt install python3`.
6. [sslyze](https://www.kali.org/tools/sslyze/) - Excellent cryptographic analyzer, used to see what cipher suites a web site / app will offer (this is good for a quick check). `sudo apt install sslyze`.
7. [testssl.sh](https://github.com/drwetter/testssl.sh) - Another comprehensive SSL/TLS analyzer, takes longer than sslyze but the information output is more detailed. `git clone --depth 1 https://github.com/drwetter/testssl.sh.git`.
8. [amass](https://github.com/owasp-amass/amass) - Network mapping of attack surfaces and external asset discovery using open source information gathering and active reconnaissance techniques. `sudo apt install amass`.
9. [Subfinder](https://github.com/projectdiscovery/subfinder) - More on reconnaissance, specifically for subdomain discovery. `sudo apt install subfinder`. 
10. xdg-utils and [Chromium](https://www.chromium.org/Home/) - Not 100% mandatory, but if you need to open up resources in the VM that are web based this is incredibly handy. This seems to be included in the base version of Kali from WSL install repo, though it's not functioning properly on my laptop. This is as of Jan 8th, 2024. Regardless, still handy to have:

```
sudo apt install xdg-utils
sudo apt install chromium
```

### Windows Installs
Some applications are going to be used natively on Windows as opposed to their Kali counterparts. This hybrid approach affords us the most flexibility. 

1. [NMAP](https://nmap.org/) - Network scanning tool - we'll elect to run this on Windows rather than WSL because we're going through one less hop. Depending on how the networking is configured for WSL (I've not looked into this) we also may avoid routing issues.
2. [Burp Suite](https://portswigger.net/burp) - Amazing website / web application testing tool. Inspect traffic, intercept requests and repeat them, change them, etc. We're going to run this on Windows rather than Kali so that we can integrate more natively with our normal web browser. This could very well be the free Community edition, or Professional, does not matter.
3. [ZAP Proxy](https://www.zaproxy.com/download/) - Another scanning tool with a focus on web app scanning (rather than traffic inspection/replay/modification, though it's capable of this as well). Automatic scanning options exist (un-auth) along with authenticated crawling and testing.
4. [Foxy Proxy](https://getfoxyproxy.org/) - Browser based plugin that makes switching proxies for your every day browser a breeze. This isn't mandatory, but makes it A LOT easier to switch between using Burp as a proxy and not. If there's a different variant you like by all means, roll with it.
5. [Wappalyzer](https://www.wappalyzer.com/apps/) - Fantastic browser plugin that will inspect site components and produce a report of different frameworks in use, security headers, libraries that are detected, CDNs, etc. This tool is very helpful in the information gathering phase.
6. [Go](https://go.dev/doc/install) - On the list again, this time we want it for Windows also. There's an MSI for installing on Windows.
7. [Python 3](https://www.python.org/downloads/) - Maybe even more important than Go, many tools are written in Python. As above, avoid 2.7.x, it's been EOL for some time. Any tools worth their weight have been updated to use Python 3.x. You can install Python from the Windows Store but in my own experience I've found the updating/reinstallation kind of unpredictable - just download the installer from the main site and use that.

## Fantastic Web Resources

These resources are web based tools that you can access from anywhere.

* [crt.sh](https://crt.sh) - Handy for crawling all associated certificates registered to a specific domain.
* [Qualys SSL Test](https://www.ssllabs.com/ssltest/) - Very good web based SSL server test, useful in seeing various kinds of information around the SSL/TLS configuration for a given site / web app.
* [Snyk Vulnerability Database](https://security.snyk.io/) - Excellent for searching into CVEs for additional information you won't normally get.
* [Ciphersuite Search](https://ciphersuite.info/) - Great tool for evaluating cipher suites and issues with ones presented by web applications.
* [CSP Evaluator](https://csp-evaluator.withgoogle.com/) - Quickly analyze the CSP for a given domain, or explicitly copy in a CSP for evaluation against benchmarks.
* [JWT Decoder](https://jwt.io) - Quickly decode any JWTs found in testing to see what kind of data is contained inside.
* [URL encoder](https://www.urlencoder.io) - Handy URL encoder to swap plain text to URL encoded text, useful for various requests and parameter testing.
