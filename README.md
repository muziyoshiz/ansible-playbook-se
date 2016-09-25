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

## License

This script is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
