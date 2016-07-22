# i3 GitHub Notifications

This is a simple utility to provide a visual cue in your i3 status bar that you have unread GitHub notifications. 

# Requirements
[`jq`](https://stedolan.github.io/jq) should be installed.

You should have the following environment variables set:

#### `GH_NOTIFICATION_TOKEN`
This should be a GitHub access token that has `notifications` permissions

#### `GH_NOTIFICATION_PIDFILE`
This should be the path to an arbitrary file that the script will be able to read/write, e.g.: `/home/you/.ghn`

If you are using this via `systemd`, you'll need to supply the env variables to the units. To do this, assuming you copy the unit files to `/usr/lib/systemd/user`, you can create a directory at `/usr/lib/systemd/user/i3-github-notifications.service.d/` and add a file called `env.conf` with the following:

```
[Service]
Environment="GH_NOTIFICATION_TOKEN=xxx"
Environment="GH_NOTIFICATION_PIDFILE=xxx"
```

# Usage

#### `i3status.conf`

Add the following to your `i3status.conf`:

```
run_watch GitHub {
    pidfile = "/home/you/.ghn"
}
```

`pidfile` should be set to the same value as `GH_NOTIFICATION_PIDFILE`

Also add the following line where you want the alert to appear:

```
order += "run_watch GitHub"
```

#### `systemd`

Edit [`./systemd/i3-github-notifications.service`](./systemd/i3-github-notifications.service) so that `ExecStart` is the full path to the included [`executable`](./bin/i3-github-notifications) on your system, e.g.:

```
ExecStart=/home/you/i3-github-notifications/bin/i3-github-notifications
```

This is defaulted to /usr/local/bin in this repo

Enable and start the timer for your user:

``` sh
systemd --user {enable,start} i3-github-notifications.timer
```

##### Optional stuff

Optionally, you can also set the polling frequency in [`./systemd/i3-github-notifications.timer`](./systemd/i3-github-notifications.timer). It defaults to once per minute.

Optionally copy the `systemd` files into `/usr/lib/systemd/user`.


#### Finally

Restart i3 and you should see a new section on your i3 bar that shows whether you have any pending GitHub notifications

