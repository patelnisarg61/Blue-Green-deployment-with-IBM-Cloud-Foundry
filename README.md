# Blue-Green-deployment-with-IBM-Cloud-Foundry

Blue-Green deployment is a straight-forward way to achieve deployments without any downtime for your cloudfoundry app. The blue-green deployment approach does this by ensuring you have two production environments, as identical as possible.

In this example, Blue intance is on production and then you deploy your new instance(Green) along with the Blue instance. Lets do this step by step.

## IBM Cloud Foundry Login
```
cf login -a http:bluemix.net -u username -p password -o organization -s space
```

## Blue-Green Deployment (Jenkins execute shell)
```
cf login -a http:bluemix.net -u username -p password -o organization -s space
set -ex 
cf rename blue-app old-blue-app
cf push blue-app
cf stop old-blue-app
cf delete old- blue-app -f
```

##  Multiple Jenkins Job (Blue-Green deployment)

Above script only works for a single jenkins job. So question remains how to automate for multiple jenkins job?

### Steps to write automation script. 
```
1. SSH to your jenkins server. Move to this directory. 
cd /var/lib/jenkins/jobs

2. Create .sh file 
vi blue-green.sh

3. Give access permission to this blue-green.sh file
chmod a+x blue-green.sh

4. Copy this script to your .sh file

set -ex
cf login -a https://bluemix.net -u username -p password -o organization -s space

set +ex
cf rename old-$1 $1

set -ex
cf rename $1 old-$1

set -ex
cf push $1
cf stop old-$1
cf delete old-$1 -f

5. Final step - Write this command to Jenkins execute shell
/var/lib/jenkins/jobs/blue-green.sh app-name
```
**- Nisarg Patel**

