prp assembler takes and assembles video streams and outputs it as rtp(real time transport protocol) 
tardis takes the internal rtp stream and converts it out as an srt(secure realiable transport) stream
up command is used to launch the docker containers and and perform pre-flight validation
verify command is used to check if live video is actively flowing through the system without needing external video player

curl -sfL https://get.k3s.io | sh -
this is used to install k3s in  a server


# Study the whole thing first then start!


### 1. Initialize Namespace & Configurations


```
kubectl create namespace prp-channels
kubectl create secret generic ch-798-tams-token 
--namespace prp-channels --from-literal=token="bearer" secret/ch-798-tams-token created

kubectl create configmap prp-assembler-slate --namespace prp-channels --from-file=slate.ts=slate.ts

```

### 2. Import & Tag Container Images (can do docker save and load)


```
sudo k3s ctr images import prp-assembler-v0.9-debug.tar.gz
sudo k3s ctr images tag docker.io/library/prp-assembler:v0.9-debug prp-assembler:v0.9-debug
docker save 109667701036.dkr.ecr.us-east-1.amazonaws.com/cp/playout/tardis:tar_1.0.33 | sudo k3s ctr images import -
sudo k3s crictl images | grep -E "prp-assembler|tardis"
```

**while we change the values of the tams url and fcs url, helm-channel-clean.yaml will not be there initially, we need to create it or we can change the section of channel in values.yaml and then update it there, prefer the second one itself.**

## 3.  load the image from local
REMEMBER- if the images is present in docker images then it is not compulsary that it will be in k3s, it should be loaded! do this using -
`docker save <image:tag> | sudo k3s ctr images import -`
### 4. Deploy Helm Chart

Bash

```
helm install ch-798 . \
  --namespace prp-channels \
  -f values.yaml \
  -f values-debug.yaml \
  --set tamsToken.existingSecret=ch-798-tams-token \
  --set slate.existingConfigMap=prp-assembler-slate \
  --set image.pullPolicy=Never 
```
image pull policy is never if it has to use the image that is present in local, this time we removed the line of setting the secret, as we have hardcoded the tams token as it is in dev mode.
This only runs the assembler, we need to do tardis enabled=true and then give the repo of the tardis image that is to be run
### 4. Verify & Play Output Stream

Bash

```
# Check pod status and logs
kubectl get pods -n prp-channels
kubectl logs -n prp-channels -l app.kubernetes.io/instance=ch-798 -c assembler -f
kubectl logs -n prp-channels -l app.kubernetes.io/instance=ch-798 -c tardis -f

# Play stream remotely
ssh amagi@10.0.9.68 "ffmpeg -hide_banner -loglevel error -i 'srt://127.0.0.1:8890?mode=caller&transtype=live&latency=500000' -c copy -f mpegts pipe:1" | ffplay -fflags nobuffer -flags low_delay -f mpegts -i -
```




helm upgrade ch-798 . \
  --namespace prp-channels \
  -f helm-channel-clean.yaml \
  -f values-debug.yaml \
  --set tamsToken.existingSecret=ch-798-tams-token \
  --set slate.existingConfigMap=prp-assembler-slate \
  --set image.pullPolicy=IfNotPresent 

account_id: "amg00861"               # X-Account-Id mesh header
    feed_code: "amg00861c33"                # feed path segment + stream-key derivations
    headend_code: "amg00861c33_001" 



acc id- amg00861
feed code- amg00861c33
headend code- 401

helm install ch-798 . \
  --namespace prp-channels \
  -f values.yaml \
  -f values-debug.yaml \
  --set slate.existingConfigMap=ch-798-slate 


`ffmpeg -hide_banner -loglevel error -i 'srt://127.0.0.1:8890?mode=caller&transtype=live&latency=500000' -c copy -f mpegts pipe:1 | ffplay -fflags nobuffer -flags low_delay -f mpegts -i -`


ssh -o ExitOnForwardFailure=yes -L 127.0.0.1:8092:127.0.0.1:8092 amagi@10.0.9.68 \
  'kubectl -n prp-channels port-forward --address 127.0.0.1 deploy/ch-798-6d755cffbc-sr7qg 8092:8092'