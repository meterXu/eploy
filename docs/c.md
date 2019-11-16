#### k8s

[kubspray](https://github.com/hyd-raiders/kubespray.git)

[helm](https://helm.sh/)   [how to install](https://github.com/hyd-raiders/Cx/blob/master/docs/c2.runBasic.md#%E7%B2%BE%E8%8B%B1%E7%8B%BC%E4%BA%BA-%E6%B5%B7%E5%B0%94%E5%A7%86helm)

[harbo](https://github.com/hyd-raiders/Cx/blob/master/docs/c1.buildFirst.md#boss-哈珀-habor-企业级镜像管理)

[rook](https://github.com/rook/rook)   [quick start](https://rook.github.io/docs/rook/v1.1/ceph-quickstart.html)

```bash

cd cluster/examples/kubernetes/ceph
kubectl create -f common.yaml
kubectl create -f operator.yaml
kubectl create -f cluster-test.yaml

# create storageclass
kubectl create -f csi/rbd/storageclass.yaml

kubectl get storageclass

# test pvc
kubectl create -f ../mysql.yaml

kubectl get pv 
kubectl get pvc
```

[ingress](..)



[chartMuesum](https://github.com/helm/chartmuseum#helm-chart)  

```bash

cat > custom.yaml << EOF
env:
  open:
    STORAGE: local
persistence:
  enabled: true
  accessMode: ReadWriteOnce
  size: 1Gi
  storageClass: "rook-ceph-block"
EOF

helm install --name my-chartmuseum -f custom.yaml stable/chartmuseum
```



[monocular](https://github.com/helm/monocular)