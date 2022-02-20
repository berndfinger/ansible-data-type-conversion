The file names as a result of a call to the find module are stored in:  
- a **dict**, containing (among others):  
    - a **list** named `files`, each element being:  
        - a **dict**, containing (among others)
            - an element named `path` with the value of the absolute path name of the file.  

Code:
```yaml
  - name: Find all files of a pattern
    ansible.builtin.find:
      paths:
        - /var/tmp
      patterns:
        - "file-*.txt"
    register: __register_find_result
    changed_when: no
```

Output:
```yaml
__register_find_result: {
        "changed": false,
        "examined": 49,
        "failed": false,
        "files": [
            {
                "atime": 1644591074.602834,
                "ctime": 1644591071.2867982,
                "dev": 64770,
...
                "path": "/var/tmp/file-01.txt",
...
            },
            {
                "atime": 1644591061.6116943,
                "ctime": 1644591045.2365181,
                "dev": 64770,
...
                "path": "/var/tmp/file-02.txt",
...
        ],
        "matched": 2,
        "msg": "All paths examined",
        "skipped_paths": {}
   }
```

You can access a single element of the variable find_result as follows:
Code:
```yaml
  - name: Show the first path of variable find_result
    debug:
      msg: "{{ __register_find_result.files[0].path }}"
```

Output:
```
ok: [localhost] => {
    "msg": "/var/tmp/file-01.txt"
}
```

You can copy the resulting file names into a new list as follows:
Code:
```yaml
  - name: Create list of absolute file names from the find result
    set_fact:
      __fact_filenames: "{{ __fact_filenames|d([]) + [ item.path ] }}"
    loop: "{{ __register_find_result.files }}"
  
  - name: Display the resulting list of absolute file names
    debug:
      var: __fact_filenames
```

Output:
```yaml
TASK [Display the resulting list of absolute file names] ***********************************************************************************
ok: [localhost] => {
    "__fact_filenames": [
        "/var/tmp/file-01.txt",
        "/var/tmp/file-02.txt"
    ]
}
```
