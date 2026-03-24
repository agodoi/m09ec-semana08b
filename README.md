# Camada de Aplicação II - Aprofundamento

##Objetivo da aula
Compreender como os protocolos da camada de aplicação funcionam de forma integrada em uma rede cliente-servidor, analisando como nomes, serviços, autenticação, conteúdo e mensagens trafegam e são entregues ao usuário final. Para isso, hoje vamos abrir as PDUs dos protocolos e aprofundar esta análise.


## Ao final da aula, vocês devem ser capazes de:

* Explicar o papel de DNS, HTTP, FTP e Email dentro da camada de aplicação;
* Diferenciar nome de host, endereço IP, porta e serviço;
* Entender que um serviço de aplicação pode depender de outro para funcionar;
* Configurar e testar serviços de aplicação no Packet Tracer de forma intencional;
* Analisar o fluxo completo entre cliente e servidor a partir da perspectiva do usuário e da rede.

## Ponto de Partida

Na camada de aplicação, o usuário não pensa em IP, quadro, segmento ou roteamento. Ele pensa em ações como abrir um site, enviar um e-mail, baixar um arquivo ou procurar um nome. A rede esconde a complexidade e entrega serviços. Estudar a camada de aplicação é estudar como esses serviços são organizados para que a experiência do usuário aconteça.

## Definição do Datagrama

Colocar um print aqui e explicar todos os campos

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

## Responder o Kahoot

