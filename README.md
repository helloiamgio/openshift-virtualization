
# OpenShift Virtualization Overview

## 📌 Cos'è

OpenShift Virtualization ti permette di eseguire **macchine virtuali (VM)** all'interno del tuo cluster **Kubernetes/OpenShift**, grazie al progetto [KubeVirt](https://kubevirt.io/).  
Gestisci container e VM con un'unica piattaforma!

---

## 🧱 Componenti Principali

- **HyperConverged Operator (HCO)**  
  L'operatore principale che installa e gestisce tutte le altre componenti.

- **KubeVirt Operator**  
  Installa e aggiorna i componenti fondamentali per eseguire le VM.

- **CDI (Containerized Data Importer)**  
  Permette di importare immagini disco e ISO per creare VM.

- **Network Addons + Multus CNI**  
  Aggiungono funzionalità di rete avanzate, come NIC multiple per VM.

- **virt-controller**  
  Controlla lo stato delle VM e coordina la loro esecuzione.

- **virt-api**  
  Espone le API REST per interagire con le VM.

- **virt-handler**  
  Gira su ogni nodo del cluster e gestisce le VM attive.

- **virt-launcher**  
  Pod isolato per ogni VM, contiene `libvirt/QEMU`.

---

## 🗺️ Architettura

![OpenShift Virtualization Architecture](openshift_virtualization_architecture.png)

---

## 🧾 Esempio YAML

```yaml
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: test-vm
  namespace: my-virtualization-ns
spec:
  running: true
  template:
    metadata:
      labels:
        kubevirt.io/domain: test-vm
    spec:
      domain:
        devices:
          disks:
            - name: containerdisk
              disk:
                bus: virtio
            - name: cloudinitdisk
              disk:
                bus: virtio
        resources:
          requests:
            memory: 1Gi
      volumes:
        - name: containerdisk
          containerDisk:
            image: kubevirt/fedora-cloud-container-disk-demo:latest
        - name: cloudinitdisk
          cloudInitNoCloud:
            userData: |
              #cloud-config
              password: fedora
              chpasswd: { expire: False }
```

---

## 📘 Documento PDF

Per una lettura offline:  
[📄 Scarica il PDF](Managing-Virtual-Machines-with-Red-Hat-OpenShift-Virtualization.pdf)

---

## 🧪 Provalo!

Puoi usare [Minikube](https://minikube.sigs.k8s.io/) o [KIND](https://kind.sigs.k8s.io/) insieme a KubeVirt per sperimentare tutto in locale 🚀
