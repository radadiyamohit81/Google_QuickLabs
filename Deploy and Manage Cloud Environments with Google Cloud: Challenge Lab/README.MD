## Task 1

  * cd /work/dm

  * sed -i s/SET_REGION/us-east1/g prod-network.yaml

  * gcloud deployment-manager deployments create prod-network --config=prod-network.yaml

  * gcloud config set compute/zone us-east1-b

  * gcloud container clusters create kraken-prod \
            --num-nodes 2 \
            --network kraken-prod-vpc \
            --subnetwork kraken-prod-subnet

  * gcloud container clusters get-credentials kraken-prod

  * cd /work/k8s

  * for F in $(ls *.yaml); do kubectl create -f $F; done


  * gcloud config set compute/zone us-east1-b

  * gcloud compute instances create kraken-admin --network-interface="subnet=kraken-mgmt-subnet" --network-interface="subnet=kraken-prod-subnet"

## Task 2

  * gcloud config set compute/zone us-east1-b

  * gcloud container clusters get-credentials spinnaker-tutorial

  * DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")

  * kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &

  * gcloud config set compute/zone us-east1-b

  * gcloud source repos clone sample-app

  * cd sample-app

  * touch a

  * git config --global user.email "$(gcloud config get-value account)"

  * git config --global user.name "Student"

  * git commit -a -m "change"

  * git tag v1.0.1

  * git push --tags

### Note 1: For cheking its working or not till now follow below steps:

#### Steps:
     * click `preview on port 8080`
     * goto `Application > sample > Infrastructure`
     * You can check here backend and frontend services are 100% done or If its not any service is showing then follow below Note.

### Note : If you getting still error follow below steps:

#### Steps:
    * First delete the `sample-app` to go through manual steps:
      1. Navigation Menu > Source Repositories.
      2. View All Repositories
      3. Hover it on `sample-app` and you are able to see setting option on right side and click.
      4. goto general setting
      5. click on `delete this repository`
      6. type your repository name as `sample-app` and click delete
      7. Now goto `google cloud platform > cloud build > Trigger and delete the existing triggers "sample-app-tags" `.
    
    * Do below code into `cloud console`.
      1. gcloud source repos create sample-app
      2. git config credential.helper gcloud.sh
      3. export PROJECT=$(gcloud info --format='value(config.project)')
      4. git remote add origin https://source.developers.google.com/p/$PROJECT/r/sample-app
      5. git push origin master
      6. Now you need to check the `sample-app` is created or not through going to `Navigation Menu > source Repositories`
    
    * Now you can Create New Triggers for that
      1. goto `Navigation Menu > cloud build > Triggers`
      2. Name:sample-app-tags
      3. Event: Push new tag
      4. Select your newly created sample-app repository.
      5. Tag: v.*
      6. Build configuration: Cloud Build configuration file (yaml or json)
      7. Cloud Build configuration file location:   /cloudbuild.yaml
      8. Click `Create`
      
    * Now below code run on `cloud console`
      1. export PROJECT=$(gcloud info --format='value(config.project)')
      2. gsutil mb -l us-central1 gs://$PROJECT-kubernetes-manifests
      3. gsutil versioning set on gs://$PROJECT-kubernetes-manifests
      4. sed -i s/PROJECT/$PROJECT/g k8s/deployments/*
      5. git commit -a -m "Set project ID"
      6. git tag v1.0.0
      7. git push --tags
      8. curl -LO https://storage.googleapis.com/spinnaker-artifacts/spin/1.14.0/linux/amd64/spin
      9. chmod +x spin
     10. ./spin application save --application-name sample \
                        --owner-email "$(gcloud config get-value core/account)" \
                        --cloud-providers kubernetes \
                        --gate-endpoint http://localhost:8080/gate
     11. export PROJECT=$(gcloud info --format='value(config.project)')
         sed s/PROJECT/$PROJECT/g spinnaker/pipeline-deploy.json > pipeline.json
         ./spin pipeline save --gate-endpoint http://localhost:8080/gate -f pipeline.json
     12. Now you need to do `Manual Execution`
     13. click `preview on port 8080`
     14. goto `Application > sample > Infrastructure`
     15. You can check here backend and frontend services are 100% done
     16. goto `Pipelines > then you can check on progress bar to see the person icon Hover it and click continue`
   
   * Now Move to `cloud console` and run below code.
     1. sed -i 's/orange/blue/g' cmd/gke-info/common-service.go
     2. git commit -a -m "Change color to blue"
     3. git tag v1.0.1
     4. git push --tags
     
Hurray! You did it
If you like my code to do with following way please make this repo as `star *`.
Thank you.
      
