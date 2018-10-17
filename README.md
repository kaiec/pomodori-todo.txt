## Pomodori-todo.txt

A pomodoro counter implementation for [Todo.txt](http://todotxt.com/).

## What it features

* easily plan, run timers, and show current tasks status via the command line
* allows tmux integration to display the remaining pomodoro timer in your tmux statusbar
* logs the pomodori start time and end time to a log file for an easy history
* uses the terminal-notifier gem to notify you when your pomodoro starts and ends

## Installation

* Make sure you have ruby installed
* run `gem install rainbow terminal-notifier`
* Download this repo somewhere on your filesystem
* symlink the `pom` executable into your TODO_ACTIONS_DIR
* optionally set environment variable POMODORO_SECONDS to the length of
  a pomodoro session. Defaults to 1500 (25 minutes).

## Usage:

	todo.sh pom action [task_number] [args]
	Actions:
	  ls
	     lists tasks with pomodori count and estimates
    log
       displays a log of your pomodori
	  start ITEM#
	     start a pomodoro timer for item ITEM#
	  add ITEM#
	     add one pomodoro to task on line ITEM# without running a timer
	  plan ITEM# PLANNED_POMODORI
	     estimate ITEM# will take PLANNED_POMODORI to complete

## Example:

<img src="https://raw.github.com/metalelf0/pomodori-todo.txt/master/screenshot.png">

## tmux integration:

Add `set -g status-right "#(cat ~/.pomo.txt.tmux)"` to your `.tmux.conf`. This
will put the time remaining in the current pomodoro in the right side of your
status bar. See `man tmux` for further details on customizing this status.

## i3blocks integration:

Set environment variables to configure process signalling:

```
POMODORO_SIG_SIGNAL=3
POMODORO_SIG_PROCESS=i3blocks
```

Now when pom is invoked and every second when a timer is running, 
signal 3 is sent to the i3blocks process. 

A possible blocklet looks like this (it uses the tmux integration):

```
#!/bin/bash
if [ -f ~/.pomo.txt.tmux ] && [ "$(($(date +%s) - $(date +%s -r ~/.pomo.txt.tmux)))" -lt 3 ]
then
        echo -n  $(cat ~/Documents/pomodoro_log.txt | tail -n1 | sed 's/.* \(started\|completed\):[ ]\+[0-9]*-[0-9]*-[0-9]*//' | xargs)
        echo " | $(cat ~/.pomo.txt.tmux)"
else
        echo ""
fi
```
