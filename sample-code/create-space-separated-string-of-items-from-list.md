---

- name: create-space-separated-string-of-items-from-list
  hosts: localhost
  vars:
    __fact_filenames:
      - '/var/tmp/file-01.txt'
      - '/var/tmp/file-02.txt'

  tasks:
  - name: Convert list to space separated string, e.g. as part of an argument list
    set_fact:
      __fact_filenames_arg_list: "{{ __fact_filenames | map('quote') | join(' ') }}"

  - name: Display resulting argument list
    debug:
      var: __fact_filenames_arg_list
