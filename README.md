# prometheus on kubernetes

創建nfs
```
mkdir -p /nfs/prometheus
echo "/nfs/prometheus *(rw,no_subtree_check,sync,all_squash,anonuid=0,anongid=0)" >> /nfs/exports
```

修改prometheus-deploy.yaml
```
          nfs:
            server: <NFS_IP>
            path: "/nfs/prometheus"
```

GPU-node:
```
git clone https://github.com/kevin7674/nvidia_smi_exporter.git
cd nvidia_smi_exporter
docker build -t="nvidia_smi_exporter:0" .
nvidia-docker run -d --net="host" nvidia_smi_exporter:0 --restart=always
```

下載
```
git clone https://github.com/kevin7674/prometheus.git
cd prometheus
```

修改prometheus-config-map.yaml
```
      - job_name: 'nvidia_smi_exporter'
        static_configs:
        - targets: ['<GPU_NODE_IP>:9101']
        - targets: ['<GPU_NODE_IP>:9101']
```

佈署prometheus
```
kubectl create -f node-exporter.yaml
kubectl create -f rbac-setup.yaml.yaml
kubectl create -f prometheus-config-map.yaml
kubectl create -f prometheus-deploy.yaml
```




