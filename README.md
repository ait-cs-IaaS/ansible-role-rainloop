# Rainloop Ansible Playbook

This Ansible playbook automates the deployment and configuration of [Rainloop Webmail](https://www.rainloop.net) on a server using Apache. It sets up the required directory structure, deploys Rainloop, and configures an Apache virtual host.

## Requirements

- **Ansible Version**: 2.9 or later
- **Server Requirements**:
  - Linux server with `sudo` privileges
  - Internet access for downloading Rainloop files
- **Roles**:
  - `apache2` role for Apache installation and configuration

## Variables

### Required Variables

| Variable             | Description                                    | Example                     |
|----------------------|------------------------------------------------|-----------------------------|
| `rainloop_install_dir` | Path where Rainloop will be installed          | `/var/www/rainloop`         |
| `rainloop_url`       | URL to the Rainloop archive file               | `https://example.com/rainloop-latest.zip` |
| `rainloop_user`      | User who will own the Rainloop files           | `www-data`                  |
| `rainloop_group`     | Group for the Rainloop files                   | `www-data`                  |
| `rainloop_domain`    | Domain configuration for Rainloop              | `example.com`               |

### Optional Variables

| Variable             | Description                                    | Default                     |
|----------------------|------------------------------------------------|-----------------------------|
| `apache2_vhosts`     | Apache virtual host configuration              | Set dynamically in playbook |

## Tasks Overview

1. **Ensure Rainloop Directory Exists**:
   - Creates the target directory for Rainloop installation.
   - Sets ownership and permissions.

2. **Extract Rainloop Files**:
   - Downloads and extracts Rainloop from the specified URL.

3. **Create Rainloop Domain Configuration**:
   - Generates a domain configuration file for Rainloop using a Jinja2 template.
   - Configuration is placed in `/var/www/rainloop/data/_data_/_default_/domains/`.

4. **Install Apache2**:
   - Uses the `apache2` role to install and configure Apache.

5. **Set Apache Virtual Hosts Fact**:
   - Configures Rainloop's Apache virtual host.

6. **Restart Apache**:
   - Automatically restarts Apache if the configuration changes.

## Usage

### Step 1: Define Variables
Create a `vars.yml` file with the necessary variable values:

```yaml
rainloop_install_dir: "/var/www/rainloop"
rainloop_url: "https://example.com/rainloop-latest.zip"
rainloop_user: "www-data"
rainloop_group: "www-data"
rainloop_domain: "example.com"
