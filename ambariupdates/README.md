Ambari Updates Role
==================

An Ansible role for managing Ambari service updates and operations.

Requirements
------------

- Ansible 2.9 or higher
- Access to Ambari server
- Appropriate credentials for Ambari authentication

Role Variables
--------------

### Default Variables

```yaml
ambari_port: 8443
cluster_name: "devnifi"
```

### Credential Management

This role requires Ambari admin credentials for authentication. To maintain security and portability, credentials should NOT be stored directly in the role. Instead, you have several options:

1. **Environment Variables (Recommended)**
   ```bash
   export AMBARI_ADMIN_USERNAME="your_username"
   export AMBARI_ADMIN_PASSWORD="your_password"
   ```

2. **Host/Group Variables with Ansible Vault**
   ```bash
   # Create encrypted vars file
   ansible-vault create group_vars/ambari_vault.yml
   
   # Add credentials
   ambari_admin_username: "your_username"
   ambari_admin_password: "your_password"
   ```

3. **External Secrets Management**
   ```yaml
   # Example with HashiCorp Vault
   ambari_admin_username: "{{ lookup('hashi_vault', 'secret=secret/ambari/credentials:username') }}"
   ambari_admin_password: "{{ lookup('hashi_vault', 'secret=secret/ambari/credentials:password') }}"
   ```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

```yaml
- hosts: ambari_servers
  vars_files:
    - group_vars/ambari_vault.yml  # Optional: if using vault for credentials
  roles:
    - role: ambariupdates
      vars:
        cluster_name: "my_cluster"  # Override default cluster name if needed

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
