ruby-builder
============

Ansible Playbook to deploy ruby to Debian or RHEL Based systems via the ruby-build tool.

If you are like me and have to manage multiple linux distributions across Production, Staging, and Development
and find default packages for ruby installs a nightmare to work with then this may be for you.

This approach was spawned out of mostly a hatred for RVM, Rbenv and all the other ruby version managers etc. and the
attempt to simplify the overall server installs in a clean, streamline, and replicable way.

This uses the ruby-build tool (https://github.com/sstephenson/ruby-build) to handle the source intsalls and will be
updated automatically every time this playbook is run to ensure it has the latest ruby definitions.

## Abridged Version of what this does

1. Installs packages required for ruby to be installed
1. Installs the ruby-build tool
1. Installs Ruby From Source to /usr/local/ruby-$VERSION
1. Links the source ruby install to /usr/local/ruby for easy reference
1. Updates the /etc/environments to ensure the ruby path is exported
1. Updates /etc/sudoers to let the ruby binares be accessible w/ sudo

The play is smart enough to see if ruby is already installed it will skip the source installation.

## Role Variables

This play inclues variables for RHEL / Deb required packages as well as two default variables.

All variables are prefixed with the name of the play "ruby_builder"

### Default Variables

    ---
    ruby_builder_ruby_version: 2.1.0
    ruby_builder_environment_path: "/usr/local/ruby/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

ruby_builder_version = The version of ruby to be installed

ruby_builder_environment_path = The path to set in /etc/environment used by templates/environment.j2

### Variables in vars/main.yml

I would advise against changing these as they shouldn't need modified.

I am more fluent with RHEL based systems so its more minimalistic than the Debian based systems.

    ---
    ruby_builder_rhel_ruby_dependencies:
      - git
      - gcc-c++
      - make
      - patch
      - autoconf
      - openssl-devel
      - readline-devel
      - libyaml-devel
      - zlib-devel
    ruby_builder_deb_ruby_dependencies:
      - git
      - libreadline-dev
      - libssl-dev
      - libyaml-dev
      - zlib1g-dev

## Working against the following Distributions

Fedora:
  1. 18 Spherical Cow
  1. 19 Schrodinger's Cat
  1. 20 Heisenbug

Ubuntu:
  1. 12.04 Precise
  1. 13.10 Saucy
