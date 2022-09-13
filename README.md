# ETCD Backup
This repository will allow you to configure a cronJob within Openshift to backup etcd within OCP 4.x  

## Prerequisites

- We assume that you already have StorageClass installed and running within the Openshift Cluster 4.x.  

- Clone the etcd_Backup repository:  
`git clone git@github.com:filipe-santos-rh/etcd_backup.git`  

### Installation

*The following automation will create a configMap within the `openshift-etcd-backup` namespace and a cronJob that will run on a daily schedule.*  


##### Step 1
*You will need to create the `openshift-etcd-backup` namespace*  
`oc adm new-project openshift-etcd-backup`  

##### Step 2
*You will need to create a service account within the openshift-etcd-backup namespace*  
`oc create sa backup-sa -n openshift-etcd-backup`  

*You will need to grant access to the newly created service account to allow it to create backups of the etcd database. For ease of the process will grant cluster-admin access to the namespace, **Not recommended for the Production environment, adjust as needed**.*  

`oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:openshift-etcd-backup:backup-sa`  

##### Step 3

*Create the Cronjob within the openshift-etcd-backup namespace*  
`oc create -f ./etcd_backup/backup_etcd.yaml`  

*The conjob is currently configured to run with the schedule @daily. This value could be change by replacing @daily with a [valid schedule format](https://en.wikipedia.org/wiki/Cron)*  

### Review

- *Confirm that the Cronjob was created successfully*  
`oc get cronjobs -n openshift-etcd-backup`

- *If the cronjob executed, review the logs of the cronjob to confirm if there are any errors*  

- *If the cronjob executed successfully, review the volumeSnapshot and confirm it's in ready state*  

[Red Hat article on how to backup and restore Nooba](https://access.redhat.com/solutions/5843261)  