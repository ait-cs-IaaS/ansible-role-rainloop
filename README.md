# Ansible Role: Rainloop

This Ansible role installs and configures [Rainloop](https://www.rainloop.net/), a modern webmail client, on a target system.

## Requirements

- Ansible 2.9+
- A Linux-based system (Debian/Ubuntu, RHEL/CentOS)
- Web server (Apache or Nginx)
- PHP (Recommended version: 8.3)
- Database (Optional: for user authentication integration)

## Role Variables

The following variables can be customized in your playbook:

```yaml
rainloop_domain:# The domain where Rainloop will be hosted.
rainloop_url: # The full URL for accessing Rainloop.
rainloop_version: # The version of Rainloop to install.
rainloop_install_dir: # The directory where Rainloop will be installed.
rainloop_download_url: "https://github.com/RainLoop/rainloop-webmail/releases/download/v{{ rainloop_version }}/rainloop-{{ rainloop_version }}.zip"  # The URL to download the Rainloop package.
rainloop_user: "www-data"  # The system user that should own the Rainloop installation.
rainloop_group: "www-data"  # The system group that should own the Rainloop installation.
rainloop_domain_config_path: "/var/www/rainloop/data/_data_/_default_/domains"  # The directory where domain-specific configurations are stored.

rainloop_apache2_vhosts:
  - name: "rainloop"  # The name of the Apache virtual host configuration.
    server_name: "{{ rainloop_url }}"  # The server name for the virtual host.
    vhost_dir: false  # Whether to create a separate directory for the virtual host.
    vhost_template: templates/rainloop-apache-config.j2  # The Jinja2 template for Apache configuration.

php_packages: #php packages 

rainloop_domain_config:
  imap_host: "{{ rainloop_url }}"  # IMAP server hostname for receiving emails.
  imap_port: 143  # IMAP server port.
  imap_secure: "TLS"  # Security protocol for IMAP connection.
  imap_short_login: "On"  # Whether to allow short login format.
  sieve_use: "Off"  # Whether to use the Sieve email filtering protocol.
  sieve_allow_raw: "Off"  # Whether raw Sieve scripts can be used.
  sieve_host: ""  # The host for Sieve server (leave empty if not used).
  sieve_port: 4190  # Port for Sieve server.
  sieve_secure: "None"  # Security setting for Sieve.
  smtp_host: "{{ rainloop_url }}"  # SMTP server hostname for sending emails.
  smtp_port: 587  # SMTP server port.
  smtp_secure: "TLS"  # Security protocol for SMTP connection.
  smtp_short_login: "On"  # Whether to allow short login format.
  smtp_auth: "On"  # Whether SMTP authentication is required.
  smtp_php_mail: "Off"  # Whether to use PHP mail function instead of SMTP.
```

## Tasks Explanation

- **Install PHP and required extensions**:
  Installs necessary PHP packages for Rainloop to function correctly.
  
- **Ensure Rainloop directory exists**:
  Creates the installation directory and sets the appropriate ownership and permissions.
  
- **Extract Rainloop files**:
  Downloads and extracts the Rainloop files into the installation directory.
  
- **Install Apache2**:
  Installs Apache2 and configures virtual hosts as specified in the role variables.
  
- **Flush handlers**:
  Ensures all handlers are executed before proceeding.
  
- **Gather facts explicitly**:
  Ensures Ansible gathers necessary system facts.
  
- **Set a custom fact for Rainloop**:
  Stores the server's IP address as a fact for use in other tasks.
  
- **Trigger Rainloop interface to create _data_ directory**:
  Makes an HTTP request to the Rainloop interface to initialize necessary directories.
  
- **Debug Rainloop HTTP response**:
  Outputs the response from the previous HTTP request for verification.
  
- **Create Rainloop domain configuration**:
  Generates the domain configuration file from a template and ensures proper permissions.

## Dependencies

- Web server role (Apache/Nginx)
- PHP role (if PHP is not installed)

## Usage

1. Include this role in your Ansible playbook.
2. Define necessary variables in your `vars` file.
3. Run the playbook with `ansible-playbook`.

```bash
ansible-playbook -i inventory playbook.yml
```

## License

MIT License

## Contributing

1. Fork the repository
2. Create a new branch (`feature-branch`)
3. Commit your changes
4. Submit a Pull Request

## Author

Maintained by [AIT CS IaaS](https://github.com/ait-cs-IaaS)

