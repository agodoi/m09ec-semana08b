# Camada de Aplicação II - Aprofundamento

## Objetivos

* Compreender como os protocolos da camada de aplicação funcionam de forma integrada em uma rede cliente-servidor;
* Analisar nomes, serviços, autenticação, conteúdo e mensagens que trafegam;
* Analisar e compreender como esses PDUs são entregues ao usuário final.

## Ao final da aula, vocês serão capazes de:

* Explicar o papel de DNS, HTTP, FTP e Email dentro da camada de aplicação;
* Diferenciar nome de host, endereço IP, porta e serviço;
* Entender que um serviço de aplicação pode depender de outro para funcionar;
* Configurar e testar serviços de aplicação no Packet Tracer de forma intencional;
* Analisar o fluxo completo entre cliente e servidor a partir da perspectiva do usuário e da rede.

## Ponto de Partida

* Na camada de aplicação, o usuário não pensa em IP, quadro, segmento ou roteamento;
* Ele pensa em ações como abrir um site, enviar um e-mail, baixar um arquivo ou procurar um nome;
* A rede esconde a complexidade e entrega serviços;
* Estudar a camada de aplicação é estudar como esses serviços são organizados para que a experiência do usuário aconteça.

## Definição do Datagrama

<img src="https://github.com/agodoi/m09ec-semana08b/blob/main/assets/datagrama.png" width="500">


### VISÃO GERAL DO PACOTE

Você está vendo um pacote que passou pelo **Router2** contendo:

* Camada 2 → Ethernet
* Camada 3 → IP
* Camada 4 → TCP
* É um tráfego **HTTP (porta 80)** sendo transportado.

### 1. CAMADA ETHERNET (Camada 2)

* Aqui acontece algo MUITO importante didaticamente: O MAC muda a cada salto (roteador)

**MAC muda**
**IP NÃO muda**

* Ethernet = entrega local (salto a salto)
* IP = entrega fim a fim

#### PREAMBLE + SFD

* Sincronização do quadro
* Indica início da transmissão

#### DEST ADDR: MAC de destino (próximo salto)

```text
000A.F384.E102
```

#### SRC ADDR: MAC do Router2 (quem está enviando agora)

```text
0001.6454.DD01
```

#### TYPE: 0x0800: Indica que o payload é IPv4

### 2. CAMADA IP (Camada 3)

#### VER: 4 (IPv4)

#### IHL: 5 (Cabeçalho padrão (20 bytes)

#### DSCP: 0x00 (Sem prioridade especial)

#### TL: 20 (ou Total Length que é tamanho do pacote IP)

#### ID: 0x0002 (identificador, usado em fragmentação)

#### FLAGS: 0 (Sem fragmentação)

#### FRAG OFFSET: 0 (Pacote inteiro)

#### TTL: 255 (Time to Live em segundos)

* Em cada roteador é subtratído de -1;
* No roteador da frente você vai ver:

```text
TTL: 254
```

#### PRO: 0x06 (TCP)

#### SRC IP (IP de origem)

```text
192.168.0.190
```
* Esse NÃO muda durante o caminho

#### DST IP (IP de destino)

```text
192.168.0.2
```
* Esse NÃO muda durante o caminho.

### 3. CAMADA TCP (Camada 4)

#### SOURCE PORT: 80 (Servidor web HTTP)

#### DESTINATION PORT: 1025 (Cliente porta efêmera)

#### SEQUENCE NUMBER: 0 (Primeiro byte da transmissão)

#### ACK NUMBER: 1 (Confirma recebimento)

#### FLAGS: 0b00010100

Traduzindo:

| Flag | Valor | Significado      |
| ---- | ----- | ---------------- |
| ACK  | 1     | Confirmação      |
| RST  | 1     | Reset de conexão |

Interpretação:

* Esse pacote está dizendo:
* “Essa conexão não é válida”
* “Estou encerrando/resetando a comunicação”

#### WINDOW: 0 (O receptor não quer mais dados)

* Isso reforça o **reset da conexão**

#### CHECKSUM (Verificação de erro)

#### URGENT POINTER (Não usado aqui)

