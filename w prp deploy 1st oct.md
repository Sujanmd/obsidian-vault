prp assembler takes and assembles video streams and outputs it as rtp(real time transport protocol) 
tardis takes the internal rtp stream and converts it out as an srt(secure realiable transport) stream
up command is used to launch the docker containers and and perform pre-flight validation
verify command is used to check if live video is actively flowing through the system without needing external video player

curl -sfL https://get.k3s.io | sh -
this is used to install k3s in  a server


helm install ch-798 . \
  --namespace prp-channels \
  -f values-debug.yaml \
  -f helm-channel-clean.yaml \
  --set tamsToken.existingSecret=ch-798-tams-token \
  --set slate.existingConfigMap=prp-assembler-slate \
  --set image.pullPolicy=IfNotPresent \
  --set exporter.enabled=false


### 1. Initialize Namespace & Configurations

Bash

```
kubectl create namespace prp-channels
kubectl apply -f tams-token-secret.yaml
kubectl apply -f assembler-slate-cm.yaml
```

### 2. Import & Tag Container Images

Bash

```
sudo k3s ctr images import prp-assembler-v0.9-debug.tar.gz
sudo k3s ctr images tag docker.io/library/prp-assembler:v0.9-debug prp-assembler:v0.9-debug
docker save 109667701036.dkr.ecr.us-east-1.amazonaws.com/cp/playout/tardis:tar_1.0.33 | sudo k3s ctr images import -
sudo k3s crictl images | grep -E "prp-assembler|tardis"
```

### 3. Deploy Helm Chart

Bash

```
helm install ch-798 . \
  --namespace prp-channels \
  -f values-prod.yaml \
  -f helm-channel-clean.yaml \
  -f values-debug.yaml \
  --set tamsToken.existingSecret=ch-798-tams-token \
  --set slate.existingConfigMap=prp-assembler-slate \
  --set image.pullPolicy=IfNotPresent \
  --set podMonitor.enabled=false \
  --set exporter.podMonitor.enabled=false
```

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



ssh amagi@10.0.9.68 "sudo ip netns exec prp-channels ffmpeg -hide_banner -loglevel error -i 'srt://127.0.0.1:8891?mode=caller&transtype=live&latency=500000' -c copy -f mpegts pipe:1" | ffplay -fflags nobuffer -flags low_delay -f mpegts -i -


helm upgrade ch-798 . \
  --namespace prp-channels \
  -f values-prod.yaml \
  -f helm-channel-clean.yaml \
  -f values-debug.yaml \
  --set tamsToken.existingSecret=ch-798-tams-token \
  --set slate.existingConfigMap=prp-assembler-slate \
  --set image.pullPolicy=IfNotPresent \
  --set exporter.podMonitor.enabled=false