# Camada de Aplicação II - Aprofundamento

##Objetivo da aula

* Compreender como os protocolos da camada de aplicação funcionam de forma integrada em uma rede cliente-servidor;
* Analisar nomes, serviços, autenticação, conteúdo e mensagens que trafegam;
* Analisar e compreender como esses PDUs são entregues ao usuário final.

## Ao final da aula, vocês devem ser capazes de:

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

Colocar um print aqui e explicar todos os campos


## (1) Análise do PDU do PING usando NOME (DNS) e não IP

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

1.8.3) Olhando para o campo UDP, qual é o SOURCE PORT? Por que?

1.8.4) Ainda no campo UDP, qual é o DESTIONAION PORT? Bate como alguma coisa que você viu nos itens anteriores?

1.8.5) No campo do DNS Query, o que está na varíavel NAME?

1.8.6) No campo DNS Answers, o que está na varíavel NAME?

1.8.7) Ainda no campo DNS Answer, o que está no subcampo IP? Bate com alguma coisa? Justifique.


## Fazer o tráfego da rede para cada protocolo e analisar o PDU

Atividade 1 — Acessar o servidor pelo nome e não pelo IP
Objetivo:
mostrar que, para o usuário, o nome é mais amigável que o endereço numérico.

Passos:
no HTTP-Client, abra o Web Browser;
em vez de digitar o IP do Multi-Server, digite apenas o nome cadastrado no DNS;
observe se a página abre;
depois vá para o modo de simulação e veja que antes da comunicação HTTP houve consulta DNS.

Conceito trabalhado:
o DNS não entrega a página; ele apenas resolve o nome;
o HTTP não resolve nomes; ele entrega conteúdo;
a aplicação ao usuário parece uma coisa só, mas a rede utiliza mais de um protocolo.

Atividade 2 — Editar a página web do servidor
Objetivo:
mostrar que HTTP entrega conteúdo publicado por um servidor.

Passos:
clique no Multi-Server;
vá em Services, depois HTTP;
edite a página inicial do servidor com um texto personalizado, por exemplo:
“Servidor web da turma X — Semana 08b”
salve;
volte ao HTTP-Client e atualize a página.

Conceito trabalhado:
HTTP é um protocolo de entrega de conteúdo;
o servidor hospeda recursos;
o navegador é um cliente de aplicação;
a camada de aplicação não é apenas troca de pacotes, mas entrega de informação útil ao usuário.

Atividade 3 — Diferenciar resolução de nomes e acesso ao serviço
Objetivo:
evitar que o aluno misture DNS com HTTP.

Passos:
no DNS-Client, rode nslookup Multi-Server;
anote o IP retornado;
no HTTP-Client, acesse o mesmo recurso pelo nome;
depois acesse pelo IP;
compare os resultados.

Discussão:
quando usamos nome, precisamos do DNS;
quando usamos IP diretamente, o DNS pode ser dispensado;
o serviço final continua sendo HTTP;
isso ajuda o aluno a separar “descobrir onde está” de “consumir o serviço”.

Atividade 4 — Email além do envio
Objetivo:
aprofundar o papel da aplicação de correio eletrônico.

Passos:
configure duas contas de e-mail em clientes distintos;
envie mensagem de um para o outro;
depois faça o recebimento;
analise no modo simulação o envio e a chegada.

Conceito trabalhado:
o aluno normalmente pensa que “email é uma coisa só”, mas a aplicação envolve funções distintas;
envio e recebimento não são exatamente a mesma operação;
há serviço no servidor, autenticação, armazenamento e consulta.

Atividade 5 — FTP com foco em autenticação e permissão
Objetivo:
mostrar que aplicação também envolve controle de acesso.

Passos:
repita o acesso com SUPER e MINI usuário;
faça o upload com SUPER;
tente o mesmo com MINI;
observe a diferença de comportamento.

Conceito trabalhado:
a camada de aplicação também implementa regras de uso;
não é apenas comunicação, mas política de acesso;
o protocolo entrega serviço, mas o servidor decide o que o usuário pode fazer.

Perguntas conceituais fortes para conduzir a aula
Você pode usar perguntas como estas durante a mediação:
Quando o usuário digita um nome no navegador, qual serviço entra em ação antes do site abrir?
Por que acessar um site por nome é mais confortável para humanos do que por IP?
Se o DNS falhar, o servidor web necessariamente parou de funcionar?
O que muda entre um serviço estar ativo e um serviço estar acessível para o usuário?
Por que o navegador mostra conteúdo, mas o nslookup não?
Por que o FTP exige autenticação explícita e permissões diferentes entre usuários?
O que o usuário enxerga como “uma ação simples” que, na verdade, depende de vários serviços em cadeia?



## 4. Parte 4: Verificando o funcionamento do DNS-Server
Escolha um host da esquerda e ping todos os demais hosts da sua topologia. Para isso faça:
* Abra o **Command Prompt** da aba **Desktop** e digite:
* ping `nome do pc` e aguarde o resultado. Note que você não colocou o IP explicitamente e sim, o nome do computador;
* Esse artifício só é possível porque existe o servidor DNS com o nome de cada PC e o seu respectivo IP;
* Você deve simular o caminho do pacote para ver isso acontecer ponto-a-ponto para aprender como é na prática;
* Caso você receba uma mensagem de erro dizendo **request could not find host dns-server. Please check the name and try again.** significa que a sua tabela DNS não possui o respectivo host e IP cadastrado. Confira a tabela novamente e adicione o que estiver faltando.

## 5. Parte 5: Verificando o funcionamento do FTP-Server
Essa parte é muito louca. Você vai salvar um arquivo `.txt` (a partir de qualquer host) no Multi-Server já que você o configurou para suportar o FTP.

### A. Criando um arquivo texto qualquer:
* Vá no host FTP-Client, mas você poderia ir em qualquer host da sua topologia e fazer o mesmo; 
* Vá na aba **Desktop**;
* Clique em **Text Editor**;
* Digite um texto de 250 palavras. Pede o GPTO criar um texto doido com 250 palavras para você. Copie e cole no editor de texto, clique em **File** e salve com o nome `meutexto.txt`;

### B. Configurando o Multi-Server como FTP:
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

### C. Salvando um arquivo .txt no Multi-Server

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




## Responder o Kahoot

