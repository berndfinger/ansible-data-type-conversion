A list of dicts with directory and file names as the result of a [conversion from a list of absolute file names]
(https://github.com/berndfinger/ansible-data-type-conversion/blob/main/create-list-of-dicts-from-list-of-filenames.md)
can be converted back to a list of files, as follows:

  - name: Create list of files from the find result
    set_fact:
      __fact_filenames_only: "{{ __fact_filenames_only|d([]) + [ item.file ] }}"
    loop: "{{ __fact_dir_filenames }}"

  - name: Display list of files
    debug:
      msg: "{{ __fact_filenames_only }}"
