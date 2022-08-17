# Nke Oscar App Objects

This is an example application for NKE FastTrack. The app is containerised version of the Oscar app, a popular e-commerce framework based on Django. For FastTrack purposes, two versions of the deployment are developed/customised (with and without Nutanix Objects for storing static files). This repo holds the manifest of the **Oscar app that stores static files locally**. 

The application version using Nutanix Objects can be found in [this repo](https://gitlab.steph-infra.emeagso.lab/pracdev/nke-oscar-app_objects).

Other useful links:

* [The complete documentation](https://django-oscar.readthedocs.io/en/latest/) for the Oscar framework
* The blog post on nutanix.dev ["Nutanix Cloud Native and Calm: Better Together!"](https://developer.nutanix.com/2019/01/13/nutanix-cloud-native-and-calm-better-together/) (the same application with additional integration with ERA, Calm and Objects)
* The customised codebase for building the app version (containing images for Nutanix shop and changes in the `setting.py` file) can be found in the folder [Nutanix-Oscar](https://github.com/nx-mjelicic/nke_oscar_app/tree/main/Nutanix-Oscar) 

## Application Architecture



## Deployment
To deploy the Oscar application:

* Create a namespace by executing:

  ``` kubectl create ns oscar``` or ```kubectl apply -f /manifests/oscar-app.yaml```

* Deploy a PostgreSQL instance:

  ``` kubectl apply -f /manifests/postgres_statefulset.yaml```

* Deploy the Oscar application:

  ``` kubectl apply -f /manifests/oscar-app.yaml```


Besides the oscar application, a migration job will be created during which the database schema will be created and data will be imported. 

## Test

Check pods status in the oscar namespace:
``` kubectl -n oscar get pods ```

```
NAME                                       READY   STATUS      RESTARTS   AGE
oscar-django-deployment-655db6cf58-5nc4v   1/1     Running     0          9h
oscar-django-migrations-9fc92              0/1     Completed   0          9h
postgres-0                                 1/1     Running     0          9h
```


The app is exposed with a NodePort service. To identify node IP:

``` kubectl get nodes -owide | grep worker ```

Find the service port:


``` kubectl -n oscar get svc oscar-django-service ```

In a browser, open the app URL `http://<Node IP>:<NodePort>/en-gb/` 

## Troubleshooting