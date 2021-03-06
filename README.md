# Certbot DNS

Make certificates with certbot and DNS challenge (DigitalOcean and Google Cloud Platform).
Easy configuration with one dictionary.

View this role on [Ansible Galaxy](https://galaxy.ansible.com/dmitryromanenko/certbot_dns)

## Usage

If you use DigitalOcean DNS service, [create an API token](https://cloud.digitalocean.com/account/api/tokens)  
That's all. Just look at `vars` section in the example playbook.

If you want to use Google Cloud Platform,
[obtain the service account key](https://cloud.google.com/iam/docs/creating-managing-service-account-keys).  
Save it as `files/google/my_key.json` in your ansible project.
Your `access_key` should look like this: `access_key: my_key.json`.

Do not forget to install this role using Ansible Galaxy:

    ansible-galaxy install dmitryromanenko.certbot_dns

## Example Playbook

```yaml
- hosts: localhost
  remote_user: root
  become: yes
  become_method: sudo
  vars:
    letsencrypt:
      email: your.email@example.com    # your email for accepting terms of service
      linux_user: root                 # this user will contain cron jobs for refreshing
      link_to_ssl: yes                 # link certificates to /etc/ssl from /etc/letsencrypt?
      domain_groups:
        example:                       # name of subdirectory with certificate and key
          type: digitalocean           # `digitalocean` or `google`
          access_key: your_token       # token for digitalocean, file name for google
          scheduled: yes               # add entry to cron to refresh the certificates?
          domains:
            - 'example.com'            # add any number of domains to certificate
            - '*.example.com'          # also you can add wildcards
      post_scripts:                    # list of commands to execute after certbot
        - sudo systemctl restart nginx
  roles:
    - dmitryromanenko.certbot_dns
```

## Requirements

Latest Ubuntu.  
Tested with Ansible `2.9`.

Use version `1.1.0` for Ubuntu `bionic` and Debian `stretch`.

## License

Apache-2.0
