# Archiving Files for CMES Project

## Prerequisites

- ansible
- az cli
- azcopy
- csvkit

## Grabbing the data

1. Created an SAS token to get the fileshare file details. Make sure it has a short lifespan.
    - Allowed Services
        - File
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
1. Get all the files from the content folder

`az storage file list --account-name cmescontent --share-name contentshare --path 'CMES-Pi/assets/content/' --output json --sas-token 'xxx' > data/all_files.json`

1. Get the topics csv and place in the `data/` directory.
   Watch for commas in the descriptions or file names even! 
    1. Clean csv `csvclean data/ArchivedTopic-tillDec2018.csv > data/ArchivedTopic-tillDec2018_clean.csv` 
    1. `csvcut -c 1,4 data/ArchivedTopic-tillDec2018_clean.csv > data/topic_id-and-description.csv`
    1. Manually add column headers to `topic_id-and-description.csv`
    1. `csvjson data/topic_id-and-description.csv > data/old_topics.json`

1. Run `ansible-playbook get_list_of_files_to_archive.yml`
1. Manually convert or delete unicode crap in `old_topics.json`. Search for `\\u` and delete entries.
1. Review results.

## Copying files to archive storage account

1. Create storage account cmesarchive if needed.
1. Install `azcopy` if needed.
1. Create short-lived SAS tokens for the source storage container.
    - Allowed Services
        - File
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
1. Create short-lived SAS tokens for the destination storage container.
    - Allowed Services
        - Blob
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
        - Write
1. Run `ansible-playbook -e source_sas_token='xxx' -e dest_sas_token='xxx' copy_files_to_archive.yml`

## Removing files from share

1. Create storage account cmesarchive if needed.
1. Install `azcopy` if needed.
1. Create short-lived SAS tokens for the source storage container.
    - Allowed Services
        - File
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
        - Delete
1. Run `ansible-playbook -e source_sas_token='xxx' -e sas_token='xxx' remove_files_from_share.yml`
