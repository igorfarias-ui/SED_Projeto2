# Células de Manufatura com Robô Compartilhado

Este projeto apresenta a modelagem e simulação de um sistema de manufatura utilizando Redes de Petri Coloridas Hierárquicas (HCPN) no software CPN Tools. O sistema é composto por três células de manufatura automatizadas. Cada célula possui duas máquinas de processamento, um robô industrial responsável pelo transporte das peças e um buffer de saída com capacidade limitada a 2 peças.

Após a produção nas células, um robô global coleta as peças dos buffers das células e as transporta para o buffer final da fábrica, que possui limitação de 4 peças. O objetivo do modelo é representar o comportamento do sistema de produção e analisar possíveis problemas como deadlock e overflow dos buffers.

## Descrição do sistema

O sistema modelado consiste em uma fábrica composta por três células de manufatura automatizadas. Cada célula possui duas máquinas de processamento, um robô responsável pelo transporte interno das peças e um buffer com capacidade de 2 peças. Após o processamento nas células, as peças são coletadas por um robô global, responsável por transportar os itens produzidos até o buffer final da fábrica.

### Componentes do sistema

O sistema de manufatura é composto pelos seguintes elementos:

- 3 células de manufatura;

- 2 máquinas por célula (M1 e M2);

- 1 robô local por célula;

- 1 buffer de saída por célula;

- 1 robô geral responsável pelo transporte final;

- 1 buffer final da fábrica.

Cada componente possui restrições físicas e operacionais que foram incorporadas ao modelo.

## Modelagem do sistema

A modelagem do sistema foi realizada no software **CPN Tools**. Nele, as Redes de Petri são compostas por três elementos principais: places, que representam os estados do sistema; transitions, que representam eventos que alteram o estado do sistema; e tokens, que representam os recursos ou entidades do sistema.

No modelo desenvolvido, os places (elipses) representam estados como máquinas livres, robôs disponíveis e buffers; transitions (retângulos) representam eventos como o início de processamento, coleta de peças e transporte; e tokens representam as peças ou disponibilidade de recursos. A utilização de Redes de Petri Coloridas permite representar diferentes tipos de tokens e estruturas de dados dentro do modelo, aumentando sua capacidade de representação. Abaixo, está uma representação em diagrama de blocos do sistema completo.

<div align = "center">
<img src = "https://github.com/user-attachments/assets/22d66bdf-df8f-4e1a-8c4e-9e50baf2120f" width = "700px" />
</div>

Abaixo, está descrita a modelagem de cada uma das componentes do sistema.

### Máquinas de processamento (M1 e M2)

Cada célula possui duas máquinas de processamento, denominadas M1 e M2. Ambas operam de forma independente, mas apresentam comportamento semelhante.

O funcionamento das máquinas ocorre em três estados principais:

- **Máquina livre**: Pronta para iniciar um novo processamento.

- **Máquina processando**: Executando a operação de fabricação da peça.

- **Peça pronta**: Aguardando a remoção da peça pelo robô.

Uma máquina não pode iniciar um novo ciclo de processamento enquanto a peça anterior não tiver sido removida pelo robô. Essa restrição garante que o sistema respeite a limitação física das máquinas. Há três transitions possíveis: Iniciar_Mx, que inicia o processamento; Fim_Mx, relacionada com a finalização do processamento da peça; e Pegar_Mx, relacionada com a movimentação do robô para pegar as peças prontas.

### Robô da célula

Cada célula possui um robô responsável por transportar peças das máquinas até o buffer da célula.

O robô possui dois estados principais:

- **Robô livre**

- **Robô carregando uma peça**

O robô somente pode coletar uma peça quando a máquina terminar o processamento e o robô estiver disponível. Após coletar a peça, o robô se desloca até o buffer da célula e deposita o item produzido. O modelo apresenta como transitions para o robô o Pegar_Mx, em que ele pega a peça das máquinas para fazer transporte e o Depositar_Buffer, em que ele transporta a peça e deposita ela no buffer.

