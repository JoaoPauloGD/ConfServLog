Configuração 

1- Escolha do Servidor de Logs
Primeiramente, escolhi um dos computadores da rede para funcionar como servidor central de logs. É nele que todos os registros enviados pelos outros PCs serão armazenados.

2- Configuração do Servidor
2.1- Habilitação do recebimento por UDP e TCP

Acessei o arquivo de configuração principal do rsyslog com o seguinte comando:
bash
CopiarEditar
sudo nano /etc/rsyslog.conf

Dentro do arquivo, adicionei as linhas que habilitam o recebimento de mensagens pelas portas padrão 514, tanto para UDP quanto para TCP:

bash
CopiarEditar
module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="514")

2.2- Criação do diretório de logs remotos
Em seguida, criei o diretório onde os logs dos clientes serão armazenados:

bash
CopiarEditar
sudo mkdir -p /var/log/remotelogs

2.3- Adição da regra de armazenamento
Depois disso, criei um novo arquivo de configuração com:

bash
CopiarEditar
sudo nano /etc/rsyslog.d/remote.conf

E adicionei a seguinte linha, que define onde os logs recebidos serão salvos, separando por hostname e programa:

bash
CopiarEditar
$template RemoteLogs,"/var/log/remotelogs/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs

2.4- Reinicialização do rsyslog
Para aplicar todas as alterações, reiniciei o serviço:

bash
CopiarEditar
sudo systemctl restart rsyslog


3- Configuração dos Clientes
3.1- Configuração do envio de logs

Nos computadores clientes, editei o mesmo arquivo /etc/rsyslog.conf com:

bash
CopiarEditar
sudo nano /etc/rsyslog.conf

E, no final do arquivo, adicionei a linha que direciona os logs para o servidor (substituindo IP_DO_SERVIDOR pelo IP correto):

bash
CopiarEditar
*.* @IP_DO_SERVIDOR:514

3.2- Reinicialização do serviço rsyslog
Depois de feita a configuração, reiniciei o serviço rsyslog em cada cliente:

bash
CopiarEditar
sudo systemctl restart rsyslog

4- Testando o Funcionamento
4.1- Enviando mensagem de teste

Então, para testar se está tudo funcionando, usei o comando logger em um dos clientes para gerar uma mensagem de log:

bash
CopiarEditar
logger "Teste de envio de log para o servidor central"

4.2- Verificando o recebimento no servidor

Logo em seguida, fui até o servidor e verifiquei se a mensagem havia sido recebida. Para isso, usei o comando:

bash
CopiarEditar
cat /var/log/remotelogs/NOME_DO_CLIENTE/logger.log

Se a frase "Teste de envio de log para o servidor central" aparecer no arquivo, significa que tudo está funcionando corretamente.