#### OPTIONS (Campos adicionais, não utilizados)

#### PADDING: 0 (Ajuste de tamanho do pacote)

## INTERPRETAÇÃO COMPLETA DO PACOTE

Esse pacote representa:

* Um **servidor HTTP (porta 80)**
* Respondendo a um cliente
* Mas enviando um **RESET (RST)**
* e bloqueando o fluxo (WINDOW = 0)
* **O QUE ISSO SIGNIFICA NA PRÁTICA?** Possíveis causas no seu laboratório:
* Simulação iniciou no meio da comunicação

## (1) Análise do PDU do PING DNS e ICMP

**Objetivo:** Sua missão é abrir o **Commmand-Prompt** do **PC Email-Client**, digitar `ping PC2` e analisar o PDU em todos os saltos até obter o primeiro pacote ping **Reply from 192.168.0.131: bytes=32 time=10ms TTL=126**. Após isso, preencher um relatório respondendo as perguntas;

1.1) Clica no botão **Simulation** para ativar a simulação passo-a-passo;

1.2) Vá em **Edit Filters** desmarque todas as caixinhas, menos DNS e ICMP ((Internet Control Message Protocol). Este último suporta o comando `ping`;  

1.3) Clica no host **Email-Client**, abra o seu Command-Prompt;

1.4) Digite `ping pc2` e [ENTER]

1.5) Minimiza o **Command-Prompt** e clica 2x no botão **avançar** ou **Alt+c** no teclado até o pacote parar no Roteador 2 (direita);

1.6) Abra esse PDU que saiu do host **Email-Client** clicando no envelope. Vai abrir uma pequena tela **PDU INFORMATION AT DEVICE: xxx**. Que no seu caso, deve ser o **Switch1**;

1.7) Na aba **OSI Model**, responda:

1.7.1) Destination?

1.7.2) Na coluna Out Layers, o que está na CAMADA 7?

1.7.2) Na CAMADA 4, está UDP ou TCP? Qual porta de destino e qual porta de origem? Você enxerga a porta de destino 53? O que isso significa? Faça uma rápida pesquisa.

1.8) Na aba OUTBOUND PDU DETAILS, responda:

1.8.1) Qual é o SRC IP?

1.8.2) Qual é o DST IP?

1.8.3) Olhando para o campo UDP, qual é o SOURCE PORT? Por que? Deveria ser TCP e não UDP? Por que?

1.8.4) Ainda no campo UDP, qual é o DESTIONAION PORT? Bate como alguma coisa que você viu nos itens anteriores?

1.8.5) No campo do DNS Query, o que está na varíavel NAME?

1.8.6) No campo DNS Answers, o que está na varíavel NAME?

1.8.7) Ainda no campo DNS Answer, o que está no subcampo IP? Bate com alguma coisa? Justifique.

1.9) Agora rode a simulação até que 1 pacote ICMP saia da origem **PC Email-Client** e retorne para este host. Abra esse pacote (que deve ser de outra cor), vá no **INBOUD PDU DETAILS** e responda:

1.9.1) No subcampo IP, qual é o SRC IP? Bate com o `ping pc2`

1.9.2) No subcampo IP, qual é o DST IP? Faz sentido a resposta que você está vendo?

1.9.3) **DESAFIO MASTER BLASTER:** descubra a lógica do campo SEQ NUMBER do campo ICMP. Se você conseguir entender está see tornando um ninja em redes.

1.9.4) Qual o valor do sub-campo TL



## (2) Análise do PDU do Serviço Web

**Objetivo:** mostrar que HTTP entrega conteúdo publicado por um servidor.

2.1) Clique no Multi-Server;

2.2) Vá em Services, depois HTTP;

2.3) Edite a página inicial do servidor **index.html** clicando sobre a palavra **edit** com um texto personalizado. Você pode pedir ajuda ao GPeTo pra criar um HTML simples e personalizado de acordo com o seu prompt.

2.4) Salve. Vai dar um pau dizendo que já existe, foda-s...;

2.5) Clique em **File Manage** para voltar à tela anterior;

2.6) Vá no host **HTTP-Client** e atualize a página;

