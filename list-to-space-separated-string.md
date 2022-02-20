For creating a string with items separated by spaces from a list, use `map('quote')` and `join(' ')`, as in https://github.com/berndfinger/community.sap_install/blob/d70f67c5f176bedbaa48fc63145d1e7b388c5805/roles/sap_general_preconfigure/tasks/RedHat/installation.yml#L53:

```
  - name: Display list of files
    debug:
      msg: "{{ __fact_filenames_only }}"

  - name: Construct argument list 
    set_fact:
      __fact_filenames_arg_list: "{{ __fact_filenames_only | map('quote') | join(' ') }}"

  - name: Display list of files
    debug:
      msg: "{{ __fact_filenames_arg_list }}"
```
