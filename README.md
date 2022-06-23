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

<br><br>
Result:
```
/renovate-16197-orphaned-processes$ docker run --rm -it -v /home/nabeels/workspace/shared/nabeelys.js:/usr/src/app/config.js renovate/renovate:32.94-slim bash
ubuntu@d3f7fd88ad3b:/usr/src/app$
ubuntu@d3f7fd88ad3b:/usr/src/app$ time renovate --binary-source=install --execution-timeout=1 --allow-scripts=true
 INFO: Repository started (repository=nabeelys/renovate-16197-orphaned-processes)
       "renovateVersion": "32.94.0"
 INFO: Dependency extraction complete (repository=nabeelys/renovate-16197-orphaned-processes)
       "baseBranch": "main",
       "stats": {
         "managers": {"npm": {"fileCount": 1, "depCount": 1}},
         "total": {"fileCount": 1, "depCount": 1}
       }
 INFO: Temporary error - aborting (repository=nabeelys/renovate-16197-orphaned-processes)
 INFO: Repository finished (repository=nabeelys/renovate-16197-orphaned-processes)
       "durationMs": 72693

real    1m14.843s
user    0m4.928s
sys     0m2.179s

```

```
ubuntu@d3f7fd88ad3b:/usr/src/app$ history
    1  time renovate --binary-source=install --execution-timeout=1 --allow-scripts=true
    2  ps -fu ubuntu
    3  time renovate --binary-source=install --execution-timeout=1 --allow-scripts=true
    4  ps -fu ubuntu
    5  history
ubuntu@d3f7fd88ad3b:/usr/src/app$
ubuntu@d3f7fd88ad3b:/usr/src/app$ ps -fu ubuntu
UID          PID    PPID  C STIME TTY          TIME CMD
ubuntu         1       0  0 11:03 ?        00:00:00 dumb-init -- bash
ubuntu         9       1  0 11:03 pts/0    00:00:00 bash
ubuntu       133       1  0 11:04 pts/0    00:00:00 npm install
ubuntu       144     133  0 11:04 pts/0    00:00:00 sh -c /tmp/prepare-1655982247632.sh
ubuntu       145     144  0 11:04 pts/0    00:00:00 /usr/bin/sh /tmp/prepare-1655982247632.sh
ubuntu       146     145  0 11:04 pts/0    00:00:00 sleep 900
ubuntu       217       1  0 11:06 pts/0    00:00:00 npm install
ubuntu       228     217  0 11:07 pts/0    00:00:00 sh -c /tmp/prepare-1655982420855.sh
ubuntu       229     228  0 11:07 pts/0    00:00:00 /usr/bin/sh /tmp/prepare-1655982420855.sh
ubuntu       230     229  0 11:07 pts/0    00:00:00 sleep 900
ubuntu       232       9  0 11:08 pts/0    00:00:00 ps -fu ubuntu
ubuntu@d3f7fd88ad3b:/usr/src/app$
```
