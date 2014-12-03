# pushme

*pushme* is a simple bash script to push an update to a phone via a service. It's primary use case is to notify of the completion of long running processes rather then having to keep checking on them. Other use cases can likely also be accommodated.

# Requirements

The only requirement is that *bash* is installed and either *wget* or *curl* are available. The script will pick the available one.

Other than that you will need an account on either [pushbullet](https://www.pushbullet.com/) or [notifymyandroid](http://www.notifymyandroid.com/).

# Installation

Since you need *wget* or *curl* available you can use them to install.

    wget -O pushme https://raw.githubusercontent.com/opennomad/pushme/master/pushme

or

    curl -o pushme https://raw.githubusercontent.com/opennomad/pushme/master/pushme

Then make the file executable

    chmoe 755 pushme

and copy the file to somewhere in your `PATH`.

# Configuration

*pushme* will need the access token from the service being used as well as the service name. This can be provided via the `PUSHME` environment like this:

    PUSHME=notifymyandroid:<your access token here>

or

    pushme=pushbullet:<your access token here>

Alternatively, this can be placed in a config file in `$HOME/.pushme`, which should contain one line such as

    pushbullet:<your access token here>

Just replace `<your access token here>` with your actual access token.

# Usage

The primary motivation for this script is to notify when a long running process finally finishes. The easiest way to call it is just

    pushme

That will produce a message to the phone stating `command on <hostname> completed`.

If the first parameter is a number it will be used to indicate that the command either *succeeded* or *failed*. For example

    pushme $?

It's also possible to pass a second parameter, which will be used to replace the word 'command'

    pushme $? rsync

will set the title of the message pushed to `rsync on <hostname> succeeded` if `$?` is 0, otherwise it will state that the command failed.

If the second parameter is anything other than a single word, it is used to set the title of the message.

The content of the pushed message will always begin with `command completed at <timestamp>`. Additional text can be piped into the script:

    rsync -rvz /opt server:/opt &> /tmp/output; tail /tmp/output | pushme $? "rsync"

This will append the tail of the file into the pushed message.

**Note:**  `$?` will reflect that output of the `rsync` command and not the `tail`. The status of the tail command can be found in `PIPESTATUS` not `$?`.
