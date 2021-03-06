# File Monitoring

Pode has support for file monitoring which can trigger the server restart, this occurs if Pode detects any file changes within the root directory of your server's script. To enable file monitoring you can either use the `-FileMonitor` switch on your [`server`](../../Functions/Core/Server) script, or enable it through the `pode.json` configuration file as follows:

```powershell
Server {
    # logic
} -FileMonitor
```

or:

```json
{
    "server": {
        "fileMonitor": {
            "enable": true
        }
    }
}
```

Once enabled, Pode will actively monitor all file changes made within the root directory of your script. For example, if your script was at `C:/Apps/Pode/server.ps1`, then Pode will monitor the `C:/Apps/Pode` directory and all sub-directories/files for changes (exclusions can be configured, see below). When a change is detected, Pode will wait 2 seconds before initiating the restart - this is so multiple rapid file changes don't trigger multiple restarts.

The file changes which are being monitored by Pode are:

* Updates
* Creation
* Deletion

!!! important
    If you change the main `server` script itself, the changes will not be picked up. It's best to import/dot-source other modules/scripts into your `server` script, as the internal restart re-invokes this `scriptblock`. If you do make changes to the main server script, you'll need to terminate the server first and then restart it.

## Include/Exclude

You can include/exclude paths/files/extensions from triggering a server restart. To include specific paths/files you can use the `-FileMonitorInclude` parameter on your `server`, and to exclude you can use the `-FileMonitorExclude` parameter. You can also configure them within the `pode.json` configuration file.

Both of the parameters are arrays, and the values should be patterns for paths/files/extensions - for paths, they should always be from the root directory of your server.

For example, to state that all `txt` and `ps1` files should only trigger restarts, you would do:

```powershell
Server {
    # logic
} -FileMonitor -FileMonitorInclude @('*.txt', '*.ps1')
```

or:

```json
{
    "server": {
        "fileMonitor": {
            "enable": true,
            "include": [ "*.txt", "*.ps1" ]
        }
    }
}
```

And to state that changes within the `public` directory should not trigger a restart, you would do:

```powershell
Server {
    # logic
} -FileMonitor -FileMonitorExclude @('public/*')
```

or:

```json
{
    "server": {
        "fileMonitor": {
            "enable": true,
            "exclude": [ "public/*" ]
        }
    }
}
```