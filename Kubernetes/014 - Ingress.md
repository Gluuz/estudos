Ingress fornece uma camada de roteamento HTTP(S) (L7) na frente dos **Services**, centralizando regras que, caso contrário, ficariam dispersas em vários objetos (NodePort, LoadBalancer, configurações externas de proxy). Ele permite consolidar **TLS**, **virtual hosts baseados em nome**, **fanout por caminho**, políticas de balanceamento e regras customizadas.

## Visão Geral do Capítulo

- O que são **Ingress** e **Ingress Controllers**.
    
- Quando utilizar Ingress em vez de Service do tipo NodePort ou LoadBalancer.
    
- Como definir regras de roteamento (virtual host e fanout de paths).
    
- Como habilitar um Ingress Controller (ex.: NGINX) no Minikube.
    
- Como acessar aplicações via DNS + /etc/hosts.
    

## Por que Ingress?

Sem Ingress:

- Cada aplicação exposta externamente exige um **Service NodePort** (porta alta aleatória) ou um **Service LoadBalancer** (um load balancer externo por service → custo e limites de quota).
    
- Alterar rotas exige mexer em proxies externos, segurança, certificados duplicados.
    

Com Ingress:

- Regras de entrada ficam **declarativas** em objetos versionáveis.
    
- Um único ponto de terminação TLS (ou poucos) serve múltiplos hosts e caminhos.
    
- Balanceamento, afinidade, reescritas, timeouts e cabeçalhos podem ser ajustados por anotações.
    

## Componentes

|Componente|Função|Escopo|
|---|---|---|
|Ingress|Manifesto declarando regras (host, path, backend Service)|Config dos hosts/caminhos|
|Ingress Controller|Processo (reverse proxy / LB L7) que assiste mudanças de Ingress e Services|Execução/roteamento|
|Service (ClusterIP)|Backend alvo; Ingress encaminha para ele|Tráfego interno|
|Pod|Contém containers da aplicação|Execução da app|

## Fluxo de Requisição (L7)

1. Cliente resolve `host` → IP público (ou IP local no caso de Minikube) do Ingress Controller.
    
2. Conexão TLS (opcional) é terminada no Controller.
    
3. Controller avalia: host header + path.
    
4. Seleciona regra mais específica (pathType + ordem de correspondência) → Service backend.
    
5. Encaminha para Endpoint (Pod IP:Port) conforme política de LB (round-robin, least connections, hash - depende do controller).
    

## Estrutura de um Objeto Ingress

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: exemplo
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  ingressClassName: nginx
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /api/(.*)
        pathType: ImplementationSpecific
        backend:
          service:
            name: api-svc
            port:
              number: 80
```

**Campos-chave**:

- `ingressClassName`: vincula ao controller correto.
    
- `rules[*].host`: host HTTP(S) alvo.
    
- `paths[*].path`: padrão de caminho.
    
- `pathType`: _Exact_, _Prefix_ ou _ImplementationSpecific_.
    
- `backend.service.name/port`: destino lógico.
    

## Tipos de Correspondência de Caminho

|   |   |   |
|---|---|---|
|pathType|Comportamento|Observações|
|Exact|Igualdade literal|`/foo` ≠ `/foo/`|
|Prefix|Prefixo com cortes em `/`|`/foo` cobre `/foo/bar`|
|ImplementationSpecific|Dependente do controller (regex / outras)|Poder variar; cuidado portabilidade|

## Cenários Comuns

### 1. Virtual Host (Name-Based Hosting)

Serviços distintos por FQDN.

```
rules:
- host: blue.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: webserver-blue-svc
          port:
            number: 80
- host: green.example.com
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: webserver-green-svc
          port:
            number: 80
```

### 2. Fanout por Caminho (um host, múltiplas apps)

```
rules:
- host: example.com
  http:
    paths:
    - path: /blue
      pathType: Prefix
      backend:
        service:
          name: webserver-blue-svc
          port:
            number: 80
    - path: /green
      pathType: Prefix
      backend:
        service:
          name: webserver-green-svc
          port:
            number: 80
```

## TLS

Adicionar bloco TLS permite terminar certificados no Ingress Controller:

```
spec:
  tls:
  - hosts:
    - app.example.com
    secretName: app-example-com-tls
