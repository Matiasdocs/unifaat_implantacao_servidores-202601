# Respostas TF09 - RA: matheus-mantovani

## Questão 1: Arquitetura e Componentes (Teórica)

a) O **Control Plane** é responsável por gerenciar o estado geral do cluster Kubernetes, incluindo o agendamento de pods, monitoramento de nós e manutenção do estado desejado. Um componente chave que reside nele é o **etcd**, que serve como armazenamento de dados distribuído para o estado do cluster.

b) O principal software que reside nos **Worker Nodes** é o **Kubelet**, que é responsável por garantir que os pods desejados estejam rodando e saudáveis, mantendo o "estado desejado" conforme definido pelos controladores.

## Questão 2: O Pod (Conceito Fundamental) (Teórica)

a) Um Pod pode conter **múltiplos contêineres** para permitir que aplicações relacionadas sejam executadas juntas, compartilhando recursos como rede (mesmo endereço IP e portas) e storage (volumes compartilhados), facilitando a comunicação e coordenação entre eles.

b) É uma prática **inadequada** criar Pods diretamente em produção porque eles não fornecem recursos de gerenciamento de estado desejado, como escalabilidade automática, atualizações rolling ou recuperação de falhas, que são essenciais para ambientes de produção. Em vez disso, deve-se usar controladores como Deployments.

## Questão 3: Manifesto YAML (Prática Teórica)

Os **quatro campos obrigatórios** que devem estar presentes na raiz de **qualquer** manifesto YAML de objeto K8s são:
- `apiVersion`
- `kind`
- `metadata`
- `spec`

## Questão 4: Deployment (Prática)

Ver arquivo `deployment.yaml`.

## Questão 5: Service (Prática)

Ver arquivo `service.yaml`.

## Tarefa Prática Integrada (Obrigatória - Conceitual)

### Cenário:
O usuário final tenta acessar a aplicação Nginx através de um dos Nodes do Cluster K8s.

### Descrição do Fluxo:
1. **Entrada:** A requisição chega na porta do **NodePort** (ex: Node IP:30000), onde o Service está exposto externamente.
2. **Service:** O Service `web-service-tf09` intercepta a requisição e usa o campo `selector` (com `app: web-tf09`) para identificar quais Pods são elegíveis para receber o tráfego.
3. **Proxy:** O componente **kube-proxy** (executando em cada nó) é responsável por rotear o tráfego do Service para um dos Pods ativos, usando regras de iptables ou IPVS.
4. **Destino Final:** A requisição chega a um dos **Pods** (gerenciados pelo Deployment) na porta 80, onde o contêiner Nginx está escutando.

## Passo a passo para subir o ambiente do exercício

1. Certifique-se de que o cluster Kubernetes está rodando e o `kubectl` está configurado para se conectar a ele.
2. Navegue até a pasta com os manifestos: `cd Aula 009/RA-matheus-mantovani`
3. Aplique o Deployment: `kubectl apply -f deployment.yaml`
4. Aplique o Service: `kubectl apply -f service.yaml`
5. Verifique se os recursos foram criados: `kubectl get deployments`, `kubectl get services`, `kubectl get pods`
6. Para acessar a aplicação, encontre a porta NodePort: `kubectl get svc web-service-tf09` e acesse via `http://<Node-IP>:<NodePort>`