
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
[📄 Scarica il PDF](Managing Virtual Machines with Red Hat OpenShift Virtualization (DO316)-4.16-student-guide.pdf)

---

## 🌐 Demo GitHub Pages

Se vuoi pubblicare una versione online:  
**https://your-username.github.io/openshift-virtualization-demo/**

Sostituisci `your-username` con il tuo GitHub username e attiva GitHub Pages dalla sezione `Settings > Pages` selezionando la branch `main` e la cartella root (`/`).

---

## 🚀 Come creare la repo GitHub

1. Vai su [https://github.com/new](https://github.com/new)
2. Inserisci il nome: `openshift-virtualization-demo`
3. Metti la repo pubblica (o privata se preferisci)
4. Clicca su **Create repository**
5. Clona la repo sul tuo PC:

```bash
git clone https://github.com/your-username/openshift-virtualization-demo.git
cd openshift-virtualization-demo
```

6. Copia dentro questi file:
   - `README.md`
   - `openshift_virtualization_architecture.png`
   - `openshift_virtualization_overview.pdf`

7. Esegui il push:

```bash
git add .
git commit -m "Initial commit with diagram and documentation"
git push origin main
```

8. (Opzionale) Attiva **GitHub Pages** come descritto sopra ✅

---

## 🧪 Provalo!

Puoi usare [Minikube](https://minikube.sigs.k8s.io/) o [KIND](https://kind.sigs.k8s.io/) insieme a KubeVirt per sperimentare tutto in locale 🚀