```

`secretName` refere-se a um **Secret** do tipo `kubernetes.io/tls` contendo `tls.crt` e `tls.key`.

## Anotações Úteis (NGINX)

|   |   |
|---|---|
|Anotação|Função|
|`nginx.ingress.kubernetes.io/service-upstream`|Evita saltar para endpoints diretamente (balance via Service)|
|`nginx.ingress.kubernetes.io/rewrite-target`|Reescrever caminho antes de encaminhar|
|`nginx.ingress.kubernetes.io/ssl-redirect`|Forçar HTTPS|
|`nginx.ingress.kubernetes.io/proxy-body-size`|Ajustar limite de upload|
|`nginx.ingress.kubernetes.io/configuration-snippet`|Injetar trechos NGINX específicos|

> **Obs:** Outras plataformas (Traefik, Kong, HAProxy) têm sistemas próprios de CRDs (IngressRoute, HTTPRoute, etc.).

## Quando Usar Ingress

|   |   |   |   |
|---|---|---|---|
|Situação|NodePort|LoadBalancer|Ingress|
|Teste local rápido|Simples|Exagero|Opcional|
|Muitos serviços externos|Portas confusas|Alto custo e quota|**Ideal**|
|TLS centralizado/múltiplos domínios|Complexo|Vários LBs|**Ingress**|
|Regras dinâmicas / rewrites|Limitado|Limitado|**Ingress**|

## Habilitando NGINX Ingress no Minikube

```
minikube addons enable ingress
kubectl get pods -n ingress-nginx
```

Confirme que o Pod do controller está `Running`.

## Criando o Ingress (Exemplo Virtual Host)

Arquivo `virtual-host-ingress.yaml`:

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: virtual-host-ingress
  annotations:
    nginx.ingress.kubernetes.io/service-upstream: "true"
spec:
  ingressClassName: nginx
  rules:
  - host: blue.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: webserver-blue-svc
            port:
              number: 80
  - host: green.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: webserver-green-svc
            port:
              number: 80
```

Aplicar:

```
kubectl apply -f virtual-host-ingress.yaml
kubectl get ingress
```

## Testando em Minikube

1. Obtenha IP: `minikube ip` (ex.: `192.168.99.100`).
    
2. Adicione em `/etc/hosts`:
    

```
192.168.99.100  blue.example.com green.example.com
```

3. Acesse `http://blue.example.com` e `http://green.example.com`.
    

## Observabilidade e Depuração

|   |   |
|---|---|
|Comando|Objetivo|
|`kubectl describe ingress <nome>`|Ver regras, events, anotações|
|`kubectl logs -n ingress-nginx deploy/ingress-nginx-controller`|Ver decisões de roteamento ou erros TLS|
|`kubectl get endpoints <service>`|Confirmar backends prontos|
|`kubectl exec -n ingress-nginx <pod> -- cat /etc/nginx/nginx.conf`|(Diagnóstico) Inspecionar config gerada|

## Boas Práticas

1. **Separar ambientes**: usar namespaces ou clusters distintos (evita colisões de host).
    
2. **TLS obrigatório**: redirecionar HTTP → HTTPS; automatizar renovação (ex.: cert-manager + ACME).
    
3. **Limitar anotações específicas**: abstrair via Helm/Kustomize para consistência.
    
4. **Versionar manifests**: GitOps para mudanças audíveis.
    
5. **Segurança**: aplicar WAF (ex.: ModSecurity) via anotações ou CRDs; usar `Seccomp`/`NetworkPolicy` para reduzir superfície.
    
6. **Escalabilidade**: para alto tráfego considerar múltiplas réplicas do controller e ativar IPVS (dependendo de implementação) ou external LB na frente.
    
7. **Migração Gradual**: introduzir Ingress apontando para Services existentes antes de desativar NodePorts públicos.
    

## Limitações e Alternativas

|   |   |
|---|---|
|Necessidade|Alternativa|
|L7 + Observabilidade rica, mTLS, routing canário|Service Mesh (Istio, Linkerd)|
|TCP/UDP puro sem HTTP|Service tipo LoadBalancer com anotação específica ou CRD (Ingress não cobre todo TCP/UDP)|
|Gateway API (evolução)|CRDs `Gateway`, `HTTPRoute`, etc. para maior portabilidade|

## Erros Comuns

|   |   |   |
|---|---|---|
|Sintoma|Causa Provável|Solução|
|404 do ingress default backend|Regra não casou host/path|Verificar host header, pathType, DNS/hosts|
|503 / upstream timed out|Pods não prontos ou readiness falhando|`kubectl get pods`, readinessProbe|
|Falha TLS (NET::ERR_CERT)|Secret TLS incorreto ou nomes não batem|Validar CN/SAN, secretName|
|502 Bad Gateway|Porta errada no backend Service|Conferir `service.port.number` e `targetPort`|
|Host resolve IP errado|`/etc/hosts` não configurado ou DNS externo não atualizado|Ajustar hosts/DNS|

## Exemplo Fanout

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: fanout-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: example.com
    http:
      paths:
      - path: /blue
        pathType: Prefix
        backend:
          service:
            name: webserver-blue-svc
            port:
              number: 80
      - path: /green
        pathType: Prefix
        backend:
          service:
            name: webserver-green-svc
            port:
              number: 80
```

## Resumo

Ingress entrega uma camada unificada de roteamento HTTP(S) para múltiplos Services:

- Centraliza TLS e nomes de host.
    
- Reduz custos de múltiplos LoadBalancers.
    
- Simplifica paths, rewrites, canários e políticas.
    
- Exige um Ingress Controller ativo e configurado.
    

Compreender bem regras, pathType, anotações e integração com DNS/TLS é essencial para produção robusta.