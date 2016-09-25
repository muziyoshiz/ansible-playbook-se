# ansible-playbook-se

ansible-playbook-se is a wrapper of ansible-playbook for restricting extra variables to defined ones in extra-vars.yml.

## Usage

1. Clone this repository
2. Add this repository directory to your $PATH
3. Create a extra-vars.yml file in the same directory as the playbook files.
4. Use ansible-playbook-se instead of ansible-playbook

extra-vars.yml should be as follows:

```
---
playbook_name1.yml:
  - allowed_variable_name1
  - allowed_variable_name2

playbook_name2.yml:
  - allowed_variable_name3
  - allowed_variable_name4
```

## Sample

Following three playbooks and one extra-vars.yml are in sample directory:

- playbook1.yml only allows to specify 'allowed1' and 'allowed2' variables in -e option
- playbook2.yml only allows to specify 'allowed1' and 'allowed2' variables in -e option
- playbook3.yml is not in extra-vars.yml and allows to specify all variables in -e option

```
$ ansible-playbook-se playbook1.yml
INFO: No extra-vars in the specified arguments.
 [WARNING]: provided hosts list is empty, only localhost is available


PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [Print allowed variable 1] ************************************************
ok: [localhost] => {
    "msg": "default_value1"
}

TASK [Print allowed variable 2] ************************************************
ok: [localhost] => {
    "msg": "default_value2"
}

TASK [Print denined variable 3] ************************************************
ok: [localhost] => {
    "msg": "default_value3"
}

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0

$ ansible-playbook-se playbook1.yml -e 'allowed1=overwritten'
INFO: All extra-vars are allowed.
 [WARNING]: provided hosts list is empty, only localhost is available


PLAY ***************************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [Print variable 'allowed1'] ***********************************************
ok: [localhost] => {
    "msg": "overwritten"
}

TASK [Print variable 'allowed2'] ***********************************************
ok: [localhost] => {
    "msg": "default_value2"
}

TASK [Print variable 'denied3'] ************************************************
ok: [localhost] => {
    "msg": "default_value3"
}

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0

$ ansible-playbook-se playbook1.yml -e 'denied3=overwritten'
ERROR: extra-var denied3 is not allowed.
ERROR: extra-vars-checker Failed.
```

## License

This script is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
