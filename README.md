# OpenSSH client configuration

This repository contains my personal [OpenSSH][] client configuration.

Secret settings, such as those for employers' environments, reside in separate repositories that are kept private and included as [submodules][git submodule].

## host public keys

For some SSH hosts, the public keys are published:

- [GitHub][SSH host public keys github.com]
- [GitLab SaaS][SSH host public keys gitlab.com]

### fingerprints

For some SSH hosts, the public keys are not published, but their fingerprints are:

- GitLab instances: `https://${instance_domain}/help/instance_configuration#ssh-host-keys-fingerprints`
- [SourceForge][SSH host public key fingerprints SourceForge]

To get the public keys of such SSH hosts:

1. Gather their public keys with the command [`ssh-keyscan(1)`][man 1 ssh-keyscan].

   Invoke it as follows:

   ```Shell
   ssh-keyscan -- gitlab.archlinux.org
   ```

   The output looks as follows:

   ```
   # gitlab.archlinux.org:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.6
   gitlab.archlinux.org ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxid4CSjzD5QiM1y12qxNAUdR4kgy+YTym1lY4Arwdf+GC+UGvFP/IzGdlmL681nQeLZN7j2+3Bbm30JZNraA9gesW6BNoOr8QJbuayZJIoQklOUEmvaP7z5PlNChJiwNiXiyXRZzw7BwR4gYGWGSiJtzGYRtIgJDBB+Tc7rVwSy0u16YG2TpFOnxCJ8S25FhRIoyp0A5A+eJgCUe4HDI4Zud+94QdZUVuvpsjzHxXiPr8U8jbsJrG/beWxOnFFx7rhtz/OoQn8sg3anJue+mgtZm/PBs4fccVl30c0Xqfizvdx09sapqyrNf326s9L8NToyi2aHxMEzXfGspOoYtl
   # gitlab.archlinux.org:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.6
   gitlab.archlinux.org ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL+Hs65GpF45799k+r9AW5+xxIRLOdOrOUFsce1BVD8f/tFGBpu6ay06f3tvXXUHVA9iRI6wogDVTpy4x5ch4jY=
   # gitlab.archlinux.org:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.6
   gitlab.archlinux.org ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICjT2SuA0k/xc5Cbyp+eBY5uN3bRL2K7GdpNtltOK6vy
   # gitlab.archlinux.org:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.6
   # gitlab.archlinux.org:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.6
   ```

2. Generate the fingerprint for each key.

   ```Shell
   base64_key='AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL+Hs65GpF45799k+r9AW5+xxIRLOdOrOUFsce1BVD8f/tFGBpu6ay06f3tvXXUHVA9iRI6wogDVTpy4x5ch4jY='
   ```

   ```Shell
   base64 --decode <<< "${base64_key}" | openssl dgst -sha256 -binary | base64
   ```

3. Compare the generated fingerprints with the known fingerprints.

4. If the generated fingerprints match the known fingerprints, trust the public keys gathered with [`ssh-keyscan(1)`][man 1 ssh-keyscan].


[git submodule]: https://www.git-scm.com/docs/gitglossary#def_working_tree
[man 1 ssh-keyscan]: https://man.openbsd.org/ssh-keyscan.1
[OpenSSH]: https://www.openssh.com/
[SSH host public key fingerprints SourceForge]: https://sourceforge.net/p/forge/documentation/SSH%20Key%20Fingerprints/
[SSH host public keys github.com]: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints
[SSH host public keys gitlab.com]: https://docs.gitlab.com/ee/user/gitlab_com/#ssh-known_hosts-entries
