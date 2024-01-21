# Swarmie

This is a proof of concept tool to use iroh global content discovery using the
bittorrent mainline DHT.

It is basically [sendme](https://github.com/n0-computer/sendme) but with an
additional step of publishing to a tracker.

**WARNING**

While dumbpipe and sendme are reasonably well tested and can be used in production,
this one is currently _very_ experimental...

# Installation

```
cargo install swarmie
```

# Usage

## Send side

```
swarmie publish <file or directory>
```

This will create a temporary iroh node that serves the content in the given file or directory.
It will publish information about the data to one or more trackers.

The provider will run until it is terminated using Control-C. On termination it will delete
the temporary directory.

This currently will create a temporary directory in the current directory. In the future this
won't be needed anymore.

### Receive side

```
swarmie subscribe <hash>
```

When given trackers as arguments, this will connect to all trackers and ask them
if they know who has the content. If no tracker arguments are provided, it will
query the bittorrent mainline DHT for trackers.

This will download the data and create a file or directory named like the source
in the **current directory**.

It will create a temporary directory in the current directory, download the data (single
file or directory), and only then move these files to the target directory.

On completion it will delete the temp directory.

All temp directories start with `.swarmie-`.