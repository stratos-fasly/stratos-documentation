# MongoDB bitnami validation

Validation and Verification for MongoDB bitnami engine deployment.

## Functional Test

- MongoDB functional test can be done with MongoDB compass windows/MAC/Linux client and can be download from this website( https://www.mongodb.com/products/compass ).

- To Connect MongoDB cluster you need to create connection string URI string. If you have created single replica you will get single connection endpoint with port 27017. If you have created multiple replica cluster you have to get all replicas endpoint URL and create connection string.

    1. Get list of services from kubernetes cluster related to MongoDB deployment.
    ```
    kubectl get service -n default | grep {{plugin-name}}
    ```
    Ex:
    ```
    kubectl get service -n default | grep mongodb-demo-v090
    ```

    2. Then you will get external services endpoint URL’s with service type LoadBalancer.

    example out put in here I have created 2 replica cluster here you can see two LoadBalancer type endpoints has created.

    ![plot](./images/kubectl_get_svc_mongoDb.png)

    3. Copy endpoint URL’s and generate connection endpoint. like below.

    ```
    mongodb://{{username}}:{{password}}@{{connection_string_1}}:27017,{{connection_string_2}}:27017,{{connection_string_3}}:27017
    ```

    Username - The username can be root or {{plugin_name}}_user

    Password - Password will be given password when run the engine

    Connection_string - Connection string should take as mentioned above.

    ex:- 

    ```
    mongodb://root:abc123456@a20c6b392c1b2409aa9413604624e004-346325208.us-west-2.elb.amazonaws.com:27017,a9f3c631e0b4646c2b93bb57678272b6-347774253.us-west-2.elb.amazonaws.com:27017,a90e3b1adcc3949fb9a39e28b60a09b3-141899268.us-west-2.elb.amazonaws.com:27017/
    ```
    ![plot](./images/mongodb_bitnami_compass_login.png)
