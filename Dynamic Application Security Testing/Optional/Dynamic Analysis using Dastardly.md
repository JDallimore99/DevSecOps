# Dynamic Analysis using Dastardly
In this scenario, you will learn how to use Dastardly from Burp Suite to scan a website for security issues.
## Dastardly 
Dastardly is an open-source lightweight web application security scanner that can be used as a Dynamic Application Security Testing tool. It can be easily integrated into your CI/CD system to find vulnerabilities in your software applications.
In the previous exercise, we learned how to use ZAP using a Baseline scan to find security issues. In this exercise, we will use Dastardly to perform Dynamic Application Security Testing (DAST) in our target machine (prod). Once you get the results from Dastardly you can compare the results with the ZAP results and make a decision about which tools better fit your requirements.
First, we need to pull Dastardly’s docker image.
```sh
docker pull public.ecr.aws/portswigger/dastardly
```
Then, let’s run Dastardly scan with the following command.
```sh
docker run --user $(id -u) --rm -v $(pwd):/dastardly -e DASTARDLY_TARGET_URL=https://prod-acsrq8h9.lab.practical-devsecops.training -e DASTARDLY_OUTPUT_FILE=/dastardly/dastardly-report.xml public.ecr.aws/portswigger/dastardly
```
**Output**
```
 §§§§§§§§§§§§§§§§§§§§§§§§\
    §                      § |
    §          ||          § |
    §         //           § |    §§§§§§§\                        §§\                               §§\ §§\
    §        //            § |    §§  __§§\                       §§ |                              §§ |§§ |
    §      /////|          § |    §§ |  §§ | §§§§§§\   §§§§§§§\ §§§§§§\    §§§§§§\   §§§§§§\   §§§§§§§ |§§ |§§\   §§\
    §          ||          § |    §§ |  §§ | \____§§\ §§  _____|\_§§  _|   \____§§\ §§  __§§\ §§  __§§ |§§ |§§ |  §§ |
    §          |/////      § |    §§ |  §§ | §§§§§§§ |\§§§§§§\    §§ |     §§§§§§§ |§§ |  \__|§§ /  §§ |§§ |§§ |  §§ |
    §            //        § |    §§ |  §§ |§§  __§§ | \____§§\   §§ |§§\ §§  __§§ |§§ |      §§ |  §§ |§§ |§§ |  §§ |
    §           //         § |    §§§§§§§  |\§§§§§§§ |§§§§§§§  |  \§§§§  |\§§§§§§§ |§§ |      \§§§§§§§ |§§ |\§§§§§§§ |
    §          ||          § |    \_______/  \_______|\_______/    \____/  \_______|\__|       \_______|\__| \____§§ |
    §                      § |                                                                              §§\   §§ |
    §§§§§§§§§§§§§§§§§§§§§§§§ |                                           from Burp Suite      v2022.11.1    \§§§§§§  |
    \________________________|                                                                               \______/

Installing or running Dastardly affirms your agreement to the Terms of Service https://portswigger.net/burp/dastardly/eula

[0.001s][warning][os,container] Duplicate cpuset controllers detected. Picking /sys/fs/cgroup/cpuset, skipping /dastardly/sys/fs/cgroup/cpuset.
[0.001s][warning][os,container] Duplicate cpuset controllers detected. Picking /sys/fs/cgroup/cpuset, skipping /dastardly/var/lib/docker/overlay2/bd93935810572edc1bb3a9d4b9911779f7259d301314f980a57e484e47b6c016/merged/sys/fs/cgroup/cpuset.
2022-11-21 05:18:27 INFO  dastardly.StartDastardly - Using Java version 17.0.4
2022-11-21 05:18:28 INFO  bsee.BurpProcess.scan.scan-1 - [0.001s][warning][os,container] Duplicate cpuset controllers detected. Picking /sys/fs/cgroup/cpuset, skipping /dastardly/sys/fs/cgroup/cpuset.
2022-11-21 05:18:28 INFO  bsee.BurpProcess.scan.scan-1 - [0.001s][warning][os,container] Duplicate cpuset controllers detected. Picking /sys/fs/cgroup/cpuset, skipping /dastardly/var/lib/docker/overlay2/bd93935810572edc1bb3a9d4b9911779f7259d301314f980a57e484e47b6c016/merged/sys/fs/cgroup/cpuset.
2022-11-21 05:18:28 INFO  bsee.BurpProcess.scan.scan-1 - Nov 21, 2022 5:18:28 AM java.util.prefs.FileSystemPreferences$1 run
2022-11-21 05:18:28 INFO  bsee.BurpProcess.scan.scan-1 - INFO: Created user preferences directory.
2022-11-21 05:18:35 INFO  bsee.BurpProcess.scan.scan-1 - 2022-11-21 05:18:35: REST API running on http://localhost:39221/
2022-11-21 05:18:35 INFO  bsee.BurpProcess.scan.scan-1 - [Thread: 42] 2022-11-21 05:18:35.025 236115184365734, net.portswigger.om INFO - connectedSocket, opened new socket: 1801766359
2022-11-21 05:18:36 INFO  bsee.BurpProcess.scan.scan-1 - Debug ID: 87w091rcf8nqy8wv5arb:l0u8
2022-11-21 05:18:36 INFO  bsee.BurpProcess.scan.scan-1 - Burp Version: 2022.11.1-17268
2022-11-21 05:18:40 INFO  bsee.BurpProcess.scan.scan-1 - [Thread: 133] 2022-11-21 05:18:40.004 236120163272530, net.portswigger.om INFO - connectedSocket, opened new socket: 935839032
2022-11-21 05:18:40 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 3
2022-11-21 05:18:45 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 4
2022-11-21 05:18:50 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 12
2022-11-21 05:18:55 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 15
2022-11-21 05:19:00 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:05 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:10 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:15 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:20 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:25 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:30 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:35 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:40 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:45 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:50 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 16
2022-11-21 05:19:55 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 17
2022-11-21 05:20:00 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 25
2022-11-21 05:20:05 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 29
2022-11-21 05:20:10 INFO  bsee.BurpProcess.scan.scan-1 - Received metric CRAWLING 0 35
2022-11-21 05:20:11 INFO  bsee.BurpProcess.scan.scan-1 - 2022-11-21 05:20:11: Crawl finished
2022-11-21 05:20:11 INFO  bsee.BurpProcess.scan.scan-1 - 2022-11-21 05:20:11: Audit started
2022-11-21 05:20:15 INFO  bsee.BurpProcess.scan.scan-1 - Received metric AUDITING 369 37
2022-11-21 05:20:20 INFO  bsee.BurpProcess.scan.scan-1 - Received metric AUDITING 975 37
2022-11-21 05:20:25 INFO  bsee.BurpProcess.scan.scan-1 - Received metric AUDITING 1397 37
2022-11-21 05:20:30 INFO  bsee.BurpProcess.scan.scan-1 - Received metric AUDITING 1730 37
2022-11-21 05:20:31 INFO  bsee.BurpProcess.scan.scan-1 - 2022-11-21 05:20:31: Audit finished
2022-11-21 05:20:35 INFO  bsee.BurpProcess.scan.scan-1 - Received metric SUCCEEDED 1765 37
2022-11-21 05:20:35 INFO  dastardly.ScanProgress - Scan has completed successfully
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:18:35 INFORMATION Running as super-user, browser sandbox is not supported
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:18:36 INFORMATION Crawl started.
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:20:11 INFORMATION Crawl finished.
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:20:11 INFORMATION Identifying items to audit.
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:20:11 INFORMATION Audit started.
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:20:30 ERROR Could not start Burp's browser sandbox because you are running as root. Either switch to running as an unprivileged user or allow running without sandbox.
2022-11-21 05:20:35 INFO  dastardly.EventLogPrinter - Nov 21 2022 05:20:31 INFORMATION Audit finished.
2022-11-21 05:20:35 ERROR dastardly.ScanFinishedHandler - Failing build as scanner identified issue(s) with severity higher than "INFO":
2022-11-21 05:20:35 ERROR dastardly.ScanFinishedHandler - Path: / Issue Type: Vulnerable JavaScript dependency Severity: LOW
2022-11-21 05:20:35 ERROR dastardly.ScanFinishedHandler - Path: /static/taskManager/js/bootstrap.js Issue Type: Vulnerable JavaScript dependency Severity: LOW
2022-11-21 05:20:36 INFO  bsee.BurpProcess.scan.scan-1 - Deleting temporary files - please wait ... done.
```
The output is present in XML format and the file is in the current directory. You can verify the existence of the output file with ls -l command.
To read the output easily, we suggest you to use this website and copy paste the output into it: https://codebeautify.org/xmlviewer
