# fix-update-2-update-set-assignments.py

This script is designed to correct the issue with ServiceNow updates having an update set assignment where the assigned update set is from a different scope as the update itself. This results in an error like the following:

```
Cannot commit Update Set. Update scope id is different than update set scope id 'global'
```

This can be solved by creating a new update set in the same scope as the update and assigning the update to that new update set. This process needs to be repeated for each update that has an incorrect update set assignment. Sometimes this means update sets in multiple scopes need to be created.

Paul Morris has [an article and a ServiceNow application](https://sn-nerd.com/2022/02/12/how-to-prevent-cannot-commit-update-set-issues-in-your-instance/) that you can install to notify you when this happens, and to rectify it _before_ you export your update set.

However, I ran into the problem where I had already exported the update set without the ability to fix it in the instance. This script is designed to fix the update set XML file so that it can be imported into the instance without the error.

It loops through the updates and update set included in the XML and figures out what new update sets need to be created. It then creates those, adds them to the xml and updates the relevant updates to point to the new update set.

## Features

-   **Generate Unique System IDs**: Utilizes UUIDs and MD5 hashing to create unique identifiers for update sets.
-   **Retrieve Applications**: Extracts a list of applications referenced within the update sets.
-   **Manage Update Set Relationships**: Identifies base update sets and creates parent-child relationships between update sets.
-   **Create Update Set Records**: Generates new update set records for applications that do not already have corresponding update sets.
-   **Update Existing Records**: Modifies existing update set records to reflect new relationships and properties.

## Usage

1. Run the script in a Python environment using the command `python fix-update-2-update-set-assignments.py`.
2. Provide the path to the XML file containing the update set records when prompted.
3. Enter the email address to be used for the `created_by` field on any generated update sets.
4. The script will process the XML file and output a new file with the suffix `_fixed.xml`.

## Requirements

-   Python 3.x
-   xml.etree.ElementTree module
-   hashlib module
-   uuid module
-   readline module (for autocompletion during file path input)
-   glob module (for file path completion)

## Note

This script is intended for use with ServiceNow update set XML files. It should be used by individuals with knowledge of ServiceNow update sets and the structure of their XML representations.

## Disclaimer

This script is provided "as is", without warranty of any kind. Use of this script is at your own risk. Always back up your data before running the script on important files.