2.7) Confirma que sua página está sendo carregada em qualquer outro host.

2.8) Agora, é a sua vez de fazer uma análise da PDU nessa comunicação com o servidor. Volte no item (1) e faça a consulta de alguns mesmos sub-campos. **Mas antes, filtre marque o protocolo TCP UDP e HTTP no Edit Filters**.

Conceitos trabalhados:
* HTTP é um protocolo de entrega de conteúdo;
* O servidor hospeda recursos;
* O navegador é um cliente de aplicação;
* A camada de aplicação não é apenas troca de pacotes, mas entrega de informação útil ao usuário.

## (3) Análise do PDU do Serviço de Email

**Objetivo:** aprofundar o papel da aplicação de correio eletrônico.

3.1) Adotando os passos da aula anterior, configure a conta de email: [https://github.com/agodoi/m09ec-semana08a](https://github.com/agodoi/m09ec-semana08a)

3.2) Envie mensagem de email para o servidor de email;

3.3) Faça análise da PDU nessa comunicação com o servidor. Volte no item (1) e faça a consulta de alguns mesmos sub-campos. Neste ensaio, alguns sub-campos não vão aparecer porque o serviço mudou. **Remarque o protocolo SMTP e POP3 no Edit Filters**.

## (4) Análise da PDU Servido FTP com foco em autenticação e permissão

**Objetivo:** mostrar que aplicação também envolve controle de acesso.

### 4.1 Criando um arquivo texto qualquer:
* Vá no host FTP-Client, mas você poderia ir em qualquer host da sua topologia e fazer o mesmo; 
* Vá na aba **Desktop**;
* Clique em **Text Editor**;
* Digite um texto de 250 palavras. Pede o GPTO criar um texto doido com 250 palavras para você. Copie e cole no editor de texto, clique em **File** e salve com o nome `meutexto.txt`;

### 4.2 Configurando o Multi-Server como FTP:
* Clique no host **Multi-Server**;
* Vá na aba **Services**;
* Vá no menu vertical **FTP**;
* No campo **User Name** crie um SUPER usuário e uma senha em **Password**. Anote isso aí no canto para não esquecer. Porque SUPER usuário? Porque ele vai ter acesso à tudo no FTP.
* Marque as 5 caixinhas: **Write**, **Read**, **Delete**, **Rename** e **List**;
* Clique em **Add**;
* Agora crie um MINI usuário e com outra senha em **Password**. Novamente, anota isso aí para não esquecer. Este **MINI** vai ter restrições de acesso ao FTP.
* Marque as caixinhas: **Read** e **List**;
* Clique em **Add**.

<img src="https://github.com/agodoi/m09ec-semana08a/blob/main/assets/FTP.png" width="600">

### 4.3 Salvando um arquivo .txt no Multi-Server

* Volte no FTP-Client;
* Abra o **Command-Prompt**;
* Digite `ftp multi-server`;
* Entre com as credenciais do SUPER usuário que você criou;
* Digite `put meutexto.txt` [ENTER];
* E o arquivo será salvo no servidor de arquivos Multi-Server;
* Ainda na mesma tela do **Command-Prompt** do FTP-Client, digite o comando `dir` e veja o seu arquivo listado no Multi-Server;
* Agora digite `quit`para fechar a conexão do SUPER usuário;
* Digite `ftp multi-server`;
* Entre com as credenciais do MINI usuário que você criou;
* Digite `put meutexto.txt` [ENTER];
* Vai dar acesso negado, certo? Isso é normal porque você não habilitou o modo **Write** para este usuário.


## (5) Perguntas Finais

5.1) Quando o usuário digita um nome no navegador, qual serviço entra em ação antes do site abrir? Faça sua verificação no Packet Tracer.

5.2) Por que acessar um site por nome é mais confortável para humanos do que por IP?

5.3) Se o DNS falhar, o servidor web necessariamente parou de funcionar?

5.4) O que muda entre um serviço estar ativo e um serviço estar acessível para o usuário?

5.5) Por que o navegador mostra conteúdo, mas o nslookup não?

5.6) Por que o FTP exige autenticação explícita e permissões diferentes entre usuários?
