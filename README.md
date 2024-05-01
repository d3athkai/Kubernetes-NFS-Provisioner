# NFS Provisioner
To setup the NFS Storage Class and Persistent Volume Provisioner for any nameapce.  
Based on [Kubernetes NFS Subdir External Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner).  
  
## NFS Provisioner Storage Class
**NOTE: Apply NFS Provisioner Storage Class only once per cluster.**  
  
To setup NFS Provisioner Storage Class:
1. Apply the manifest:  
   `kubectl apply -f nfs-provisioner-class.yaml`
1. Verify the Storage Class:  
   `kubectl get storageclass`
  
To delete NFS Provisioner Storage Class:
1. Apply the manifest:  
   `kubectl delete -f nfs-provisioner-class.yaml`
2. Verify the Storage Class:  
   `kubectl get storageclass`
  
## NFS Provisioner RBAC and Deployment
**NOTE: Apply NFS Provisioner RBAC and Deployment for every namespace that required this NFS storage for data persistence.**  

To setup NFS Provisioner RBAC and Deployment for a namespace:
1. Update the *\<NAMESPACE\>* in **base/kustomization.yaml**
2. Update the *<NFS_SERVER_IP>* & *<NFS_SHARE>* in **overlays/prod/nfs-provisioner-deployment.yaml**
3. Verify the output manifests to be applied:  
   `kubectl kustomize <path-to>/nfs-provisioner/overlays/prod`
4. Apply the manifests:  
   `kubectl kustomize <path-to>/nfs-provisioner/overlays/prod | kubectl apply -f -`
5. Test Persistent Volume Claim:  
   `kubectl apply -f nfs-subdir-test-claim.yaml`
6. Verify the Persistent Volume Claim, ensure the status of the pvc is **Bound**:  
   `kubectl get pvc -n <namespace>`
7. Delete the Persistent Volume Claim:  
   `kubectl delete -f nfs-subdir-test-claim.yaml`
   
To delete NFS Provisioner RBAC and Deployment for a namespace:
1. Update the *\<NAMESPACE\>* in **base/kustomization.yaml**
2. Update the *<NFS_SERVER_IP>* & *<NFS_SHARE>* in **overlays/prod/nfs-provisioner-deployment.yaml**
3. Verify the output manifests to be deleted:  
   `kubectl kustomize nfs-subdir-external-provisioner`
4. Apply the manifests:  
   `kubectl delete -k nfs-subdir-external-provisioner`
