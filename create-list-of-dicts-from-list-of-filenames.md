For creating a list of dicts, each one containing the directory name and the file name from a list of absolute file names
(see also [Convert the result of a call to the find module to a list of filenames](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/find-result-to-list-of-filenames.md)),
use the following code:

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/create-list-of-dicts-from-list-of-filenames.yml):
```yaml
  - name: Convert a list of absolute file names into a list of directories and file names
    set_fact:
      __fact_dir_filenames: "{{ __fact_dir_filenames|d([]) + [ __new_dict ] }}"
    with_items: "{{ __fact_filenames }}"
    vars:
      __new_dict:
        dir: "{{ item | dirname }}"
        file: "{{ item | basename }}"

  - name: Display the resulting list of dicts
    debug:
      var: __fact_dir_filenames
```

Output:
```yaml
TASK [Display the resulting list] **********************************************************************************************************
ok: [localhost] => {
    "__fact_dir_filenames": [
        {
            "dir": "/var/tmp",
            "file": "file-01.txt"
        },
        {
            "dir": "/var/tmp",
            "file": "file-02.txt"
        }
    ]
}
```
