## O que é escalabilidade?

De acordo com Elemar, fundador do site **arquiteturadesoftware.online**, escalabilidade é definida como:

> "A capacidade de sistemas suportarem o aumento (ou a redução) dos workloads incrementando (ou reduzindo) o custo em menor ou igual proporção."

Ou seja, escalabilidade é a habilidade de aumentar ou diminuir a capacidade de processamento de um sistema proporcionalmente ao custo envolvido. Quanto maior o custo computacional, mais capacidade se pode adicionar, e, da mesma forma, é possível reduzir essa capacidade conforme os custos diminuem.

## Escalabilidade vs Performance

Há uma distinção importante entre **escalabilidade** e **performance**:

- **Performance** foca em reduzir a **latência** e aumentar o **throughput**. Se queremos mais performance, o objetivo é diminuir o tempo de resposta e aumentar a quantidade de operações processadas por segundo.
  
- **Escalabilidade**, por outro lado, trata da capacidade de **aumentar ou diminuir** o throughput (quantidade de operações realizadas) conforme a capacidade computacional é ajustada (adicionada ou removida). Ou seja, ao aumentar os recursos computacionais, aumentamos o throughput, e ao reduzir os recursos, o throughput também diminui.

Portanto, um sistema pode ser muito performático, mas isso não implica necessariamente que ele seja escalável. A escalabilidade está mais relacionada à capacidade de adaptar o sistema para diferentes níveis de demanda.

## Escala Vertical e Escala Horizontal

Quando falamos de escalabilidade, geralmente nos referimos a dois tipos: **escala vertical** e **escala horizontal**.

### Escala Vertical

A escala vertical envolve o aumento de recursos computacionais em uma única máquina. Isso pode ser feito ao aumentar a memória RAM, CPU, armazenamento e velocidade de disco. Com mais recursos disponíveis, o sistema consegue lidar com uma maior carga de trabalho.

No entanto, a escalabilidade vertical tem suas limitações:

- Existe um limite físico para o quanto uma máquina pode ser expandida. Nem sempre será possível adicionar mais hardware.
- Colocar "todos os ovos na mesma cesta": se essa máquina falhar, todo o sistema pode ficar fora do ar, pois há uma dependência centralizada nela.

### Escala Horizontal

A escala horizontal, por sua vez, envolve o aumento da quantidade de máquinas para distribuir a carga de trabalho. Ao invés de aumentar os recursos de uma única máquina, adicionamos mais servidores para lidar com as requisições. Para que isso funcione, é necessário utilizar algo como um **proxy reverso** ou um **load balancer** que distribua as requisições entre as várias máquinas de forma balanceada.

Vantagens da escala horizontal:

- Não há necessidade de máquinas extremamente poderosas, apenas várias máquinas que podem trabalhar em conjunto.
- Se uma máquina falhar, as outras continuam funcionando, garantindo maior disponibilidade do sistema.

Hoje em dia, a **escala horizontal** é a abordagem mais comum, especialmente em sistemas distribuídos. No entanto, para que isso funcione bem, o software deve ser projetado para suportar essa arquitetura, algo que exige ajustes específicos.


Escalabilidade é sobre ajustar a capacidade de um sistema conforme a demanda muda, seja adicionando ou removendo recursos. A escala vertical foca no aumento dos recursos em uma única máquina, enquanto a escala horizontal distribui a carga entre várias máquinas. Cada abordagem tem suas vantagens e desvantagens, mas a escala horizontal é a mais utilizada atualmente, devido à sua flexibilidade e resiliência em caso de falhas.
