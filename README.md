# OpenShift Bootcamp

**OpenShift Bootcamp** is a hands-on repository demonstrating infrastructure automation, GitOps, and application delivery for Red Hat OpenShift using:

- **Ansible** for provisioning and configuration management
- **ArgoCD** for GitOps-based continuous delivery

> 🚀 This project serves as a learning lab and a reference implementation for teams adopting OpenShift, ArgoCD, and Ansible.

## Repository Structure

This project is organized around two main concerns: provisioning via
Ansible and GitOps automation with ArgoCD.

```
openshift-bootcamp/
├── ansible/                 # Playbooks, roles, and inventories for deploying ArgoCD
│   ├── inventory            # Ansible inventory file listing target hosts/clusters
│   ├── main.yaml            # Entry-point playbook that orchestrates setup
│   ├── roles/               # Reusable roles; e.g. install_argocd contains templates
│   │   └── install_argocd/
│   │       ├── tasks/       # Tasks executed by the role
│   │       ├── templates/   # Jinja2 manifests used by the role
│   │       └── vars/        # Variables used during role execution
│   └── vars/                # Global variable definitions (edit before running)
├── argocd/                  # GitOps applications and kustomize overlays
│   ├── apps/                # ApplicationSet YAMLs grouped by environment (dev, int, live)
│   └── manifests/           # Kustomize bases and overlays for shared resources
│       ├── logging/         # Example of logging stack configuration
│       │   ├── do380/       # Working configuration of logging operator in do380 environments
│       │   ├── local/       # Working configuration of logging operator in local crc environments
│       │   └── overlays/    # Cluster-specific customizations
└── README.md                # This file
```
> ⚠️ **Note:** the `argocd/manifests/logging/*` directories contain
> example configurations that may be outdated; treat them as
> historical references rather than active code. In a real deployment you
> would create/maintain your own kustomize bases or pull from an upstream
> repository.
Each section is intended to be self‑contained: modify `ansible/vars`
for provisioning parameters, drop new ApplicationSets under `argocd/apps`
for deployment, and use `manifests` subdirectories to share or override
base resources across versions and clusters.

## Getting Started

1. **Prerequisites**: An OpenShift cluster (4.x), `oc` CLI tool, Ansible 2.9+.
2. **Provisioning**: Update the variables in `ansible/vars/main.yaml` to suit your environment (cluster details, credentials, etc.), then use the playbook to bootstrap the cluster and install ArgoCD. Sensitive values may be encrypted with Ansible Vault; you can decrypt them at runtime using a vault password file.

   ```sh
   cd ansible
   # edit vars/main.yaml as needed
   # create a vault password file (only readable by you):
   echo "mysecretpassword" > ~/.ansible_vault_pass.txt
   or use
   vi ~/.ansible_vault_pass.txt and paste the password in the editor

   ansible-playbook -i inventory main.yaml \
     --vault-password-file ~/.ansible_vault_pass.txt \
     --extra-vars "some_var=value another_var=value2"
   ```

   *Tip: keep the vault file outside of version control; add it to `.gitignore`.*

   > **Note:** During the OpenShift Bootcamp session, the vault password
   > will be provided to participants so they can decrypt the example vault
   > variables.

3. **Deploy Applications**: Commit or apply manifests under `argocd/` to your git repo; ArgoCD will manage the sync.

## Key Components

- **Ansible Roles**: `install_argocd` role shows how to apply custom Jinja2 templates and handle role variables.
- **ArgoCD Apps**: `apps/dev/logging-appset.yaml` is an example of an ApplicationSet for environment-specific deployments.
- **Kustomize Overlays**: The `argocd/manifests` path contains logging operator configurations and cluster overlays.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with descriptions and testing steps

## License

This project is released under the MIT License. See `LICENSE` for details.

---

*Last updated: March 2026*
