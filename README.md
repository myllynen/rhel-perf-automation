# RHEL Performance Automation

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Ansible playbooks for RHEL performance monitoring automation.

## Contents

* [inventory](inventory)
  * Example inventory
* [perf_monitor_install_minimal.yml](perf_monitor_install_minimal.yml)
  * Playbook to install minimal setup without central monitoring
 [perf_monitor_install_monitor.yml](perf_monitor_install_monitor.yml)
  * Playbook to install setup with central monitoring configured

Depending on the environment and requirements separate playbooks and/or
vars files, group vars, variables defined in an inventory, or some other
approach might be appropriate for providing monitoring configuration.
These examples aim to provide a known-good starting point for typical
installations.

## Quick Usage Example

These playbooks use the `metrics` Ansible role which is included in the
the Red Hat supported _RHEL system roles_ Ansible collection. It is
available as RPM or from Red Hat Automation Hub for AAP, please see the
links below. In essence, the playbooks configure
[Performance Co-Pilot (PCP)](https://pcp.io/) with given features.

For a complete RHEL performance guide and PCP usage examples, see
[this page](https://myllynen.github.io/rhel-performance-guide/).

To use these playbooks on Fedora, change the included role names from
`redhat.rhel_system_roles.metrics` to `linux-system-roles.metrics`.

In an environment without Ansible Automation Platform (AAP) install the
needed RPM packages from the standard repositories on a "control node".
It must be possible to connect from the control node to all the hosts
and become as the superuser there:

```
# Install Ansible and the role on control node,
# this needed only where the playbook will be run
sudo dnf install ansible-core rhel-system-roles
```

To install the minimal setup on any number of hosts without a central
monitoring host for the environment, use the provided minimal setup
playbook as follows:

```
# Edit inventory to match the local environment
# Add hosts where to do basic PCP setup in the monitor group
vi inventory
# Review the playbook before using it
less perf_monitor_install_minimal.yml
# Install minimal RHEL performance monitoring setup
ansible-playbook -i inventory perf_monitor_install_minimal.yml
```

To install a setup with one host providing a central Grafana Web UI and
visualizing metrics from all hosts in the environment, use monitoring
setup playbook as follows:

```
# Edit inventory to match the local environment
# Add the host where to setup Grafana in the grafana group
# Add hosts where to do basic PCP setup in the monitor group
vi inventory
# Review the playbook before using it
less perf_monitor_install_monitor.yml
# Install RHEL performance monitoring setup with Web UI
ansible-playbook -i inventory perf_monitor_install_monitor.yml
```

The Grafana Web UI will be available on the configured host at
http://localhost:3000. If needed, use SSH tunneling to allow access to
it without additional firewall configuration, the target here is the
host in the Ansible grafana group:

```
ssh -L 127.0.0.1:3000:localhost:3000 rhel1.example.com
```

## See Also

See also
[https://github.com/linux-system-roles/metrics](https://github.com/linux-system-roles/metrics).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles).

See also
[https://github.com/myllynen/rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles).

See also
[https://myllynen.github.io/rhel-performance-guide/](https://myllynen.github.io/rhel-performance-guide/).

## License

GPLv3+
