# prometheus on kubernetes
  
  
   
## 先到每台GPU-node做以下步驟

```
git clone https://github.com/kevin7674/nvidia_smi_exporter.git
cd nvidia_smi_exporter
./nvidia_smi_exporter 9101 &
```

### vi /etc/rc.local
```
./nvidia_smi_exporter 9101 &
```

## 回到Master
 
 
 
### 創建nfs空間
```
mkdir -p /nfs/prometheus
echo "/nfs/prometheus *(rw,no_subtree_check,sync,all_squash,anonuid=0,anongid=0)" >> /nfs/exports
```

### 下載
```
git clone https://github.com/kevin7674/prometheus.git
cd prometheus
```

### 修改prometheus-deploy.yaml
```
          nfs:
            server: <NFS_IP>
            path: "/nfs/prometheus"
```

### 修改prometheus-config-map.yaml
```
      - job_name: 'nvidia_smi_exporter'
        static_configs:
        - targets: ['<GPU_NODE_IP>:9101']
        - targets: ['<GPU_NODE_IP>:9101']
```

### 佈署prometheus
```
kubectl create -f node-exporter.yaml
kubectl create -f rbac-setup.yaml.yaml
kubectl create -f prometheus-config-map.yaml
kubectl create -f prometheus-deploy.yaml
```




