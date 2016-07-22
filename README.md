# i3 GitHub Notifications

This is a simple utility to provide a visual cue in your i3 status bar that you have unread GitHub notifications. 

# Requirements
1. [`jq`](https://stedolan.github.io/jq) should be installed.

2. You should have the following environment variables set:

#### `GH_NOTIFICATION_TOKEN`
This should be a GitHub access token that has `notifications` permissions

#### `GH_NOTIFICATION_PIDFILE`
This should be the path to an arbitrary file that the script will be able to read/write, e.g.: `/home/you/.ghn`

# Usage
1. Clone this repo somewhere

2. Add the following to your `i3status.conf`:

```
run_watch GitHub {
    pidfile = "/home/you/.ghn"
}
```

`pidfile` should be set to the same value as `GH_NOTIFICATION_PIDFILE`

3. Also add the following line (to `i3status.conf`) where you want the alert to appear:

```
order += "run_watch GitHub"
```

4. Edit [`./systemd/i3-github-notifications.service`](./systemd/i3-github-notifications.service) so that `ExecStart` is the full path to the included [`executable`](./bin/i3-github-notifications) on your system, e.g.:

```
ExecStart=/home/you/i3-github-notifications/bin/i3-github-notifications
```

This is defaulted to /usr/local/bin in this repo

5. Optionally, you can also set the polling frequency in [`./systemd/i3-github-notifications.timer`](./systemd/i3-github-notifications.timer). It defaults to once per minute.

6. Optionally copy the systemd files into `/usr/lib/systemd/user`.

7. Enable and start the timer for your user:

``` sh
systemd --user {enable,start} i3-github-notifications.timer
```

8. Restart i3 and you should see a new section on your i3 bar that shows whether you have any pending GitHub notifications
