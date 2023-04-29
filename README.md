# K8s Example

Example Crossplane repo based off of platform-ref-aws but using kustomize to build out the composition

In the network folder there are kustomize base directories to build both a composition that creates a vpc and one that uses existing. If you use an existing VPC be sure 
dnshostname and dnssupport are enabled.

## Install XRDs and Composition

1. Install customer xrd and compositions
   `kubectl apply -f config/customer`
2. Install k8s xrd and compositions
   `kubectl apply -f config/customer/k8s`
   `kubectl apply -f config/customer/k8s/aws/eks`
3. Install network xrd and compositions
   `kubectl apply -f config/customer/network`
   `kubectl apply -f config/customer/network/aws`

## Create the claim
There are two claims in the example folder:
* customerenv-claim.yaml - uses an existing vpc. Please change the value of vpcId to an existing VPC. Also, this vpc should have dnssupport and dnshostname enabled.
* customerenv-claim-new-vpc.yaml - creates a new vpc

2. Apply the claim
   * Existing VPC: `kubectl apply -f config/examples/customerenv-claim.yaml`
   * New VPC: `kubectl apply -f config/examples/customerenv-claim-new-vpc.yaml`
3. Check composites
   `kubectl get composite`
4. Check managed resources
   `kubectl get managed`
5. Should take 10-20 mins for all resources to become true

## Trouble-shoot
If a managed resource or composite fail to complete, do a describe on each object and look at the events.

## Kustomize
Underneath the K8s and network folders there are base directories. In each base directory there are:
* A resources sub-folder which can be used to build the composition
* A composition-base.yaml file which is the base composition file to build
* A kustomization.yaml file used to select which resources in the resource directory to use and add to the composition file.

### Building the composition
1. Add or modify a file in the resources sub-directory
2. If added a resource, add it to the kustomization.yaml file
3. Build with kustomize and specify the composition file.
   `kustomize build -o [file path]`
4. Apply the new composition
   `kubectl apply -f [file path]`





