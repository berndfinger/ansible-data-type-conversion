For creating a complex argument list with items separated by spaces from a list, with each item prepended by a string, use `product()`, `map('join')`, and `list`. For constructing the complete command, use `map('quote')` and `join(' ')`.

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/create-complex-command-argument-list.yml):
```yaml
  tasks:
  - name: Convert a list of ports into a list of arguments prepended by a string, for firewalld-cmd
    ansible.builtin.set_fact:
      __fact_firewall_cmd_args:
        "{{ ['--add-port='] | product(firewall[0].port) | map('join') | list }}"

  - name: Display the argument list
    ansible.builtin.debug:
      var: __fact_firewall_cmd_args
```

Output:
```yaml
TASK [Display the argument list] ***********************************************************************************************************
ok: [localhost] => {
    "__fact_firewall_cmd_args": [
        "--add-port=1128/tcp",
        "--add-port=1129/tcp",
        "--add-port=4377/tcp",
        ...
```

This is how the command string is built:
```yaml
  - name: Build the final firewall-cmd command string
    ansible.builtin.set_fact:
      __fact_firewall_cmd_command:
        "firewall-cmd --zone=public {{ __fact_firewall_cmd_args | map('quote') | join(' ') }}"

  - name: Display the firewall-cmd command
    ansible.builtin.debug:
      var: __fact_firewall_cmd_command
```

Output:
```yaml
TASK [Display the firewall-cmd command] ****************************************************************************
ok: [localhost] => {
    "__fact_firewall_cmd_command": "firewall-cmd --zone=public --add-port=1128/tcp --add-port=1129/tcp --add-port=4377/tcp --add-port=5050/tcp --add-port=9090/tcp --add-port=9091/tcp --add-port=9092/tcp --add-port=9093/tcp --add-port=37700-37790/tcp --add-port=30105/tcp --add-port=30107/tcp --add-port=30140/tcp --add-port=47701/tcp --add-port=47702/tcp --add-port=47706/tcp --add-port=47712/tcp --add-port=47714/tcp --add-port=47740/tcp --add-port=57700/tcp --add-port=57713/tcp --add-port=57714/tcp --add-port=51000/tcp --add-port=64997/tcp"
}
```
