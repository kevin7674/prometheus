# prometheus

啟動prometheus
```
git clone https://github.com/kevin7674/prometheus.git
cd prometheus
kubectl create -f node-exporter.yaml
kubectl create -f rbac-setup.yaml.yaml
kubectl create -f prometheus-config-map.yaml
kubectl create -f prometheus-deploy.yaml
```

GPU-node:
```
git clone https://github.com/kevin7674/nvidia_smi_exporter.git
cd nvidia_smi_exporter
docker build -t="nvidia_smi_exporter:0" .
nvidia-docker run -d --net="host" nvidia_smi_exporter:0
```
