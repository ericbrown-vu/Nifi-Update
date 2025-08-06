# NiFi Update Script

## Credential Management

This role requires NiFi API credentials for authentication. To maintain security and portability, credentials should NOT be stored directly in the role. Instead, you have several options for providing credentials:

### 1. Environment Variables (Recommended)

Set the following environment variables:
```bash
export NIFI_ADMIN_USERNAME="your_username"
export NIFI_ADMIN_PASSWORD="your_password"
```

### 2. Host/Group Variables with Ansible Vault

Create an encrypted vault file in your project:
```bash
# Create encrypted vars file
ansible-vault create group_vars/nifi_vault.yml

# Add credentials
nifi_admin_username: "your_username"
nifi_admin_password: "your_password"

# Reference in your playbook
vars_files:
  - group_vars/nifi_vault.yml
```

### 3. External Secrets Management

For production environments, consider using external secrets management systems:
- HashiCorp Vault
- AWS Secrets Manager
- Azure Key Vault

Example with HashiCorp Vault:
```yaml
nifi_admin_username: "{{ lookup('hashi_vault', 'secret=secret/nifi/credentials:username') }}"
nifi_admin_password: "{{ lookup('hashi_vault', 'secret=secret/nifi/credentials:password') }}"
```

## Security Notes
- Never commit credentials to version control
- Use different credentials for each environment
- Regularly rotate credentials
- Consider using service accounts where appropriate
