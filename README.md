# renovate-16197-orphaned-processes

Edit the `TOKEN` and `USER` to match your github user details
```
module.exports = {
    token: '{TOKEN}',
    gitAuthor: "Renovate Bot <{USER}@github.com>",
    repositories: ['{USER}/renovate-16197-orphaned-processes'],
    platform: 'github',
    logContext: "json",
    logFileLevel: "debug",
    logFile: "/tmp/log.json",
    logLevel: "debug",
    dependencyDashboard: true,
    includeForks: true
};
```

Run the interactive docker command:
```
docker run --rm -it -v config.js:/usr/src/app/config.js renovate/renovate:32.94-slim bash
```

<br>

Due to the `"prepare": "echo 'long prepare for 900 sec' && sleep 900"` in the `package.json` file and the cli arguments passed to renovate below, the npm command will timeout. then the `ps` command will show an active npm process although renovate run finished.

<br><br>
Inside the container run:

```
renovate --binary-source=install --execution-timeout=1 --allow-scripts=true
```

```
ps -fu ubuntu
```
