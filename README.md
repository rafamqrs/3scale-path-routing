# Install APIcast Operator

# Create an APIcast
We must create two APICast, the first is for staging and the second for production.
1. Create a namespace to install the APICast(Optional)
2. Install the APICast Operator(Red Hat Integration - 3scale APIcast gateway)

    - Go to **Operator Hub**  > Search for **"Red Hat Integration - 3scale APIcast gateway"** > Click in **Install** > Select the **Update Channel** with the same version of previous installed **3scale**.
....
# Deploy two APICast 
1. Create a secret with the 3Scale admin credentials on the namespace you want to install the APICast

    - Select namespace: ```oc project <created_namespace>```
    - Create secret: ```oc create secret generic <secret_name>     --from-literal=AdminPortalURL=https://<secret>@<3scale-admin-url>```
    
2. Creating an APICast for staging.

    - Go to **Installed Operators** > **APIcast** > Create instance > fill out the form as only with the values bellow:
        
        - **Name** : <Name of the apicast>. Sample: apicast-staging-custom.
        - **Admin Portal Credentials Ref**. Enter the secret name create in the step 1. 
        - **Embedded Configuration Secret Ref**
            
            - Enable only the **Path Routing Enabled**
            - Set the **Deployment Environment** to **staging**
            - Set the replicas for you desire, replicas: 1
        - Click in Create
    
    - Repeat the previous steps just chaging the **Deployment Environment** to **production**
3. Check if the pods are running with the desire replicas.    

# Create Openshift two Routes to use the new APICast
1. Create Route:

    - oc create route edge --service=<service_staging_name> --hostname=api-3scale-apicast-staging.apps... -n <apicast_namespace>
    - oc create route edge --service=<service_production_name> --hostname=api-3scale-apicast-production.apps... -n <apicast_namespace>
    
# Update the Products to use the Self-Manager APICast
1. Update the Product.

    - Go to Products > Select Product > Settings > DEPLOYMENT > Select APIcast self-managed
    - Check if the **Staging Public Base URL** has the value of route created in the previous step(Create Route Staging)
    - Check if the **Production Public Base URL** has the value of route created in the previous step(Create Route Production)
    - Click on **Update Product** button
    
### **Repeat the previous step for each Product.**  
# Create Mapping Rules for Products
1. Create or Update Mapping Rules.

    - Go to Mapping Rules > Create Mapping Rule > and set the require path > Create Mapping Rule.
    
### **Repeat the previous steps for each Product.**
