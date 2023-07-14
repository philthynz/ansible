## Overview
This role installs the AWX Operator and optionally AWX onto k8s. The big differenec with this role is that the image versions can all be specified, becuase it builds the Helm chart. Which makes AWX a lot easier when it comes to multi architectures. This has been tested on docker-desktop AMD64 and k3's ARM64.

It's important to use the same operator versions that match the AWX release. You will find mathcing version on the [awx-operator release page](https://github.com/ansible/awx-operator/releases)

## Requirements
Deploy machine packages:
 - ansible > 2.13
 - pip kubernetes
 - kubectl in your PATH
 - helm v3 in your PATH
 - helm diff plugin

 Kubernetes:
 - A k8s cluster
 - A cert issuer already configured in K8s. Like let's encrypt or Cloudflare

## Using the role
1. Install the role with `ansible-galaxy install -r requirements.yml`
2. Optional - Update [ansible.cfg](../../playbook-examples/ansible.cfg) with where the role was installed and run `export ANSIBLE_CONFIG=ansible.cfg` to use it.
3. Install the requirements on your deployment machine.
4. Edit the [values.yaml](../../playbook-examples/values.yaml) with the values you require. See the [awx-operator](https://github.com/ansible/awx-operator) page for more info.
5. Edit the [example playbook](../../playbook-examples/install_awx.yaml) variables with the values you require, some of the variables have defaults. You can pass them in the playbook file like in the example, or on the ansible-playbook cli. 

## Install Operator Example
```
ansible-playbook install_awx.yaml -e kubeconfig_context="default"
```

## Remove Operator Example
```
ansible-playbook install_awx.yaml -e kubeconfig_context="default" -t remove
```

## Role Tags
| Tag Name | Description              |
|----------|--------------------------|
| remove   | Removes the AWX Operator |

## Variables
| Variable Name | Description         | Default |
|----------|--------------------------|---------|
| namespace_name | Defines the Namespace | awx |
| kubeconfig_context: | Defines the k8s context in one of your .kube/config files | docker-desktop |
| git_checkout_dest: /tmp/awx-operator | Where to checkout the git repo | /tmp/awx-operator |
| git_repo_tag | The tag from the AWX-Operator repo to use, which will also be used as the operator image tag | 2.3.0 |
| awx_operator_image_base | Which awx-operator image base to use | quay.io/ansible/awx-operator |
| helm_values_file  | Where the Helm values yaml is located | "{{ playbook_dir }}/values.yaml" |
| kube_env | A yaml map which defines the values for making the Helm chart with Makefile | [ NAMESPACE: "{{ namespace_name }}" IMAGE_TAG_BASE: "{{ awx_operator_image_base }}"] |

## Troubleshooting

 - Sometimes the CRD's and RBAC resources don't always get removed. If this happens, just run the playbook with `-t remove` again.