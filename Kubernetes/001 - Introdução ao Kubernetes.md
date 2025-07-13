## 1. O que é Kubernetes  
- Orquestrador de containers open-source  
- Automatiza implantação, dimensionamento e gerenciamento de apps em containers  
- Permite focar na aplicação, deixando o “baixo nível” de containers com o Kubernetes  

## 2. Origem e Evolução  
- Surgiu no Google a partir do projeto interno **Borg**, depois evoluído para **Ωmega**  
- Lançado como projeto open-source pela Cloud Native Computing Foundation  
- Forte expertise do Google em escala massiva de containers

## 3. Conceitos Básicos  
1. **APIs**  
   - Todo recurso do Kubernetes é manipulado via chamadas de API  
2. **kubectl**  
   - CLI cliente que interage com as APIs do servidor  
3. **Modelo Declarativo**  
   - Arquivos YAML definem o estado desejado dos objetos  
   - `kubectl apply -f …` atualiza o cluster para corresponder ao YAML

## 4. Arquitetura de Cluster  
- **Cluster**: conjunto de máquinas (nós) que compartilham recursos  
- **Master Node** (Plano de Controle)  
  - **API Server**: recebe e valida requisições  
  - **Controller Manager**: garante que o estado real corresponda ao desejado  
  - **Scheduler**: aloca pods nos nós disponíveis  
- **Worker Nodes**  
  - **kubelet**: agente que recebe instruções do master e gerencia containers  
  - **kube-proxy**: lida com rede e balanceamento de carga entre pods  

## 5. Recursos e Alocação  
- Cada nó expõe CPUs e memória disponíveis  
- O master soma todos os recursos para agendamento eficiente  
- Permite garantir que pods sejam executados em nós que atendam às necessidades declaradas

## 6. Objetos Principais  
1. **Pod**  
   - Menor unidade implantável, agrupa um ou mais containers  
   - Normalmente um container por pod  
2. **Deployment**  
   - Declara réplicas de pods e gerencia atualizações declarativas  
3. **ReplicaSet**  
   - Controla número de réplicas de um pod para manter disponibilidade  

## 7. Fluxo Simplificado  
1. Definir arquivo YAML para Deployment (quantas réplicas, imagens, etc.)  
2. Executar `kubectl apply -f deployment.yaml`  
3. API Server recebe requisição e persiste objeto  
4. Scheduler aloca pods em nós disponíveis  
5. kubelet nos nós cria containers conforme instruído  
6. Deployment/ReplicaSet monitoram e garantem o número desejado de pods  