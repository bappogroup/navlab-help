# Deploy New Changes

## Front End

Changes under the `front-end` folder will trigger automatic build & deployment. A commit in dev branch triggers an automatic deploy on to [dev.navlab.online](https://dev.navlab.online) . Once the new changes are fully tested, the commits can be merged into `master` branch which triggers a deployment to [navlab.online](https://navlab.online) .

## Back End

Changes under the`api` folder triggers an automatic build of image and uploads to AWS Elastic Container Registry but you'll need to manually update the service in Elastic Container Services.

Deployment of API changes is manual at this stage.

1. Go to AWS console, Elastic Container Registry
2. Go to `navlab-api` and copy the tag of the latest image \(or the image you want to use\), e.g. `f21293dfb198ab19da459b2830f75a08a24745ac`
3. Go to Elastic Container Services
4. Click on `Task Definitions` then select the relavent task, i.e.  `api`or `prd-api`,  and create a new revision
5. Click on the container and change the image url to use the latest tag you copied from step 2:

![](.gitbook/assets/image%20%2815%29.png)

Leave the other configs as is and confirm create the new revision.

6. Go to Clusters, choose `dev-api` \(dev env\) or `api` \(prd env\)  
7. Select the only service api1 and click on Update, then use the newly created revision in the Task Definition part. Leave the other configs as is and continue to update the service.

