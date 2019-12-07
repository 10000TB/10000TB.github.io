---
layout: "post"
title: "Random notes to my experience with \"u-root\" - \"A fully Go userland with Linux bootloaders\""
categories: engineering
tags: [go, userland, engineering]
time: 10
---

![go mascot](https://blog.golang.org/gopher/header.jpg)
(image credit: [The Go Blog - The Go Gopher](https://blog.golang.org/gopher))

I accidentally had a chance to work with a fully go userland - `uroot` ( [github](https://github.com/u-root/u-root), [official website](http://u-root.tk/) ), not long ago. I am not sure if I will work further in the area that interacts with it closely. Out of the fear of losing value of all the time I have invested in the area, it occur to me that I might just write down my experience with it, put together a summary, and check point what I have learnt at this right moment when my memories about it are fresh. Maybe it will be useful for future myself in case I come back to the area around uroot or linux boot, or for other people reading this post.

## What is uroot ?

### What is `userland` ?

To understand `uroot`, I think one first need to get some context on what `userland` is. `Userland`, or `user space`, refers to everything (programs, libraries, or else ?) that runs outside kernel. Usually operating system uses userland software to interacts with kernel in order to achieve tasks like input/output tasks, file system objects manipulation, etc. So softwares delegated to do those tasks are part of userland.

A few more useful reading materials:  

* [User space](https://en.wikipedia.org/wiki/User_space)
* [What's the difference of the Userland vs the Kernel?](https://unix.stackexchange.com/questions/137820/whats-the-difference-of-the-userland-vs-the-kernel)

### What is 'uroot' then ?

`uroot` is "A fully Go userland with Linux bootloaders! u-root can create a root file system (initramfs) containing a busybox-like set of tools written in Go". Check out its [github page](https://github.com/u-root/u-root) for more details around what it is mainly used for, and what it can do, and how you can get started with `uroot`.

I worked briefly on an ipv6 network boot server ([dhcp](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol), [DHCPv6](https://tools.ietf.org/html/rfc8415)), and spent quite some time browsing [`boot`](https://github.com/u-root/u-root/tree/master/cmds/boot) code, and many [DHCP client modules](https://github.com/u-root/u-root/tree/master/pkg/dhclient). In particular, the [`cmds`](https://github.com/u-root/u-root/tree/master/cmds) folder holds implementations for
command line utils. Take a peek at [`cmds/core`](https://github.com/u-root/u-root/tree/master/cmds/core) and you will know what I am referring to :). If you live and breathe in Terminal or Terminal-ish environment day to day, you will see a lot of familiar faces, and remember that they all implemented in `golang`. Some of them:

```
basename
cat
chmod
chroot
cmp
comm
cp
cpio
date
dd
df
...
(check out https://github.com/u-root/u-root/tree/master/cmds/core for a full list)
```

A few more useful reading materials:  

* [USENIX 2015 ATC Paper](https://www.usenix.org/system/files/conference/atc15/atc15-paper-minnich.pdf)
* [USENIX 2015 ATC Talk](https://www.usenix.org/conference/atc15/technical-session/presentation/minnich)
* [uroot official website](http://u-root.tk/)

## My experience

Quite bit of my time was spent on investigation or learning. The problem:

(In some scenario under some specific design requirements) A network boot server does not return an ipv6 address in DHCPv6 `REPLY` message, while the dhcp client it interacts with (and which sends dhcp `SOLICIT` request to server) explicitly ask for that, and would fail the boot if without it.

The final solution (or more accurate: workaround, IMO) was to have the boot process (actually, the dhclient process) boot further regardless the existence of an ipv6 address in `REPLY` message.

The actual code is one line, :).

See my pull request message below and link to original PR on github:

```
pxeboot: continue to boot when lease failed

* When lease configuration failed, there is
still a default ip/ipv6 address available
locally, which was auto configured before
pexclient can send request packet. There
are usecases where DHCP server does not
assign IP and prefer local hardware based
address to be picked up. So updating pxeboot
to acommendate that.

* Arguably, it may be good to always have IP
assigned from server. This CL does not intend
to address the ask/argument about what is
best, but to update pxeboot to provide the
flexibility to the particular usecase mentioned
above.

* On a separate note, it maybe good to retry
lease configuration a few times upon failures
and then give up to boot further.

Signed-off-by: David Hu xuehaohu@google.com

```
(The pull request: https://github.com/u-root/u-root/pull/1382/commits/a92f2eb335b92cec8a4959870afb839f103908c5)


## Other related organizations / projects / useful resources

* Linuxboot - https://www.linuxboot.org/
* Contributors - http://u-root.tk/#contributors
* Setup guide - http://u-root.tk/#u-root
* Replace UEFI with Linux - https://schd.ws/hosted_files/osseu17/84/Replace%20UEFI%20with%20Linux.pdf
