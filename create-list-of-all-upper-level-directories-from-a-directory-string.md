A string containing a directory can be converted to a list of all directories above it, for example for modifying the permissions of all these directories but not recursively from the top level, as follows:

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/create-list-of-all-upper-level-directories-from-a-directory-string.yml):
```
  vars:
    - _dir: '/software/subdir1/subdir2/subdir3/subdir4/'
    - __fact_dirname: ''

  tasks:
    - name: Normalize directory path - remove a "/" from the end of the string if present
      set_fact:
        __fact_dir_normalized: "{{ _dir | regex_replace('\\/$', '') }}"

    - name: Determine the number of directory levels
      set_fact:
        __fact_directory_levels: "{{ __fact_dir_normalized | regex_findall('/') | length }}"

    - name: Fill the list of directories
      include_tasks: ./dir-to-list-of-dirs-loop-block.yml
      loop: "{{ range(1,__fact_directory_levels|int+1,1)|list }}"
      loop_control:
        loop_var: line_item

    - name: Display the resulting list of directories
      debug:
        var: __fact_directories
```

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/dir-to-list-of-dirs-loop-block.yml):
```
# This loop block is necessary because setting the two variables in one set_fact task leads to incorrect results.
- name: Set __fact_dirname
  set_fact:
    __fact_dirname: "{{ (__fact_dirname + '/' + __fact_dir_normalized.split(\"/\")[line_item|int])|string }}"

- name: Set __fact_directories
  set_fact:
    __fact_directories: "{{ __fact_directories|d([]) + [ __fact_dirname ] }}"
```

Output:
```
TASK [Display the resulting list of directories] *******************************************************************************************
ok: [localhost] => {
    "__fact_directories": [
        "/software",
        "/software/subdir1",
        "/software/subdir1/subdir2",
        "/software/subdir1/subdir2/subdir3",
        "/software/subdir1/subdir2/subdir3/subdir4"
    ]
}
```
