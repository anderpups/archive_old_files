# Moving files using the az cli

1. Created an SAS token for the file storage share. Make sure it has a short lifespan.
    - Allowed Services
        - File
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
1. Get all the files from the content folder

`az storage file list --account-name cmescontent --share-name contentshare --path 'CMES-Pi/assets/content/' --output json --sas-token 'xxx' > all_files.json`

1. Get the Topic IDs from the csv file.

`column -s -t ArchivedTopic-tillDec2018.csv | awk -F ',' '{print $1}' | jq -R [inputs] > old_topic_ids.json`

1. 
