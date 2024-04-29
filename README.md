## Port forwarding
Using the following command:
``` bash
kubectl port-forward pod/task6-1-66df7c9f6f-wb96c 80:3040
```

## Build and upload new docker image
``` bash
docker build -t chunyiwang/task6-2:v2 .
docker push chunyiwang/task6-2:v2
```

## Update pod image
To update the image of Pod, simply update the Deployment section in the configuration file and modify the content under the Container field to:
``` yaml
    spec:
      containers:
      - image: chunyiwang/task6-2:v2
        name: workshop
        resources: {}
```
Apply new configuration
``` bash
kubectl apply -f task6-1.yaml
```
Since no update policy is specified in the configuration file, the default rolling update policy is used. Under this policy, when kubernetes detects a change in the configuration file, it does not simply delete the old Pod and create a new Pod. Instead, it gradually replaces the old Pods with a new Pod, and the new Pod inherits all unchanged old Pod attributes and configurations. Under this update strategy, it may be observed that both new and old Pods exist simultaneously.
It is precisely because of the rolling update strategy that kubernetes can achieve non-stop updates.

## Test new functions
``` bash
curl 'http://10.1.122.2/subtractTwoNumber?n1=15&n2=3'
```
or test with port mapping enabled:
``` bash
curl 'http://localhost/subtractTwoNumber?n1=10&n2=5'
```
