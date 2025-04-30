# ConfServLog
Centralização de Logs com Rsyslog
Este projeto teve como objetivo implementar a centralização de logs em uma rede local utilizando o serviço rsyslog. A proposta consiste em configurar um servidor dedicado para o recebimento e armazenamento de mensagens de log provenientes de múltiplos clientes, possibilitando uma administração mais eficiente e um monitoramento centralizado dos eventos da rede.

Atividades Realizadas:
Configuração de um servidor Linux para atuar como coletor de logs, recebendo mensagens através do protocolo UDP na porta 514.

Alteração dos arquivos de configuração do rsyslog nos dispositivos clientes e no servidor, definindo as diretivas adequadas para o envio e recebimento dos logs.

Criação de uma regra personalizada no servidor para armazenar as mensagens recebidas dos clientes em um arquivo específico (/var/log/clients.log), facilitando a organização e análise.

Realização de testes práticos entre os clientes e o servidor, comprovando o funcionamento correto da comunicação e registro dos logs.

