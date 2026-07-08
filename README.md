# molecule_kubevirt

An Ansible role used by Molecule scenarios to create and destroy KubeVirt virtual machines for role testing.

## What this repository contains

- Ansible role logic in `tasks/create.yml` and `tasks/destroy.yml`
- KubeVirt VM template in `templates/kubevirt_vm.yml.j2`
- Molecule scenarios in `molecule/`
- Default VM and connection settings in `defaults/main.yml`

## What the role does

During create:
- Ensures Python `kubernetes` client is installed
- Ensures a target namespace exists (default: `molecule`)
- Validates `molecule_yml.platforms` input
- Creates one KubeVirt VM per platform entry
- Waits for VMs to become Running and receive an IP
- Waits for SSH connectivity
- Writes Molecule instance config

During destroy:
- Removes VMs declared in `molecule_yml.platforms`

## Key variables

Common defaults (see `defaults/main.yml`):
- `molecule_kubevirt_default_kubeconfig_path`
- `molecule_kubevirt_default_namespace`
- `molecule_kubevirt_default_ssh_user`
- `molecule_kubevirt_default_ssh_password`
- `molecule_kubevirt_default_vm_arch`
- `molecule_kubevirt_default_vm_network_attachment`

Each platform entry should define at least:
- `name`
- `image`

Optional per-platform values include `user`, `password`, `port`, `arch`, `image_pull_policy`, and `userdata`.

## Running Molecule

From this repository:

```bash
molecule test -s default
```

Other scenarios are available under `molecule/` (for example `ubuntu_24`, `raspios_bookworm`, and `raspios_trixie`).

## Notes

- The role expects access to a Kubernetes cluster with KubeVirt installed.
- `KUBECONFIG` can be provided via environment variable, otherwise the default kubeconfig path is used.

## License

MIT
