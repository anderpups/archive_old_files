# Moving files using the az cli

1. Created an SAS token to get the fileshare file details. Make sure it has a short lifespan.
    - Allowed Services
        - File
    - Allowed Resource types
        - Container
    - Allowed permissions
        - Read
        - List
1. Get all the files from the content folder

`az storage file list --account-name cmescontent --share-name contentshare --path 'CMES-Pi/assets/content/' --output json --sas-token 'xxx' > all_files.json`

1. Get the Topics from the csv file. Watch for commas in the descriptions or file names even!
    1. Clean csv `csvclean ArchivedTopic-tillDec2018.csv > ArchivedTopic-tillDec2018_clean.csv` 
    1. `csvcut -c 1,4 ArchivedTopic-tillDec2018_clean.csv > topic_id-and-description.csv`
    1. Manually add column headers to `topic_id-and-description.csv`
    1. `csvjson topic_id-and-description.csv > old_topics.json`

1. Run `ansible-playbook -i localhost, site.yml`
1. Manually convert or delete unicode crap in `old_topics.json`. Search for `\\u`
