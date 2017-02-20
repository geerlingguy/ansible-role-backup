# Ansible Role: Backup for Simple Servers

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-backup.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-backup)

Back up Linux servers with a simple Rsync-and-Cron-based solution.

## Requirements

Requires the following to be installed:

  - rsync
  - cron

MySQL or a MySQL-compatible database needs to be installed if you'd like to enable MySQL database backups.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    TODO.

## Dependencies

None.

## Example Playbook

    - hosts: servers
    
      vars:
        TODO: TODO.
    
      roles:
        - geerlingguy.backup

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
