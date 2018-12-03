
![enter image description here](https://lh3.googleusercontent.com/Z17i1m0zQqsIkbzPZVkQNy2BXBotZHnNFCNx9y9rx1R8bFwGVoa_GtNcrz7bo3ZhJA6kG0B5WCIDpg=s1024 "gitlab")
# Gitlab-ce 

### Clone project 
```bash
git clone https://github.com/vandocouto/Gitlab-ce
cd Gitlab-ce
```
### Step by Step

Step 1 - listing labels on nodes
```bash
kubectl get nodes --show-labels
NAME           STATUS   ROLES    AGE   VERSION   LABELS
kubernetes-1   Ready    master   22d   v1.12.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=kubernetes-1,node-role.kubernetes.io/master=
kubernetes-2   Ready    node     22d   v1.12.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=kubernetes-2,nexusStorage=nexus,node-role.kubernetes.io/node=
```
Step 2 -Setting label on node kubernetes-2
```bash
kubectl label nodes kubernetes-2 gitlabStorage=gitlab
node/kubernetes-2 labeled
```
Step 3 - Create object namespaces gitlab
```bash
kubectl apply -f namespaces.yaml
namespace/gitlab created
```
Step 4 - Create volumes PV and PVC (etc -log - opt) 
**Creating the /storage/gitlab directory on node kubernetes-2**

```bash
mkdir -p /storage/gitlab
```

```bash
kubectl apply -f storage.yaml
persistentvolume/volume-gitlab-etc created
persistentvolume/volume-gitlab-log created
persistentvolume/volume-gitlab-opt created
persistentvolumeclaim/gitlab-etc created
persistentvolumeclaim/gitlab-log created
persistentvolumeclaim/gitlab-opt created
```
Step 5 - Create the secret opaque, report keypair (crt / key), convert to base64 - [Base64Encode](https://base64encode.org) 
```bash
 kubectl apply -f secret.yaml
secret/gitlab-tls created
```
Step 6 - Create object deployment 
**Define variables**
```bash
 kubectl apply -f deployment.yaml
deployment.extensions/gitlab created
```
Step 7 - Create objetct service (port 80 e 22)
```bash
kubectl apply -f service.yaml
service/gitlab-service created
```
Step 8 - Create object ingress 
**Define variables**
```bash
 kubectl apply -f ingress.yaml
ingress.extensions/gitlab-ingress created
```
Step 9 - View pod logs 
```bash
kubectl logs gitlab-55966b6876-mbxkb -n gitlab -f
```

access your browser and type https://gitlab.domain.com
![enter image description here](https://lh3.googleusercontent.com/84KjGHjcfvcgdgPPKW-438YHheWaeT4jA_Vz_oxXNMlTzfphP3SMGhm_JYVWCfiTbGdtpBEnLy_Mbg=s1024 "Gitlab1")
Set new password user root 
![enter image description here](https://lh3.googleusercontent.com/7vX-of4c07UFHaIT9tV-8L3HEqFRb32Pu6m7sK-C4rs1s8Dh6dhyXtYuqCytSIu4wNzPjX9IDnU4pA=s1024 "gitlab2")

Go to admin area

![enter image description here](https://lh3.googleusercontent.com/A0ZnaAbGKNYYdc2QOsHNigAr0KOxFl8G7X9B0sh7SYvIvuytUK9tauc8mp2HDOZoaUaRjWRe_BCuZQ=s1024 "gitlab3")

Go to Settings 
![enter image description here](https://lh3.googleusercontent.com/IMjI3ee54cYaWZn0nsLF9XtPyX2roUZY2lAEKDYbq0vOl-KHmWmBFIaRruB4bsSfY0QvIIOFrTOTWA=s1024 "gitlab5")

Disable Sign-up-restrictions

![enter image description here](https://lh3.googleusercontent.com/zx8E8LFjQ6Md6hUigVoDeYwgpGOLdicM1y4M8-vYeCyeCZvqefdXsgkbCV96JGbYs0lnMaqGkY9IjQ=s1024 "gitlab6")

Save Changes

![enter image description here](https://lh3.googleusercontent.com/7NIYQVQ6QgJFn81lT-5PL6ehxlCIC4is6IhhOLf_7s6Uh4elcdwAA-PdlQHX18bxp9jCAuyi2cgmtQ=s1024 "gitlab7")
