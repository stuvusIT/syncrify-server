# Syncrify-Server

Ansible role to install and configure a syncrify server in `/opt/Syncrify` using systemd.

## Requirements

An apt-based system (this role has been tested on Debian 9).

## Role Variables

This role only uses the following top-level variables, as most of the configuration is done using the `syncrify_server_options` dict.

| Name                      | Required/Default   | Description                                                                    |
|---------------------------|:------------------:|--------------------------------------------------------------------------------|
| `global_cache_dir`        | :heavy_check_mark: | Cache directory to download files to (on the controller)                       |
| `syncrify_server_user`    | `syncrify`         | User under which to run the syncrify server                                    |
| `syncrify_server_group`   | `syncrify`         | Group under which to run the syncrify server                                   |
| `syncrify_server_options` | `{}`               | Dict containing settings written into the syncrify `AppConfig.xml`, see below. |


### Options

You can define any option in the dict `syncrify_server_options`, data types are detected and formatted correctly for Syncrify.
The following are options we found by configuring Syncrify and looking at the resulting `AppConfig.xml`.
They may not all be needed for your use case, and there are definitively more of them (PRs welcome).

| Name                          | Required/Recommended value                   | Description                                                             |
|-------------------------------|:--------------------------------------------:|-------------------------------------------------------------------------|
| `defaultOperation`            | :heavy_check_mark:                           | Operation mode of the server, e.g. `door`                               |
| `adminEmail`                  | :heavy_check_mark:                           | Email to send important updates and backup reports to                   |
| `errorRecipient`              | :heavy_check_mark:                           | Email recipient to send errors to                                       |
| `preferredHostNameForClients` | :heavy_check_mark:                           | Domain or hostname this server runs on                                  |
| `InitialSetupComplete`        | `true`                                       | Whether the initial configuration is finished                           |
| `smtpSecurity`                |                                              | SMTP Security scheme (`TLS`/`SSL`/`None`)                               |
| `smtpPort`                    |                                              | SMTP port                                                               |
| `smtpServer`                  |                                              | URI of the SMTP server                                                  |
| `smtpUser`                    |                                              | SMTP username                                                           |
| `smtpPasswort`                |                                              | SMTP password                                                           |
| `useWebSmtp`                  | `false`                                      | Use web SMTP (synametrics SMTP relay) if normal SMTP connection fails   |
| `emailServerWebServiceHost`   |                                              | Web SMTP URI                                                            |
| `emailServerWebServicePort`   |                                              | Web SMTP port                                                           |
| `UseRdbmsForDeleteRetension`  | `true`                                       | Whether to enable RDBMS retension                                       |
| `jvmPath`                     | `./jre/bin/java`                             | Path where the java binary is located                                   |
| `httpIP`                      | `127.0.0.1`                                  | IP address to bind to                                                   |
| `httpPort`                    | `0`                                          | HTTP port to listen on (0 disables the port)                            |
| `httpPort2`                   | `5800`                                       | HTTP port to listen on (for some reason there are two)                  |
| `httpPortSSL`                 | `-1`                                         | SSL Port. A value of `-1` disables the port                             |
| `vmParams`                    | `-Xmx512m -DLoggingConfigFile=logconfig.xml` | Java VM parameters                                                      |
| `synametricsUrl`              | `http://synametrics.com/SynametricsWebApp/`  | Synametrics URL                                                         |
| `lastSelectedTab`             |                                              | Which tab in the web interface was last selected (index **as string** ) |
| `imagePath`                   | `images/`                                    | Images path relative to the installation directory                      |
| `blockRDBMSToHandlers`        | `true`                                       | Whether to block rdbms to handlers                                      |
| `RansomwareFileName`          |                                              | Filename for ransomeware check                                          |
| `failureOverHttpPort`         | `54222`                                      | Failover http port                                                      |
| `disableCSRFPrevention`       | `true`                                       | Whether to disable CSRF prevention in the web UI                        |
| `ntServiceCommand`            | `systemctl restart syncrify.service`         | Command to restart the service from the web UI                          |
| `saveClientBackupLog`         | `true`                                       | Whether to save client logs on the server                               |
| `globalSelFilter`             |                                              | RegExp filter specifying which files to consider for backup             |
| `sn`                          |                                              | Serial number (needed to deploy a license)                              |
| `company`                     |                                              | Company to set (needed to deploy a license)                             |

## Example

```yml
syncrify_server_options:
  defaultOperation: door
  InitialSetupComplete: True
  adminEmail: admin@example.com
  company: Example Company
  preferredHostNameForClients: backup.example.com
  errorRecipient: admin@example.com
  smtpServer: smtp.example.com
  smtpPort: 587
  smtpUser: syncrifyServer
  smtpPassword: 123456asdf
  smtpSecurity: STARTTLS
  useWebSmtp: False
  jvmPath: ./jre/bin/java
  httpIP: 127.0.0.1
  httpPort: 0
  httpPort2: 5800
  httpPortSSL: -1
  vmParams: -Xmx512m -DLoggingConfigFile=logconfig.xml
  synametricsUrl: http://synametrics.com/SynametricsWebApp/
  UseRdbmsForDeleteRetension: True
  blockRDBMSToHandlers: True
  failureOverHttpPort: 54222
  ntServiceCommand: systemctl start syncrify.service
  saveClientBackupLog: True

```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

- [Fritz Otlinghaus (Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
- [Michel Weitbrecht (SlothOfAnarchy)](https://github.com/SlothOfAnarchy) _michel.weitbrecht@stuvus.uni-stuttgart.de_
