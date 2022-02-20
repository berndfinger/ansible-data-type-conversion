For creating a string with items separated by spaces from a list, use `map('quote')` and `join(' ')`, as in https://github.com/berndfinger/community.sap_install/blob/d70f67c5f176bedbaa48fc63145d1e7b388c5805/roles/sap_general_preconfigure/tasks/RedHat/installation.yml#L53:

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/create-space-separated-string-of-items-from-list.yml):
```
  tasks:
  - name: Convert list to space separated string, e.g. as part of an argument list
    set_fact:
      __fact_filenames_arg_list: "{{ __fact_filenames | map('quote') | join(' ') }}"

  - name: Display resulting argument list
    debug:
      var: __fact_filenames_arg_list
```

Output:
```
TASK [Display resulting argument list] *****************************************************************************************************
ok: [localhost] => {
    "__fact_filenames_arg_list": "/var/tmp/file-01.txt /var/tmp/file-02.txt"
}
```
