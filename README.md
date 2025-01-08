# stutter
Force a process to pause its work every once in a while.

It's nicer than `nice`!

## Usage

<ul>

  **stutter** &nbsp; -p _PID_

</ul>

or: 

<ul>
  
  **stutter** &nbsp; _command_ &nbsp; \[ _command-arguments_ ... \]

</ul>

## Examples

1. Run the 'dd' command under stutter. (^C will cancel both stutter and dd.)
   ```
   stutter dd if=/dev/zero of=/dev/null status=progress
   ```
   
2. Cause existing process 8191 to stutter. (^C cancels stutter, not 8191.)

        stutter -p 8191

## Installation

It's a shell script, so just download [stutter][rawlink], mark it executable (`chmod +x`), and move it to your bin directory (`/usr/local/bin/stutter`).

  [rawlink]: https://github.com/hackerb9/stutter/raw/refs/heads/main/stutter "Download stutter shell script"

## Background

The `nice` command gives a hint to UNIX systems that certain processes should use less CPU. The Linux kernel additionally offers `ionice` and `chrt` which affect how often the process can access resources like the disk. However, those tools have often proven insufficient. Background processes which have been set as nice as possible can interfere with high priority tasks, such as streaming video or even the user interface. If you meet a processes which is needlessly impolite even after you've asked it to be nice, then this program is for you. Stutter forces a process to pause repeatedly (100 times a second by default) using UNIX signals. 

This is particularly useful for daemons launched from cron which should not interfere with any interactive users. 

## How it works

Technically, it uses SIGSTOP/SIGCONT with a 10% active duty cycle and 0.1s period. You can adjust the on/off time by editing [the script](stutter).

```bash
# A reasonable default is to have the process active for 0.01s and
# sleep for 0.09s. (A 10% duty cycle with a period of 0.1s).
ontime=0.01
offtime=0.09
```

