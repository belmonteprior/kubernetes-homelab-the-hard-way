```markdown
# Kubernetes The Hard Way - 03-network-resolution.md

## Network Resolution and /etc/hosts Configuration

Para cumplir con los nombres de host y FQDN esperados por los manifiestos oficiales (`controller`, `worker-0`, `worker-1`), configuramos las direcciones IP estáticas internas en el archivo de hosts de cada nodo.

### Configuración del archivo `/etc/hosts`
Accede a la shell de cada nodo (`controller`, `worker-0` y `worker-1`) y aplica el siguiente bloque de red:

```bash
multipass shell controller

Una vez dentro de la máquina virtual, aplica el siguiente bloque para configurar el archivo de resolución:
sudo tee /etc/hosts << 'EOF'
127.0.0.1 localhost
10.200.0.10 server.kubernetes.local server
10.200.0.20 node-0.kubernetes.local node-0
10.200.0.30 node-1.kubernetes.local node-1
EOF
(Repite este mismo proceso de acceso y configuración para worker-0 y worker-1).

cat /etc/hosts