# DATAMINERWATCH
###### *A bash script designed to look after Xolokram Primeminer when mining Datacoins* 

## Why? For what?

When I have started mining primecoins on my PC, occasionally two thing were happening: miner hangs during nighttime hours and (much much rarer) miner crashes. Well, there are better things to do at night than watching miner output, so I have searched the net and surprisingly didn't found any useful solution. 

So I have written one myself: [https://github.com/arkhebuz/primewatch](https://github.com/arkhebuz/primewatch). This is fork for Datacoin mining, as both coins are miner-compatibile. Not a cutting edge programming but address three things:
* Is Internet connection working at all? Script periodically pings Google DNS and if errors are returned pings four more servers and check's connection carrier, then writes info to logfile if less than half of packets are received.
* Is miner running at all? If not, launch again.
* Did miner hang somewhere at connection after all? I had hangs lasting an hour or two with lots of `force reconnect if possible!` or `system:111` communicates (on my box usually one of them was clearly dominating, not both at the same time), without any `[MASTER]` line printed to output. Script simply compares line numbers of last `force reconnect if possible!` and `system:111` communicates with line number of last `[MASTER]` communicate. If `[MASTER]` is not the last one, primeminer is killed, launched (if in jump mode against another pool) and info is written to logs.

## Quick how-to
* I'm assuming you have primeminer binary. If not, google it, check peercointalk.org forum, etc.
* Get dataminerwatch script, either copy-paste from this site or use `git clone https://github.com/arkhebuz/datawatch` command. Use `chmod +x datawatch.sh` when necessary.
* **Edit** datawatch.sh file, all this is commented in code. You need to:
  1. Change catalog where logs will be stored;
  2. Set your network interface virtual filesystem catalog (like `/sys/class/net/eth0`);
  3. Set interval in seconds between checks. Should be large enough to let the miner recover under it's own steam in most cases. Also, too small will make script steal cpu cycles from miner;
  4. Set primeminer binary location;
  5. Edit primeminer launch parameters. This script is written with [http://dtc.xpool.xram.co](http://dtc.xpoll.xram.co) and [http://dtc.gpool.net](http://dtc.gpool.net) pools in mind, see my comments in code and check their sites;
* Launch script, pass to it which pool and mode it should use, like `./primewatch.sh gpool stay`. There are two modes: "stay" and "jump". When stay is used, script doesn't change the pool on connection hang. When jump is used, script will jump from one to another on hangs, constantly mining to the same DTC address. You can just leave the terminal on, or use `screen`, or put script in autostart, or do something else. I prefer the second option.

## Quirks
* Every primeminer run has it's own logfile with it's output, named witch launch time (YYYY-MM-DD_hh.mm.ss). Additionaly there's a `netlog` file, where only communicates from script about connection are stored.
* Dependencies? Primeminer, pkill, pgrep, bash, grep... Nothing special.
* Aha, works for me. May not work for you. Tested on Debian Stable.
* Donate? Just a tip maybe?  
   DTC: DMy7cMjzWycNUB4FWz2YJEmh8EET2XDvqz  
   XPM: AV3w57CVmRdEA22qNiY5JFDx1J5wVUciBY  
