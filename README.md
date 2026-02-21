[![Actions Status - Master](https://github.com/juju4/ansible-ollama/workflows/AnsibleCI/badge.svg)](https://github.com/juju4/ansible-ollama/actions?query=branch%3Amain)
[![Actions Status - Devel](https://github.com/juju4/ansible-ollama/workflows/AnsibleCI/badge.svg?branch=devel)](https://github.com/juju4/ansible-ollama/actions?query=branch%3Adevel)

# Ollama ansible role

Ansible role to setup [Ollama](https://github.com/ollama/ollama/).

See also
* https://ollama.com/

## Requirements & Dependencies

### Ansible
It was tested on the following versions:
 * 2.10-17

### Operating systems

Tested on Ubuntu 24.04, 22.04, 20.04, Centos/Rockylinux 9.

macOS (Darwin) is experimentally supported via Homebrew. No CI coverage; manually tested.

## macOS Notes

macOS support requires:
- Homebrew installed (automatically detected at `/opt/homebrew` or `/usr/local`)
- Uses `brew services` for process management (no launchd plist customization)
- Same `ollama_models_downloads` variable for model pre-pulling

This path is intended for local development/workstation use.

## Example Playbook

Just include this role in your list.
For example

```
- host: myhost
  roles:
    - juju4.ollama
```

you probably want to review variables

## Variables

TBD


## Continuous integration

```
$ pip install molecule docker
$ molecule test
$ MOLECULE_DISTRO=ubuntu:24.04 molecule test --destroy=never
```


## Troubleshooting & Known issues

* If using GPU and systemd hardening, you will require custom options for access (see defaults/main.yml for amdgpu example).

* When using reverse-proxy with https and custom CA, you need to bundle common CA certificates and custom one if need to use with specific libraries. For example, crewai use httpx which will require environment variable SSL_CERT_FILE=/path/to/ca-bundle-custom.pem. If you use only your custom CA, other tools will stop to work (search...).
  * On RedHat/Fedora, `cat /etc/pki/tls/certs/ca-bundle.crt ca-custom.crt > ca-bundle-custom.pem`
  * On Debian/Ubuntu, `cat /usr/share/ca-certificates/mozilla/*.crt ca-custom.crt > ca-bundle-custom.pem`

* `litellm.APIConnectionError: OllamaException - {"error":"POST predict: Post \"http://127.0.0.1:33887/completion\": EOF"}`
  * https://github.com/ollama/ollama/issues/7640
  * Check ollama logs to find underlying issue
  * happened with or without systemd hardening
  * possibly just not enough memory or "GPU hang"

* Typical issues in logs
```
journalctl -u ollama -g "level=ERROR" --since today -l --no-pager
journalctl -u ollama -g "out of memory" --since today -l --no-pager
journalctl -u ollama -g "GPU Hang" --since today -l --no-pager
journalctl -u ollama -g "level=WARN" --since today -l --no-pager
journalctl -u ollama -g "Server listening on " --since today -l --no-pager
journalctl -u ollama -g "invalid option provided" --since today -l --no-pager
journalctl -u ollama -g "model requires more system memory" --since today -l --no-pager
```

## License

BSD 2-clause
