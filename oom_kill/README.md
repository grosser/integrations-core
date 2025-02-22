# Agent Check: OOM Kill

## Overview

This check monitors the kernel OOM (out of memory) kill process through the Datadog Agent and the System Probe.

## Setup

### Installation

The OOM Kill check is included in the [Datadog Agent][1] package. It relies on an eBPF program implemented in the System Probe.

The eBPF program used by the System Probe is compiled at runtime and requires you to have access to the proper kernel headers.

On Debian-like distributions, install the kernel headers like this:
```sh
apt install -y linux-headers-$(uname -r)
```

On RHEL-like distributions, install the kernel headers like this:
```sh
yum install -y kernel-headers-$(uname -r)
yum install -y kernel-devel-$(uname -r)
```

**Note**: Kernel version 4.11 or later is required for the OOM Kill check to work.
In addition, CentOS/RHEL versions earlier than 8 are not supported.

### Configuration

1. In the `system-probe.yaml` file at the root of your Agent's configuration directory, add the following configuration:

    ```yaml
    system_probe_config:
        enable_oom_kill: true
    ```

2. Ensure that the `oom_kill.d/conf.yaml` file is present in the `conf.d/` folder at the root of your Agent's configuration directory to start collecting your OOM Kill metrics. See the [example oom_kill.d/conf.yaml][2] for all available configuration options.

3. [Restart the Agent][3].

### Configuration with Helm

With the [Datadog Helm chart][4], ensure that the `datadog.systemProbe` and `datadog.systemProbe.enableOOMKill` parameters are enabled in the `values.yaml` file.

### Validation

[Run the Agent's status subcommand][5] and look for `oom_kill` under the Checks section.

## Data Collected

### Metrics

See [metadata.csv][6] for a list of metrics provided by this check.

### Service Checks

The OOM Kill check does not include any service checks.

### Events

The OOM Kill check submits an event for each OOM Kill that includes the killed process ID and name, as well as the triggering process ID and name.

## Troubleshooting

Need help? Contact [Datadog support][7].

[1]: https://docs.datadoghq.com/agent/guide/
[2]: https://github.com/DataDog/datadog-agent/blob/master/cmd/agent/dist/conf.d/oom_kill.d/conf.yaml.example
[3]: https://docs.datadoghq.com/agent/guide/agent-commands/#start-stop-and-restart-the-agent
[4]: https://github.com/helm/charts/tree/master/stable/datadog
[5]: https://docs.datadoghq.com/agent/guide/agent-commands/#agent-status-and-information
[6]: https://github.com/DataDog/integrations-core/blob/master/oom_kill/metadata.csv
[7]: https://docs.datadoghq.com/help/
