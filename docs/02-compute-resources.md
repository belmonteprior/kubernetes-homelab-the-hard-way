# Kubernetes The Hard Way - 02-compute-resources.md

# Sincronización del Repositorio Oficial
Clonamos el repositorio oficial de Kubernetes The Hard Way (v1.32) en tu máquina anfitriona para disponer de las plantillas y listas de descargas.

PowerShell
git clone --depth 1 [https://github.com/kelseyhightower/kubernetes-the-hard-way.git](https://github.com/kelseyhightower/kubernetes-the-hard-way.git)
cd kubernetes-the-hard-way

# Descarga Organizada de Binarios
Descargamos los binarios oficiales (etcd, Kubernetes, containerd, cni-plugins, runc, crictl) directamente en la carpeta downloads/ de tu anfitrión para reutilizarlos en los nodos sin saturar el ancho de banda. (Si ejecutas este paso desde WSL2 dentro de tu entono de Windows):

Bash
ARCH=$(dpkg --print-architecture)
cat downloads-${ARCH}.txt

wget -q --show-progress \
  --https-only \
  --timestamping \
  -P downloads \
  -i downloads-${ARCH}.txt

ls -oh downloads
Extraemos y organizamos los componentes según su rol:  

Bash
{
  ARCH=$(dpkg --print-architecture)
  mkdir -p downloads/{client,cni-plugins,controller,worker}
  tar -xvf downloads/crictl-v1.32.0-linux-${ARCH}.tar.gz \
    -C downloads/worker/
  tar -xvf downloads/containerd-2.1.0-beta.0-linux-${ARCH}.tar.gz \
    --strip-components 1 \
    -C downloads/worker/
  tar -xvf downloads/cni-plugins-linux-${ARCH}-v1.6.2.tgz \
    -C downloads/cni-plugins/
  tar -xvf downloads/etcd-v3.6.0-rc.3-linux-${ARCH}.tar.gz \
    -C downloads/ \
    --strip-components 1 \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcdctl \
    etcd-v3.6.0-rc.3-linux-${ARCH}/etcd
  mv downloads/{etcdctl,kubectl} downloads/client/
  mv downloads/{etcd,kube-apiserver,kube-controller-manager,kube-scheduler} \
    downloads/controller/
  mv downloads/{kubelet,kube-proxy} downloads/worker/
  mv downloads/runc.${ARCH} downloads/worker/runc
  rm -rf downloads/*gz
  chmod +x downloads/{client,cni-plugins,controller,worker}/*
}

# Instalación Local de kubectl
Instalamos kubectl en el entorno anfitrión de Windows/WSL para interactuar con el plano de control una vez levantado el clúster.

En WSL2 / Linux subsistema:

Bash
sudo cp downloads/client/kubectl /usr/local/bin/
kubectl version --client

# Para replicar los recursos de cómputo del repositorio oficial adaptados a nuestras máquinas virtuales con Multipass, levantamos los nodos asegurando los recursos correctos.

### Aprovisionamiento de Máquinas Virtuales
Ejecuta los siguientes comandos desde tu máquina anfitriona:

```bash
multipass launch 24.04 --name controller --cpus 2 --mem 2G --disk 10G
multipass launch 24.04 --name worker-0 --cpus 1 --mem 1G --disk 10G
multipass launch 24.04 --name worker-1 --cpus 1 --mem 1G --disk 10G


### Verificacion de Instancias
multipass list