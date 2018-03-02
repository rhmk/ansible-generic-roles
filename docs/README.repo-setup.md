This playbook configures are RHEL server to receive its updates from a reposync server.
On your reposync server register the server against RHN and subscribe to the repositories, you want to sync.
Then run the following script as a cron job. It creates the proper repositories and repofiles you need to put to /etc/yum/repos.d

```
--8<-- snip -----
#!/bin/bash

SERVERIP=1.2.3.4 ## change me

cd /var/www/html/repos

#Full or Diff sync the Repos
reposync -n -d -l --downloadcomps --download-metadata

ls -l | grep ^d | awk '{print $9}' | while read dirs; do
  echo $dirs
  if [ -f ${dirs}/comps.xml ]; then
     createrepo -v ${dirs}/ -g comps.xml
   else
     createrepo -v ${dirs}/
  fi
  #createrepo $dirs

  rf=/var/www/html/repofiles/${dirs}.repo
  echo "[$dirs]" > $rf
  echo "name=$dirs" >> $rf
  echo "baseurl=http://${SERVERIP}/repos/$dirs/" >> $rf
  echo "enabled=1" >> $rf
  echo "gpgcheck=0" >> $rf

done
--8<-- snip -----
```

You can then set the following variables in the playbook:

Define the URL where the repofiles exist in "reponame.repo", e.g. rhel-7-server-rpms
reposrvurl: http://$SERVERIP/repofiles/

Set this variable to true if you want to remove/disable all existing repositories
repo_reset: true

Use this of the list of repositories you want to subscribe to 
repositories:
                  - rhel-7-server-rpms
                  - repo2
                  - repo3
