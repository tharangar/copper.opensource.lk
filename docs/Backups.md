# Backups



<p align="justify">
Backups and restoration is requied to ensure bussiness continuty in any corporate environment. Copper Email solution is bit complex so it has varity of data sources to be backed up. 
Still this backup and restoration part is not completed, this is the initila plan and hope to complete backup with suitable backup management system with encription .

Even though backing up is automated there should be a possibility to manually run the backup process too. Further restoraion is also should be done using latest backups from a shell coammand.

</p>

Copper system has 4 main data to make sure system is successfully backed up.

1. secret.yaml 
2. tls folder which has key files
3. data folder which has email maildir 
4. groupoffice database backup

<p align="justify">
Above data will be backed up daily in a local repository and central repository which can be restored in any occation by system administrator from his local repository. This central repository will be handled by Hub. Initial deploy script will check whether there is secret.yaml and if it exits it will ask to deploy it and if not it will create new one with user inputs.
After initial installation another script should run to populate groupoffice database backto the database, tls files and maildir back in place.

</p>