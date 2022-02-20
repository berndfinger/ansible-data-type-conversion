A list of dicts with directory and file names as the result of a [conversion from a list of absolute file names](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/create-list-of-dicts-from-list-of-filenames.md)
can be converted back to a list of files, as follows:

[Code](https://github.com/berndfinger/ansible-data-type-conversion/blob/main/sample-code/create-list-of-filenames-from-list-of-dicts.yml):
```
  - name: Convert a list of dicts of directory and file name into a list of files
    set_fact:
      __fact_filenames_only: "{{ __fact_filenames_only|d([]) + [ item.file ] }}"
    loop: "{{ __fact_dir_filenames }}"

  - name: Display the resulting list of files
    debug:
      var: __fact_filenames_only
```

Output:
```
TASK [Display the resulting list of files] *************************************************************************************************
ok: [localhost] => {
    "__fact_filenames_only": [
        "file-01.txt",
        "file-02.txt"
    ]
}
```
