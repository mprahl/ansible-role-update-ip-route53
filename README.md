# update-ip-route53
This is an Ansible role that updates DNS records on Amazon's Route 53 (AWS) with
your public IP address.

Please note that as part of this role, `openssl`, [boto](https://github.com/boto/boto), and
[pyOpenSSL](https://github.com/pyca/pyopenssl) will be installed. If you are using CentOS/Red Hat, the role will
install `pip` (requires [EPEL](https://fedoraproject.org/wiki/EPEL)) and then install `boto` and `pyOpenSSL` in a
Python virtualenv instead because the packaged version of `pyOpenSSL` is not recent enough.

If you are not using this role on Debian/Ubuntu, CentOS/Red Hat,
`openssl`, `boto`, and `pyOpenSSL` must be installed manually before using this role.

## Requirements

Ansible 2.4+ is required for this role. This role also must be run by root or
through sudo/become.

## Role Variables

#### Required Variables
* **update_ip_r53_aws_access_key** - the access key to an AWS user that is allowed to add records to the specified zone.
* **update_ip_r53_aws_secret_key** - the secret key to an AWS user that is allowed to add records to the specified zone.
* **update_ip_r53_records** - the list of dictionaries describing the Route 53 (AWS) domain/zones the public IP address
    should be updated on. All the accepted keys map to the `route53` parameters. The required keys are `zone` and
    `record`. The optional keys are `type` (defaults to `A`) and `wait`.

#### Optional Variables
* **update_ip_r53_virtualenv_dir** - the path to create the Python virtualenv to install the Python dependencies on
    CentOS/Red Hat.

## Example Playbook

```yaml
- name: Update host.example.com and host2.example.com
  hosts: host
  become: yes

  vars:
  - ler53_aws_access_key: SomeAccessKey
  - ler53_aws_secret_key: SomeSecretKey
  - update_ip_r53_records:
    - zone: example.com
      record: host.example.com
    - zone: example.com
      record: host2.example.com

  roles:
  - mprahl.update-ip-route53
```

## License

MIT