### Buffer da célula

Cada célula possui um buffer de saída com capacidade limitada a duas peças. Esse buffer representa uma área de armazenamento temporário antes que as peças sejam transportadas para o próximo estágio da produção.

Para modelar essa restrição foi utilizado o conceito de slots disponíveis, representados por tokens adicionais que indicam quantos espaços livres existem no buffer. Quando o buffer atinge sua capacidade máxima, novas peças não podem ser depositadas, o que pode causar bloqueio temporário das máquinas. 

As transitions apresentadas para o buffer da célula são os Depositar_Buffer, em que as peças chegam e Pegar_Cx, em que o robô geral pega a peça para levá-la ao buffer final.

### Robô Geral

O robô geral é responsável por transportar peças dos buffers das células até o buffer final da fábrica. Esse robô coleta peças dos buffers das células e realiza o transporte até o depósito final, permitindo que o sistema continue produzindo mesmo quando os buffers intermediários estão parcialmente ocupados. Ele consegue transportar até duas peças por vez. As transitions relacionadas a esse robô são as de Pegar_Cx, em que ele transporta a peça pronta de uma célula e o Depositar_Final, no qual ele deposita a peça no buffer final. 

### Buffer final da fábrica

O sistema possui um buffer final responsável por armazenar os produtos acabados antes de sua remoção da fábrica. Esse buffer possui capacidade máxima de quatro peças, representando a limitação do espaço de armazenamento disponível na saída do sistema. Assim como nos buffers das células, a limitação de capacidade foi modelada utilizando slots disponíveis.

Esse sistema é limitado pelo BufferGl_Slots, que impõe um limite de 4 peças, além disso as transitions relacionadas com ele são o Depositar_Final, na qual ela recebe peças e o Retirada, em que o agente externo remove uma peça do buffer, disponibilizando mais um slot.

### Estrutura do sistema

O modelo foi desenvolvido utilizando Hierarchial Colored Petri Nets (HCPN). A página inicial representa o sistema completo da fábrica, enquanto páginas internas representam componentes específicos do sistema. Na página principal, chamada Fabrica, cada célula de manufatura é representada por uma Substitution Transition, denominada Célula 1, Célula 2 e Célula 3.  Cada instância de célula reutiliza a mesma página **Celula_Manufatura**, permitindo a reutilização do modelo. Isso reduz a complexidade do sistema e facilita a manutenção e compreensão do modelo.

Abaixo, está a imagem da estrutura geral do sistema de manufatura modelado, que foi nomeado como Fábrica.
<div align = "center">
<img src = "https://github.com/user-attachments/assets/aea1ef7b-4414-4bca-a9c3-858d5781368b" width = "700px" />
</div>

Cada uma das três células segue o modelo da página Celula_Manufatura, que é mostrado abaixo:

<div align = "center">
<img src = "https://github.com/user-attachments/assets/fb2a32fb-e429-4f58-abe1-5e0276e44696" width = "700px" />
</div>


## Funcionamento do modelo

O funcionamento do sistema ocorre da seguinte forma:

```
[1] As máquinas M1 e M2 iniciam o processamento das peças;
[2] Após o término do processamento, as peças ficam disponíveis para coleta;
[3] O robô da célula coleta a peça e a transporta até o buffer da célula;
[4] O buffer armazena temporariamente as peças produzidas;
[5] O robô geral coleta as peças dos buffers das células;
[6] As peças são transportadas até o buffer final da fábrica.
```

Durante a simulação, é possível observar o fluxo de produção nas células, o transporte de peças pelos robôs, a competição por recursos e a limitação de capacidade dos buffers. Esses elementos permitem avaliar o comportamento dinâmico do sistema e identificar gargalos.

Durante a operação do sistema podemos ocorrer alguns bloqueios, como nos casos de buffers cheios, robôs ocupados com outras taredas e máquinas aguardando remoção de peças, o que é comum em sistemas reais.

## Vídeo do Youtube:






