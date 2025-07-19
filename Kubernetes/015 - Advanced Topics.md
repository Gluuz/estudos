Este capítulo oferece uma visão condensada — porém técnica — de diversos recursos avançados do Kubernetes usados em ambientes corporativos de produção: metadados enriquecidos, políticas de segurança, governança de recursos, escalabilidade automática, execução batch, aplicações com estado, extensibilidade da API, observabilidade, empacotamento e estratégias modernas de deployment.

---

## 1. Annotations

**Annotations** são pares chave–valor arbitrários anexados ao `metadata.annotations` de qualquer objeto. Diferente de _labels_, **não** participam de seleção (selectors) ou agrupamento; destinam‑se a transportar metadados não identitários: informações de build (commit, branch, PR), rastreabilidade (URLs de monitoramento, tracing, dashboards de logs), instruções para controladores (ex.: ingress, service mesh), flags de rollout, revision history.

```
metadata:
  name: webserver
  annotations:
    description: "Deployment PoC – 02 Mar 2022"
    commit: "a1b2c3d"
```

Criação/alteração:

```
kubectl annotate pod mypod key1=value1 key2=value2
```

Use `--save-config=true` para persistir a última configuração aplicada no campo de _managed fields_, auxiliando `kubectl apply` em diff/merge.

---

## 2. Quotas e Limites de Recursos

Para multi‑tenancy justo, usa‑se:

- **ResourceQuota** (por _Namespace_): limita soma agregada de _requests_ / _limits_ de CPU, memória, GPUs, storage, e contagem de objetos (Pods, Services, PVCs etc.).
    
- **LimitRange**: define defaults, mínimos e máximos por _Pod_ ou _Container_, além de razão request:limit.
    

Exemplo de quotas de computação:

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: myspace
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
```

Exemplo de limites por container:

```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-constraint-per-container
  namespace: myspace
spec:
  limits:
  - type: Container
    default:
      cpu: 500m
    defaultRequest:
      cpu: 500m
    max:
      cpu: "1"
    min:
      cpu: 100m
```

**Boas práticas**: sempre definir _requests_ (para scheduling previsível) e _limits_ (para isolamento); alinhar quotas com SLOs e capacidade física.

---

## 3. Autoscaling

Três camadas complementares:

1. **Horizontal Pod Autoscaler (HPA)**: altera réplicas com base em métricas (CPU, memória, métricas customizadas / externas via API de metrics). Ex.:
    
    ```
    kubectl autoscale deploy myapp --min=2 --max=10 --cpu-percent=80
    ```
    
2. **Vertical Pod Autoscaler (VPA)**: recomenda e (modo _Auto_) ajusta `resources.requests` de containers ao longo do tempo (histórico + atual). Evite conflito com HPA baseado na mesma métrica de CPU—combiná‑los exige configuração cuidadosa (ex.: HPA usando métricas externas).
    
3. **Cluster Autoscaler (CA)**: adiciona/remove nós para atender pendências de scheduling ou consolidar capacidade ociosa.
    

**Estratégia**: começar com HPA + requests corretos; introduzir VPA em modo recomendação; ativar CA em clouds ou clusters elásticos.

---

## 4. Jobs e CronJobs

**Job** garante conclusão de tarefas finitas; gerencia retries e paralelismo. Principais campos:

- `parallelism`: nº máximo de pods simultâneos.
    
- `completions`: nº total de execuções bem-sucedidas.
    
- `backoffLimit`: tentativas antes de marcar falha.
    
- `activeDeadlineSeconds`: timeout global.
    
- `ttlSecondsAfterFinished`: garbage collection atrasada.
    

**CronJob** agenda criação de Jobs (formato _cron_ padrão). Campos relevantes:

- `schedule`: expressão cron.
    
- `concurrencyPolicy`: `Allow` | `Forbid` | `Replace`.
    
- `startingDeadlineSeconds`: tolerância a atrasos.
    

---

## 5. StatefulSets

Para workloads _stateful_ que requerem identidade estável (nomes determinísticos, ordinal, volume persistente exclusivo). Proporciona:

- Pods numerados (`pod-0`, `pod-1`...), ordem de criação/terminação.
    
- Associação 1:1 de PVCs (ex.: `volumeClaimTemplates`).
    
- Rolling updates ordenados (pod por pod) com possibilidade de _partitioned updates_. **Desafios**: dependência de um _Headless Service_ (`clusterIP: None`) para DNS estável e de Storage Classes para PVC dinâmico.
    

---

## 6. Custom Resources e Extensibilidade

Dois mecanismos:

- **CRDs (CustomResourceDefinition)**: adicionam novos tipos rapidamente, versionáveis (`spec.versions`). Controller externo (operador) implementa reconciliação.
    
- **API Aggregation**: serviços de API secundários atrás do API Server principal (maior controle, autenticação/autorizações refinadas).
    

**Operadores** (Controller + CRDs) encapsulam lógica operacional (backup, upgrade, scaling) codificada como _reconciliation loops_.

---

## 7. Security Context & Pod Security Admission

**Security Context** (níveis Pod/Container) define:

- Identidade de execução (`runAsUser`, `fsGroup`, `runAsGroup`).
    
- Privilégios: `privileged`, `allowPrivilegeEscalation`.
    
- Capabilities Linux (add/drop).
    
- SELinux/AppArmor profiles.
    
- ReadOnlyRootFilesystem.
    

Exemplo:

```
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - name: app
    image: busybox
    securityContext:
      allowPrivilegeEscalation: false
