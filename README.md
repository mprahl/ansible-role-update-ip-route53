# update-ip-route53
This role updates DNS records on Amazon Route 53 (AWS) with the managed host's
public IP address.

The role supports Debian/Ubuntu and Red Hat family hosts. FreeBSD support has
been removed.

On supported hosts, the role expects `boto3` and `botocore` to already be
installed for the Python interpreter Ansible uses on the managed host.
Unsupported platforms fail fast with a clear error.

## Requirements

Ansible 2.17+ is required for this role. The following collections must also be
available on the control node:

```shell
ansible-galaxy collection install amazon.aws community.general
```

The managed host must already have Python with `boto3` and `botocore`
available to Ansible.

## Role Variables

#### Required Variables
* **update_ip_r53_aws_access_key** - the access key to an AWS user that is allowed to add records to the specified zone.
* **update_ip_r53_aws_secret_key** - the secret key to an AWS user that is allowed to add records to the specified zone.
* **update_ip_r53_records** - the list of dictionaries describing the Route 53 (AWS) domain/zones the public IP address
    should be updated on. All the accepted keys map to the `amazon.aws.route53` parameters. The required keys are `zone` and
    `record`. The optional keys are `type` (defaults to `A`) and `wait`.

## Example Playbook

```yaml
- name: Update host.example.com and host2.example.com
  hosts: host

  vars:
    update_ip_r53_aws_access_key: SomeAccessKey
    update_ip_r53_aws_secret_key: SomeSecretKey
    update_ip_r53_records:
      - zone: example.com
        record: host.example.com
      - zone: example.com
        record: host2.example.com

  roles:
    - mprahl.update-ip-route53
```

## License

MIT
