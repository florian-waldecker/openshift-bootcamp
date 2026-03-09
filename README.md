# OpenShift Bootcamp

**OpenShift Bootcamp** is a hands-on repository demonstrating infrastructure automation, GitOps, and application delivery for Red Hat OpenShift using:

- **Ansible** for provisioning and configuration management
- **ArgoCD** for GitOps-based continuous delivery

> 🚀 This project serves as a learning lab and a reference implementation for teams adopting OpenShift, ArgoCD, and Ansible.

## Repository Structure

```
openshift-bootcamp/
├── ansible/             # Playbooks, roles, and inventories for deploying OpenShift and ArgoCD
│   ├── inventory
│   ├── main.yaml
│   ├── roles/
│   │   └── install_argocd/...
│   └── vars/
├── argocd/              # GitOps applications and kustomize overlays
│   ├── apps/            # ApplicationSets organized by environment (dev/int/live)
│   └── manifests/        # Shared kustomize bases/overlays for setups like logging
├── tmp/                 # Temporary templates and configs used during development
└── README.md            # This file
```

## Getting Started

1. **Prerequisites**: An OpenShift cluster (4.x), `oc` CLI, Ansible 2.9+, and `kubectl`/`kustomize`.
2. **Provisioning**: Update the variables in `ansible/vars/main.yaml` to suit your environment (cluster details, credentials, etc.), then use the playbook to bootstrap the cluster and install ArgoCD. Sensitive values may be encrypted with Ansible Vault; you can decrypt them at runtime using a vault password file.

   ```sh
   cd ansible
   # edit vars/main.yaml as needed
   # create a vault password file (only readable by you):
   echo "mysecretpassword" > ~/.ansible_vault_pass.txt
   chmod 600 ~/.ansible_vault_pass.txt

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
- **Kustomize Overlays**: The `argocd/manifests` path contains versioned channel configurations and cluster overlays.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with descriptions and testing steps

## License

This project is released under the MIT License. See `LICENSE` for details.

---

*Last updated: March 2026*
