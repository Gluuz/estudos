Como implantar uma aplicação isolada no Minikube usando tanto o Dashboard (Web UI) quanto a linha de comando (CLI), além de expor o serviço via **NodePort** e acessá‑lo externamente.

## Objetivos

- Implantar uma aplicação pelo Dashboard do Kubernetes.
    
- Implantar uma aplicação a partir de arquivo YAML com `kubectl`.
    
- Expor o serviço usando o tipo **NodePort**.
    
- Acessar a aplicação fora do cluster Minikube.
    

---

## 1. Implantação via Dashboard

1. Inicie o Minikube e verifique seu status:
    
    ```
    minikube start
    minikube status
    ```
    
2. Execute `minikube dashboard` para abrir a Web UI. Caso o navegador não abra automaticamente, copie o link fornecido no terminal.
    
3. No Dashboard, clique em **+ Create** (canto superior direito) e selecione **Create from form**.
    
4. Preencha os campos:
    
    - **Name**: `web-dash`
        
    - **Image**: `nginx` (ou `docker.io/library/nginx`)
        
    - **Replicas**: `1`
        
    - **Service Type**: `External`, **Port** `8080`, **Target Port** `80`, **Protocol** `TCP`
        
    - **Namespace**: `default`
        
5. Ao clicar em **Deploy**, o Dashboard criará:
    
    - Um **Deployment** `web-dash`
        
    - Um **ReplicaSet** associado
        
    - Um **Pod** rodando o container Nginx
        

> **Dica:** Em **Advanced Options**, você pode ajustar **Labels**, **Requests** de recursos e outros parâmetros.

---

## 2. Verificação pelo CLI

No terminal, você pode listar os recursos criados:

```
kubectl get deploy,rs,po -n default
```

Isso mostrará Deployment, ReplicaSet e Pods correspondentes.

---

## 3. Implantação via YAML & `kubectl`

1. **Excluir** o Deployment anterior (opcional):
    
    ```
    kubectl delete deployment web-dash
    ```
    
2. **Gerar manifesto** do Deployment imperativamente:
    
    ```
    kubectl create deployment webserver \
      --image=nginx:alpine --replicas=3 --port=80 \
      --dry-run=client -o yaml > webserver.yaml
    ```
    
3. **Aplicar** o manifesto:
    
    ```
    kubectl apply -f webserver.yaml
    ```
    
4. **Verificar** criação de ReplicaSet e Pods:
    
    ```
    kubectl get rs,po
    ```
    

> **Alternativa imperativa:**
> 
> ```
> kubectl create deployment webserver --image=nginx:alpine --replicas=3 --port=80
> ```

---

## 4. Expondo o Serviço com NodePort

1. **Criar** manifesto `webserver-svc.yaml`:
    
    ```
    apiVersion: v1
    kind: Service
    metadata:
      name: web-service
      labels:
        app: nginx
    spec:
      type: NodePort
      selector:
        app: nginx
      ports:
      - port: 80
        targetPort: 80
        protocol: TCP
    ```
    
2. **Aplicar** o Service:
    
    ```
    kubectl apply -f webserver-svc.yaml
    ```
    
3. **Ou** exponha diretamente:
    
    ```
    kubectl expose deployment webserver --name=web-service --type=NodePort
    ```
    
4. **Listar** Services:
    
    ```
    kubectl get svc
    ```
    

No resultado, observe o mapeamento `PORT(S)` como `80:<NodePort>`, por exemplo, `80:31074/TCP`.

---

## 5. Acesso Externo

1. Obtenha o IP da VM Minikube:
    
    ```
    minikube ip
    ```
    
2. Acesse em `http://<MINIKUBE_IP>:<NODE_PORT>` no navegador.
    
3. **Alternativa** com o comando:
    
    ```
    minikube service web-service       # abre no navegador
    minikube service web-service --url # retorna a URL
    ```
    

> **Nota:** Em clusters Minikube no Docker driver, pode ser necessário usar:
> 
> ```
> kubectl port-forward svc/web-service 8080:80
> ```
> 
> ou `minikube tunnel` após criar um Service do tipo LoadBalancer.

---

## 6. Probes: Liveness, Readiness e Startup

Para garantir a saúde e disponibilidade da aplicação, o Kubernetes oferece **Probes** que são verificações periódicas:

|Probe|Objetivo|
|---|---|
|Liveness|Reinicia containers não responsivos (exec, HTTP, TCP, gRPC).|
|Readiness|Define quando o Pod está pronto para receber tráfego (mesmos modos do Liveness).|
|Startup|Dá mais tempo para apps legados iniciarem antes de aplicar Liveness/Readiness.|

### 6.1 Exemplo de Liveness Probe (exec)

```
livenessProbe:
  exec:
    command: ["cat", "/tmp/healthy"]
  initialDelaySeconds: 15
  periodSeconds: 5
  failureThreshold: 1
```

### 6.2 Exemplo de Readiness Probe (HTTP)

```
readinessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

### 6.3 Exemplo de Startup Probe

```
startupProbe:
  tcpSocket:
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

Essas verificações permitem que o **kubelet** gerencie reinicializações, roteamento de tráfego e estados de inicialização de forma automatizada.