```

**Pod Security Admission** aplica _profiles_ (Privileged, Baseline, Restricted) por namespace via labels (`pod-security.kubernetes.io/enforce`). Substitui o antigo PodSecurityPolicy.

---

## 8. Network Policies

Controlam tráfego L3/L4 entre Pods/Namespaces/IPs. Componentes:

- `podSelector` alvo.
    
- `policyTypes`: `Ingress`, `Egress`.
    
- Regras: listas de `from` / `to` + portas e protocolos.
    

Boa prática: _default deny_ e whitelisting incremental. Requer CNI compatível (ex.: Calico, Cilium). Exemplo (ingress+egress):

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: demo-netpol
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes: [Ingress, Egress]
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - port: 6379
      protocol: TCP
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - port: 5978
      protocol: TCP
```

---

## 9. Observabilidade: Monitoring, Logging e Troubleshooting

**Monitoring**:

- _Metrics Server_: agregador leve para HPA & `kubectl top`.
    
- _Prometheus_: scraping + TSDB + Alertmanager; integra com Exporters & ServiceMonitor (via Operator).
    

**Logging**:

- Kubernetes só expõe logs de containers ativos (`kubectl logs`); usar stack centralizada (Fluentd / Fluent Bit + Elasticsearch / Loki / OpenSearch).
    

**Troubleshooting**:

- `kubectl describe`, `kubectl get events|events --watch`, `kubectl exec` (inspeção live), `kubectl logs -p` (container anterior).
    
- Captura de rede avançada via eBPF (Cilium / Pixie) ou ferramentas como `kubectl sniff`.
    

---

## 10. Helm

**Helm** empacota múltiplos manifests em **Charts** com:

- `Chart.yaml` (metadados), `values.yaml` (defaults), templates Go (`{{ }}`) + pipeline (`include`, `tpl`).
    
- Versionamento e repositórios (chartmuseum, OCI registries).
    
- _Release_ = instância instalada com histórico para _rollback_. **Boas práticas**: separação de _values_ por ambiente; evitar lógica excessiva em templates; usar _helm lint_ e testes (`helm test`).
    

---

## 11. Service Mesh

Complementa/ou substitui Ingress + NetworkPolicy + métricas básicas adicionando:

- mTLS transparante (criptografia + identidade de workload).
    
- Traffic shaping (circuit breaking, retries, timeouts, canary, mirror/ shadow traffic).
    
- Observabilidade (tracing, metrics, access logs uniformes).
    
- Policy (RBAC de serviço, rate limiting). Arquitetura: **Control Plane** (config API + distribuidores de certificados) e **Data Plane** (sidecars Envoy ou eBPF). Exemplos: Istio, Linkerd, Cilium Service Mesh, Consul, Kuma, Traefik Mesh.
    

**Trade-offs**: overhead de latência e memória, complexidade operacional. Alternativas emergentes com eBPF reduzem sidecars (ambient mesh, sidecarless).

---

## 12. Estratégias de Deployment

- **Rolling Update**: padrão; substituição gradual; mistura versões.
    
- **Canary**: múltiplos Deployments servindo percentuais diferentes (ajuste por réplicas, Ingress, service mesh ou Weighted Service). Bom para testar novas versões com subset de tráfego.
    
- **Blue/Green**: ambientes paralelos; _switch_ atômico de rota (Ingress, LoadBalancer ou troca de selector). Minimiza tempo de rollback.
    
- (Outras não citadas no básico) **A/B Testing**, **Shadow/Mirroring**, **Feature Flags**.
    

**Critérios de escolha**: criticidade, tempo de rollback, necessidade de experimentação, compatibilidade de esquema (DB migrations).

---

## 13. Resumo Rápido

|Tema|Recurso Principal|Objetivo|Observações|
|---|---|---|---|
|Metadados|Annotations|Info não-identitária|Lidas por controladores externos|
|Governaça|ResourceQuota / LimitRange|Fair share & defaults|Evita starvation e overcommit agressivo|
|Escala|HPA / VPA / CA|Ajuste dinâmico|Combinar com cuidado para evitar thrash|
|Batch|Job / CronJob|Execução finita & agendada|Use TTL para limpeza|
|Estado|StatefulSet|Identidade e storage estável|Requer Headless Service + PVCs|
|Extensão|CRD / Aggregation|Novos tipos e APIs|Operadores implementam lógica|
|Segurança|SecurityContext / Pod Security|Restrição de privilégios|Perfis enforced por namespace|
|Rede|NetworkPolicy|Controle de fluxo L3/L4|Depende de CNI compatível|
|Observabilidade|Metrics Server / Prometheus / Logging Stack|Métricas & logs centralizados|Necessário para autoscaling avançado|
|Empacotamento|Helm|Gestão de releases|Values por ambiente|
|Malha|Service Mesh|mTLS, roteamento avançado|Custo operacional|
|Deploy|Rolling, Canary, Blue/Green|Evolução segura|Escolha baseada em risco|

---

## 14. Próximos Passos

- Experimentar criar um CRD + controller simples (Kubebuilder).
    
- Configurar HPA baseado em métrica customizada (Prometheus Adapter).
    
- Implementar política _default deny_ + regras específicas com Calico ou Cilium.
    
- Adicionar Pod Security Labels (`restricted`) aos namespaces sensíveis.
    
- Implantar Helm chart com valores diferenciados por ambiente.
    
- Realizar canary controlado via Ingress annotations ou service mesh.
    

---

_Este capítulo forneceu um panorama dos elementos avançados essenciais para operar Kubernetes em escala de produção. Para aprofundar, recomendo estudar a Gateway API (evolução de Ingress), Open Policy Agent / Kyverno para políticas declarativas, e práticas de segurança de supply chain (SBOM, signatures, admission com cosign)._