# wcm_io_devops.aem_security

This Role applies security changes to an AEM instance.

## admin user password change

At the moment only the password change for the admin user is
implemented.

:bulb: Please note that the password of the Felix Webconsole will not be
changed by this role, since this should be changed by deploying an OSGi
configuration.

Please refer to
[conga-aem-definitions](https://github.com/wcm-io-devops/conga-aem-definitions)
for more information on how to generate the package.

Especially these two places are from interest:
* Template: [aem-cms-system-config.provisioning.hbs](https://github.com/wcm-io-devops/conga-aem-definitions/blob/develop/conga-aem-definitions/src/main/templates/aem-cms/aem-cms-system-config.provisioning.hbs)
* Role: [aem-cms](https://github.com/wcm-io-devops/conga-aem-definitions/blob/develop/conga-aem-definitions/src/main/roles/aem-cms.yaml#L176)

## Usages

This role is used by the Ansible role
[wcm_io_devops.conga_aem_cms](https://github.com/wcm-io-devops/ansible-conga-aem-cms)
to change the admin password during instance setup.

## Requirements

This role requires Ansible 2.0 or higher and was tested with AEM 6.3

## Role Variables

Available variables are listed below, along with their default values:

        aem_security_admin_user: admin

Username of admin user.

        # aem_security_admin_password_new: admin

New Password, set together with aem_security_admin_password_old to change the password.

        aem_security_admin_password_old: admin

Old password, set if you want to change to the new password.

        aem_security_aem_port: 4502

Port and package manager service URL of the AEM instance.

        aem_security_url_base: "http://{{ inventory_hostname }}:{{ aem_security_aem_port }}"

Base url for the AEM instance.

        aem_security_url_userinfo: "{{ aem_security_url_base }}/bin/querybuilder.json?path=/home/users&1_property=rep:authorizableId&1_property.value={{ aem_security_admin_user }}&p.limit=-1"

URL for user info.

        aem_security_url_password_check: "{{ aem_security_url_base }}/crx/de/j_security_check"

URL for password check.

        aem_security_url_password_valid_code: 403

Expected http code for a valid password.

        aem_security_url_password_invalid_code: 401

Expected http code for an invalid password.

        aem_security_url_password_set: "{{ aem_security_url_base }}/crx/explorer/ui/setpassword.jsp"

URL used for setting the new password.

## Dependencies

This role has no dependencies.

## Example Playbook

Changes the admin password from "admin" to "password".

```yaml
- hosts: aem-author
  vars:
    aem_security_admin_password_new: password
    aem_security_admin_password_old: admin
  roles:
    - wcm_io_devops.aem_security
```

## License

Apache 2.0
