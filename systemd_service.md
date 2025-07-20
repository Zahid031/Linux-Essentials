# Creating a systemd Service to Run a Script Periodically

This guide provides a roadmap for creating a systemd service that runs a script at a specified interval. We'll create a service that runs a script to append a log message to a file every 10 seconds.

## Roadmap

1.  **Create the Script:** Write the script that the service will execute.
2.  **Create a systemd Service File:** Define the service that will run the script.
3.  **Create a systemd Timer File:** Define a timer to trigger the service at a specific interval.
4.  **Install and Start the Service:** Enable and start the new service and timer.

---

## Step 1: Create the Script

First, let's create a simple shell script that appends the current date and time to a log file.

Create a file named `log_script.sh` in a suitable location, for example, `/usr/local/bin/`.

```bash
#!/bin/bash
echo "Log entry: $(date)" >> /var/log/my_service.log
```

Make the script executable:

```bash
sudo chmod +x /usr/local/bin/log_script.sh
```

---

## Step 2: Create the systemd Service File

Next, create a service file to define the service. Create a file named `my_service.service` in `/etc/systemd/system/`.

```ini
[Unit]
Description=My Custom Logging Service

[Service]
ExecStart=/usr/local/bin/log_script.sh
Type=simple

[Install]
WantedBy=multi-user.target
```

### Comprehensive Explanation of Service File Directives

Here is a more detailed breakdown of the options available in a systemd service file.

---

#### `[Unit]` Section

This section contains generic information about the service that is not dependent on the type of service.

*   **`Description`**: A human-readable title for the service. This is what you'll see in the output of `systemctl status`.
    *   *Example*: `Description=My Awesome App Service`

*   **`After`**: Defines the startup order. The service will start *after* the units listed here are active. This does not imply a dependency.
    *   *Example*: `After=network.target` ensures the network is up before your service starts.

*   **`Wants`**: A weaker version of `Requires`. If the units listed here are started, this service will also be started. If a wanted unit fails to start, it doesn't affect this service.
    *   *Example*: `Wants=network.target`

*   **`Requires`**: Defines a hard dependency. If the units listed here are not active, this service will fail to start. If a required unit is stopped, this service will also be stopped.
    *   *Example*: `Requires=postgresql.service`

---

#### `[Service]` Section

This section contains configuration that is specific to the service itself.

*   **`ExecStart`**: The absolute path to the command or script to be executed when the service is started.
    *   *Example*: `ExecStart=/usr/local/bin/myapp --config /etc/myapp/config.yaml`

*   **`ExecStop`**: An optional command to run when the service is stopped. If not specified, systemd will terminate the process.
    *   *Example*: `ExecStop=/usr/local/bin/myapp-stop.sh`

*   **`ExecReload`**: An optional command to execute to reload the service's configuration without a full restart. You can use the `$MAINPID` variable to signal the main process.
    *   *Example*: `ExecReload=/bin/kill -HUP $MAINPID`

*   **`WorkingDirectory`**: The working directory for the executed process.
    *   *Example*: `WorkingDirectory=/var/lib/myapp`

*   **`User` & `Group`**: For security, it's best practice to run services as a non-privileged user and group.
    *   *Example*: `User=myappuser`, `Group=myappgroup`

*   **`Restart`**: Defines the restart policy.
    *   `no`: The service will not be restarted (default).
    *   `on-success`: Restart only when the service process exits cleanly.
    *   `on-failure`: Restart when the process exits with a non-zero exit code.
    *   `on-abnormal`: Restart if the process is terminated by a signal.
    *   `on-watchdog`: Restart if the watchdog timeout is reached.
    *   `on-abort`: Restart if the process exits due to an uncaught signal not configured as a clean exit status.
    *   `always`: Always restart the service, regardless of the exit code.
    *   *Example*: `Restart=on-failure`

*   **`TimeoutStopSec`**: The time to wait for the service to stop gracefully before systemd force-kills it with `SIGKILL`.
    *   *Example*: `TimeoutStopSec=10`

*   **`Environment`**: Sets environment variables for the service process.
    *   *Example*: `Environment=APP_ENV=production`

*   **`EnvironmentFile`**: Path to a file containing environment variables. A `-` prefix before the path makes it optional (no error if the file doesn't exist).
    *   *Example*: `EnvironmentFile=-/etc/myapp/myapp.env`

*   **`SyslogIdentifier`**: A tag to identify log messages from this service in the system journal.
    *   *Example*: `SyslogIdentifier=myapp`

*   **`KillMode`**: Specifies how to terminate the service's processes.
    *   `control-group` (default): Kills all processes in the service's control group.
    *   `process`: Kills only the main process.
    *   `mixed`: Sends `SIGTERM` to the main process and `SIGKILL` to the rest.
    *   `none`: No processes are killed.
    *   *Example*: `KillMode=process`

*   **Resource Limits**: You can set resource limits for the service.
    *   *Example*: `LimitNOFILE=65535` (max number of open files), `MemoryLimit=512M` (max memory usage).

---

#### `[Install]` Section

This section is used by the `systemctl enable` and `systemctl disable` commands.

*   **`WantedBy`**: Specifies the target that the service should be enabled for. When you run `systemctl enable`, a symbolic link to the service file is created in the `.wants` directory of the specified target.
    *   *Example*: `WantedBy=multi-user.target` means the service will start when the system enters the multi-user runlevel (standard for servers).


---

## Step 3: Create the systemd Timer File

To run the service every 10 seconds, we'll create a corresponding timer file. Create a file named `my_service.timer` in `/etc/systemd/system/`.

```ini
[Unit]
Description=Run my_service every 10 seconds

[Timer]
OnBootSec=10
OnUnitActiveSec=10
Unit=my_service.service

[Install]
WantedBy=timers.target
```

### Explanation of Timer File Directives

*   **`[Unit]` Section:**
    *   `Description`: A brief description of the timer.

*   **`[Timer]` Section:**
    *   `OnBootSec`: The time to wait after boot before the first execution.
    *   `OnUnitActiveSec`: The time to wait between executions after the service has been activated. We set this to `10` to run the service every 10 seconds.
    *   `Unit`: The name of the service file to be activated.

*   **`[Install]` Section:**
    *   `WantedBy`: Specifies the target that the timer should be started with. `timers.target` is the standard target for timers.

---

## Step 4: Install and Start the Service

Now that we have created the script, service, and timer files, we need to enable and start them.

1.  **Reload systemd:**
    ```bash
    sudo systemctl daemon-reload
    ```

2.  **Start the timer:**
    ```bash
    sudo systemctl start my_service.timer
    ```

3.  **Enable the timer to start on boot:**
    ```bash
    sudo systemctl enable my_service.timer
    ```

4.  **Check the status of the timer and service:**
    ```bash
    sudo systemctl status my_service.timer
    sudo systemctl status my_service.service
    ```

5.  **Verify the log file:**
    You can watch the log file to see the output of the script:
    ```bash
    tail -f /var/log/my_service.log
    ```

This will show a new log entry every 10 seconds.
