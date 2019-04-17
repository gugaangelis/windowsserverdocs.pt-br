---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: "Apêndice L - eventos a serem monitorados"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d153a16b31070f2bbfac4a47a814792ef66b472
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-l-events-to-monitor"></a>Apêndice l: eventos de Monitor

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-l-events-to-monitor"></a>Apêndice l: eventos de Monitor  
A tabela a seguir lista os eventos que você deve monitorar em seu ambiente, de acordo com as recomendações fornecidas no [do Active Directory monitoramento de sinais de comprometimento](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Na tabela a seguir, a coluna "ID de evento de Windows atual" lista a ID de evento como ele é implementado em versões do Windows e Windows Server que estão atualmente no suporte base.  
  
A coluna "ID de evento de Windows herdado" lista a ID de evento correspondente nas versões herdadas do Windows, como computadores cliente que executam o Windows XP ou versões anteriores e servidores que executam o Windows Server 2003 ou versões anteriores. "Potencial importância" coluna identifica se o evento deve ser considerado de baixo, médio ou alto nível de importância em detectando a ataques e a coluna "Event Summary" fornece uma breve descrição do evento.  
  
Um nível de importância potencial de alta significa que uma ocorrência do evento deve ser investigada. Possível importância do média ou baixa significa que esses eventos só devem ser investigados se elas ocorrem inesperadamente ou nos números que excedem significativamente a linha de base esperada em um período de tempo medido. Todas as organizações devem testar essas recomendações em seus ambientes antes de criar alertas que exigem obrigatórias respostas de investigação. Cada ambiente é diferente, e alguns dos eventos classificados com um nível de importância potencial de alta podem ocorrer devido a outros eventos a inofensivos.  
  
|||||  
|-|-|-|-|  
|**ID de evento atual do Windows**|**ID de evento herdado do Windows**|**Nível de importância potencial**|**Resumo de eventos**|  
|4618|N/D|Alto|Ocorreu um padrão de evento de segurança monitorado.|  
|4649|N/D|Alto|Um ataque de repetição foi detectado. Pode ser um falso positivo a inofensivo por causa do erro de configuração incorreta.|  
|4719|612|Alto|Política de auditoria do sistema foi alterada.|  
|4765|N/D|Alto|History SID foi adicionado a uma conta.|  
|4766|N/D|Alto|Falha ao tentar adicionar History SID a uma conta.|  
|4794|N/D|Alto|Foi feita uma tentativa de definir o modo de restauração de serviços de diretório.|  
|4897|801|Alto|Separação de função habilitada:|  
|4964|N/D|Alto|Grupos especiais atribuídos a um novo logon.|  
|5124|N/D|Alto|Uma configuração de segurança foi atualizada sobre o serviço Respondente OCSP|  
|N/D|550|Médio a alto|Ataque possível negação de serviço (DoS)|  
|1102|517|Médio a alto|O log de auditoria foi limpo|  
|4621|N/D|Médio|Administrador recuperou o sistema de CrashOnAuditFail. Os usuários que não são administradores agora poderão fazer logon. Algumas atividades auditáveis podem não ter sido registrada.|  
|4675|N/D|Médio|SIDs filtradas.|  
|4692|N/D|Médio|Tentativa de backup da chave de mestre de proteção de dados.|  
|4693|N/D|Médio|Tentativa de recuperação de chave de mestre de proteção de dados.|  
|4706|610|Médio|Uma nova confiança foi criada em um domínio.|  
|4713|617|Médio|Política de Kerberos foi alterada.|  
|4714|618|Médio|Política de recuperação de dados criptografados foi alterada.|  
|4715|N/D|Médio|A política de auditoria (SACL) em um objeto foi alterada.|  
|4716|620|Médio|Informações de domínio confiável foram modificadas.|  
|4724|628|Médio|Foi feita uma tentativa de redefinir a senha de uma conta.|  
|4727|631|Médio|Foi criado um grupo global com segurança ativada.|  
|4735|639|Médio|Foi alterado um grupo local com segurança ativada.|  
|4737|641|Médio|Foi alterado um grupo global com segurança ativada.|  
|4739|643|Médio|Política de domínio foi alterada.|  
|4754|658|Médio|Foi criado um grupo universal com segurança ativada.|  
|4755|659|Médio|Foi alterado um grupo universal com segurança ativada.|  
|4764|667|Médio|Foi excluído um grupo de segurança desativada|  
|4764|668|Médio|Um tipo do grupo foi alterado.|  
|4780|684|Médio|O ACL foi definido em contas que são membros do grupo Administradores.|  
|4816|N/D|Médio|O RPC detectou uma violação de integridade ao descriptografar uma mensagem recebida.|  
|4865|N/D|Médio|Foi adicionada uma entrada de informações de floresta confiável.|  
|4866|N/D|Médio|Uma entrada de informações de floresta confiável foi removida.|  
|4867|N/D|Médio|Uma entrada de informações de floresta confiável foi modificada.|  
|4868|772|Médio|O Gerenciador de certificado negou uma solicitação de certificado pendente.|  
|4870|774|Médio|Os serviços de certificado revogaram um certificado.|  
|4882|786|Médio|As permissões de segurança para os serviços de certificado alterado.|  
|4885|789|Médio|O filtro de auditoria para os serviços de certificado foi alterado.|  
|4890|794|Médio|As configurações de serviços de certificados do Gerenciador de certificados alterado.|  
|4892|796|Médio|Uma propriedade de serviços de certificado foi alterada.|  
|4896|800|Médio|Um ou mais linhas foram excluídas do banco de dados de certificado.|  
|4906|N/D|Médio|O valor CrashOnAuditFail foi alterado.|  
|4907|N/D|Médio|Configurações de auditoria no objeto foram alteradas.|  
|4908|N/D|Médio|Tabela de Logon de grupos especial modificada.|  
|4912|807|Médio|Por política de auditoria de usuário foi alterada.|  
|4960|N/D|Médio|O IPsec ignorou um pacote recebido que falhou na verificação de integridade. Se este problema persistir, pode indicar que um problema de rede ou que os pacotes estão sendo modificados em trânsito neste computador. Verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Este erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec.|  
|4961|N/D|Médio|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. Se este problema persistir, pode indicar um ataque de repetição contra este computador.|  
|4962|N/D|Médio|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. O pacote recebido tinha um número muito baixo sequência para garantir que não era uma repetição.|  
|4963|N/D|Médio|O IPsec ignorou um pacote recebido de texto simples que deveria ter protegido. Isso geralmente ocorre porque o computador remoto que altera sua diretiva do IPsec sem informar este computador. Isso também pode ser uma tentativa de ataque de falsificação.|  
|4965|N/D|Médio|O IPsec recebeu um pacote de um computador remoto com uma segurança parâmetro de índice (SPI) incorreto. Isso geralmente é causado por hardware com defeito que está corrompendo os pacotes. Se esses erros persistirem, verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Esse erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec. Nesse caso, se a conectividade não estiver impedida, em seguida, esses eventos podem ser ignorados.|  
|4976|N/D|Médio|Durante a negociação do modo principal, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema de rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4977|N/D|Médio|Durante a negociação do modo rápido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema de rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4978|N/D|Médio|Durante a negociação do modo estendido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema de rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4983|N/D|Médio|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|4984|N/D|Médio|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|5027|N/D|Médio|O serviço do Firewall do Windows não pôde recuperar a política de segurança do armazenamento local. O serviço continuará executando a diretiva atual.|  
|5028|N/D|Médio|O serviço do Firewall do Windows foi incapaz de analisar a nova política de segurança. O serviço continuará com a diretiva atualmente executada.|  
|5029|N/D|Médio|O serviço do Firewall do Windows Falha ao inicializar o driver. O serviço continuará a impor a política atual.|  
|5030|N/D|Médio|O serviço do Firewall do Windows não conseguiu iniciar.|  
|5035|N/D|Médio|O Driver do Firewall do Windows não conseguiu iniciar.|  
|5037|N/D|Médio|O Driver do Firewall do Windows detectou um erro de tempo de execução crítico. Encerramento.|  
|5038|N/D|Médio|A integridade do código determinou que o hash de imagem de um arquivo não é válido. O arquivo pode estar corrompido devido a uma modificação não-autorizada, ou o hash inválido pode indicar um erro de dispositivo de disco potencial.|  
|5120|N/D|Médio|Serviço de Respondente OCSP iniciado|  
|5121|N/D|Médio|Serviço de Respondente OCSP parado|  
|5122|N/D|Médio|Uma entrada de configuração alterada no serviço Respondente OCSP|  
|5123|N/D|Médio|Uma entrada de configuração alterada no serviço Respondente OCSP|  
|5376|N/D|Médio|As credenciais do Gerenciador de credenciais foram copiadas.|  
|5377|N/D|Médio|Credenciais do Gerenciador de credenciais foram restauradas a partir de um backup.|  
|5453|N/D|Médio|Negociação IPsec com um computador remoto falhou porque o serviço de chaves IKE e AuthIP IPsec inserindo módulos (IKEEXT) não é iniciado.|  
|5480|N/D|Médio|Serviços IPsec não puderam obter a lista completa de interfaces de rede no computador. Isso representa um potencial risco de segurança porque algumas interfaces de rede talvez não obtenham a proteção fornecida pelos filtros IPsec aplicados. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|  
|5483|N/D|Médio|Os Serviços IPsec não puderam inicializar o servidor RPC. Os Serviços IPsec não pôde ser iniciados.|  
|5484|N/D|Médio|Os Serviços IPsec sofreu uma falha grave e foi desligado. O desligamento dos Serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança.|  
|5485|N/D|Médio|Os Serviços IPsec não puderam processar alguns filtros IPsec em um evento plug and play para interfaces de rede. Isso representa um potencial risco de segurança porque algumas interfaces de rede talvez não obtenham a proteção fornecida pelos filtros IPsec aplicados. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|  
|6145|N/D|Médio|Um ou mais erros ocorreram durante o processamento da política de segurança nos objetos de política de grupo.|  
|6273|N/D|Médio|Servidor de política de rede negou acesso a um usuário.|  
|6274|N/D|Médio|Servidor de política de rede descartou a solicitação de um usuário.|  
|6275|N/D|Médio|Servidor de política de rede descartou a solicitação de contabilização de um usuário.|  
|6276|N/D|Médio|Servidor de política de rede em quarentena um usuário.|  
|6277|N/D|Médio|Servidor de política de rede concedeu acesso a um usuário, mas colocou-o em experiência porque o host não atendeu à política de integridade.|  
|6278|N/D|Médio|Servidor de política de rede concedeu acesso total a um usuário porque o host atendeu a política de integridade definida.|  
|6279|N/D|Médio|Servidor de política de rede bloqueado devido a constantes falhas das tentativas, a conta de usuário.|  
|6280|N/D|Médio|Servidor de política de rede desbloqueado a conta de usuário.|  
|-|640|Médio|Banco de dados de conta geral alterado|  
|-|619|Médio|Qualidade de serviço política mudaram|  
|24586|N/D|Médio|Ocorreu um erro conversão de volume|  
|24592|N/D|Médio|Falha ao tentar reiniciar automaticamente a conversão em volume %2.|  
|24593|N/D|Médio|Gravação de metadados: apresentou erros durante a tentativa de modificar metadados Volume %2. Se o problema persistir, descriptografar o volume|  
|24594|N/D|Médio|Recriação de metadados: uma tentativa de gravar uma cópia dos metadados no volume %2 falhou e pode aparecer corrupção do disco. Se o problema persistir, descriptografe o volume.|  
|4608|512|Baixo|Windows está sendo inicializado.|  
|4609|513|Baixo|Windows está sendo desligado.|  
|4610|514|Baixo|Um pacote de autenticação foi carregado pela autoridade de segurança Local.|  
|4611|515|Baixo|Um processo de logon confiável foi foi registrado com a autoridade de segurança Local.|  
|4612|516|Baixo|Os recursos internos alocados para a fila de mensagens de auditoria se esgotaram, provocando a perda de algumas auditorias.|  
|4614|518|Baixo|Um pacote de notificação foi carregado pelo gerente de contas de segurança.|  
|4615|519|Baixo|Uso inválido da porta LPC.|  
|4616|520|Baixo|A hora do sistema foi alterada.|  
|4622|N/D|Baixo|Um pacote de segurança foi carregado pela autoridade de segurança Local.|  
|4624|528,540|Baixo|Uma conta foi logon com êxito.|  
|4625|529-537,539|Baixo|Uma conta de falha no logon.|  
|4634|538|Baixo|Uma conta foi efetuada o logoff.|  
|4646|N/D|Baixo|Modo de prevenção IKE DoS iniciado.|  
|4647|551|Baixo|O usuário iniciou logoff.|  
|4648|552|Baixo|Tentativa de logon usando credenciais explícitas.|  
|4650|N/D|Baixo|Foi estabelecida uma associação de segurança do modo IPsec principal. Modo estendido não foi habilitado. Autenticação de certificado não foi usada.|  
|4651|N/D|Baixo|Foi estabelecida uma associação de segurança do modo IPsec principal. Modo estendido não foi habilitado. Um certificado foi usado para autenticação.|  
|4652|N/D|Baixo|Falha na negociação do modo principal de IPsec.|  
|4653|N/D|Baixo|Falha na negociação do modo principal de IPsec.|  
|4654|N/D|Baixo|Falha na negociação do modo rápido IPsec.|  
|4655|N/D|Baixo|Uma associação de segurança do modo IPsec principal foi encerrado.|  
|4656|560|Baixo|Um identificador de um objeto foi solicitado.|  
|4657|567|Baixo|Um valor do registro foi modificado.|  
|4658|562|Baixo|O identificador de um objeto foi fechado.|  
|4659|N/D|Baixo|Um identificador de um objeto foi solicitado com a intenção de excluir.|  
|4660|564|Baixo|Um objeto foi excluído.|  
|4661|565|Baixo|Um identificador de um objeto foi solicitado.|  
|4662|566|Baixo|Foi realizada uma operação em um objeto.|  
|4663|567|Baixo|Foi feita uma tentativa de acessar um objeto.|  
|4664|N/D|Baixo|Foi feita uma tentativa de criar um link físico.|  
|4665|N/D|Baixo|Foi feita uma tentativa de criar um contexto de cliente do aplicativo.|  
|4666|N/D|Baixo|Um aplicativo tentou realizar uma operação:|  
|4667|N/D|Baixo|Um contexto de cliente do aplicativo foi excluído.|  
|4668|N/D|Baixo|Um aplicativo foi inicializado.|  
|4670|N/D|Baixo|As permissões em um objeto foram alteradas.|  
|4671|N/D|Baixo|Um aplicativo tentado acessar um ordinal bloqueado através do TBs.|  
|4672|576|Baixo|Privilégios especiais atribuídos ao novo logon.|  
|4673|577|Baixo|Um serviço de privilégios foi chamado.|  
|4674|578|Baixo|Tentativa de uma operação em um objeto privilegiado.|  
|4688|592|Baixo|Um novo processo foi criado.|  
|4689|593|Baixo|Um processo foi encerrado.|  
|4690|594|Baixo|Foi feita uma tentativa de duplicar o identificador de um objeto.|  
|4691|595|Baixo|Foi solicitado o acesso indireto a um objeto.|  
|4694|N/D|Baixo|Tentativa de proteção contra verificação dos dados protegidos.|  
|4695|N/D|Baixo|Tentativa de contra verificação verificação dos dados protegidos.|  
|4696|600|Baixo|Um token primário foi atribuído para processar.|  
|4697|601|Baixo|Tentar instalar um serviço|  
|4698|602|Baixo|Uma tarefa agendada foi criada.|  
|4699|602|Baixo|Uma tarefa agendada foi excluída.|  
|4700|602|Baixo|Uma tarefa agendada estava habilitada.|  
|4701|602|Baixo|Uma tarefa agendada estava desabilitada.|  
|4702|602|Baixo|Uma tarefa agendada foi atualizada.|  
|4704|608|Baixo|Um direito de usuário foi atribuído.|  
|4705|609|Baixo|Um direito de usuário foi removido.|  
|4707|611|Baixo|Uma relação de confiança em um domínio foi removida.|  
|4709|N/D|Baixo|Os Serviços IPsec foram iniciados.|  
|4710|N/D|Baixo|Os Serviços IPsec foi desabilitado.|  
|4711|N/D|Baixo|Pode conter qualquer um destes procedimentos: o PAStore Engine aplicou na máquina a cópia localmente em cache da diretiva IPsec de armazenamento do Active Directory. O PAStore Engine aplicou a diretiva IPsec de armazenamento do Active Directory no computador. O PAStore Engine aplicou a diretiva IPsec de armazenamento de registro local no computador. O PAStore Engine não pôde aplicar cópia localmente em cache da diretiva IPsec de armazenamento do Active Directory no computador. O PAStore Engine não conseguiu aplicar na máquina a diretiva IPsec de armazenamento do Active Directory. O PAStore Engine não conseguiu aplicar na máquina a diretiva IPsec de armazenamento de registro local. O PAStore Engine não pôde aplicar algumas regras da diretiva IPsec ativa no computador. O PAStore Engine não pôde carregar na máquina a diretiva IPsec de armazenamento de diretório. O PAStore Engine carregou na máquina a diretiva IPsec de armazenamento de diretório. O PAStore Engine não pôde carregar na máquina a diretiva IPsec de armazenamento local. O PAStore Engine carregou na máquina a diretiva IPsec de armazenamento local. O PAStore Engine sondou as alterações na diretiva IPsec ativa e não detectou alterações.|  
|4712|N/D|Baixo|Os Serviços IPsec encontraram uma falha possivelmente séria.|  
|4717|621|Baixo|Acesso de segurança do sistema foi concedido a uma conta.|  
|4718|622|Baixo|Acesso de segurança do sistema foi removido de uma conta.|  
|4720|624|Baixo|Uma conta de usuário foi criada.|  
|4722|626|Baixo|Uma conta de usuário foi habilitada.|  
|4723|627|Baixo|Foi feita uma tentativa de alterar a senha de uma conta.|  
|4725|629|Baixo|Uma conta de usuário foi desabilitada.|  
|4726|630|Baixo|Uma conta de usuário foi excluída.|  
|4728|632|Baixo|Um membro foi adicionado a um grupo global com segurança ativada.|  
|4729|633|Baixo|Um membro foi removido de um grupo global com segurança ativada.|  
|4730|634|Baixo|Foi excluído um grupo global com segurança ativada.|  
|4731|635|Baixo|Foi criado um grupo local com segurança ativada.|  
|4732|636|Baixo|Um membro foi adicionado a um grupo local com segurança ativada.|  
|4733|637|Baixo|Um membro foi removido de um grupo local com segurança ativada.|  
|4734|638|Baixo|Foi excluído um grupo local com segurança ativada.|  
|4738|642|Baixo|Uma conta de usuário foi alterada.|  
|4740|644|Baixo|Uma conta de usuário foi bloqueada.|  
|4741|645|Baixo|Uma conta de computador foi alterada.|  
|4742|646|Baixo|Uma conta de computador foi alterada.|  
|4743|647|Baixo|Uma conta de computador foi excluída.|  
|4744|648|Baixo|Foi criado um grupo local com segurança desativada.|  
|4745|649|Baixo|Foi alterado um grupo local com segurança desativada.|  
|4746|650|Baixo|Um membro foi adicionado a um grupo local com segurança desativada.|  
|4747|651|Baixo|Um membro foi removido de um grupo local com segurança desativada.|  
|4748|652|Baixo|Foi excluído um grupo local com segurança desativada.|  
|4749|653|Baixo|Foi criado um grupo global com segurança desativada.|  
|4750|654|Baixo|Foi alterado um grupo global com segurança desativada.|  
|4751|655|Baixo|Um membro foi adicionado a um grupo global com segurança desativada.|  
|4752|656|Baixo|Um membro foi removido de um grupo global com segurança desativada.|  
|4753|657|Baixo|Foi excluído um grupo global com segurança desativada.|  
|4756|660|Baixo|Um membro foi adicionado a um grupo universal com segurança ativada.|  
|4757|661|Baixo|Um membro foi removido de um grupo universal com segurança ativada.|  
|4758|662|Baixo|Foi excluído um grupo universal com segurança ativada.|  
|4759|663|Baixo|Foi criado um grupo universal com segurança desativada.|  
|4760|664|Baixo|Foi alterado um grupo universal com segurança desativada.|  
|4761|665|Baixo|Um membro foi adicionado a um grupo universal com segurança desativada.|  
|4762|666|Baixo|Um membro foi removido de um grupo universal com segurança desativada.|  
|4767|671|Baixo|Uma conta de usuário desbloqueada.|  
|4768|672,676|Baixo|Foi solicitado o tíquete (TGT) de autenticação Kerberos.|  
|4769|673|Baixo|Foi solicitado o tíquete de serviço Kerberos.|  
|4770|674|Baixo|Um tíquete de serviço Kerberos renovado.|  
|4771|675|Baixo|Falha na pré-autenticação Kerberos.|  
|4772|672|Baixo|Falha em uma solicitação de tíquete de autenticação Kerberos.|  
|4774|678|Baixo|Uma conta foi mapeada para o logon.|  
|4775|679|Baixo|Uma conta não pôde ser mapeada para o logon.|  
|4776|680,681|Baixo|O controlador de domínio tentou validar as credenciais de uma conta.|  
|4777|N/D|Baixo|O controlador de domínio falhou ao validar as credenciais de uma conta.|  
|4778|682|Baixo|Uma sessão foi reconectada a uma estação de janela.|  
|4779|683|Baixo|Uma sessão foi desconectada de uma estação de janela.|  
|4781|685|Baixo|O nome de uma conta foi alterado:|  
|4782|N/D|Baixo|O hash da senha uma conta foi acessado.|  
|4783|667|Baixo|Foi criado um grupo de aplicativos básicos.|  
|4784|N/D|Baixo|Foi alterado um grupo de aplicativos básicos.|  
|4785|689|Baixo|Um membro foi adicionado a um grupo de aplicativos básicos.|  
|4786|690|Baixo|Um membro foi removido de um grupo de aplicativos básicos.|  
|4787|691|Baixo|Um não-membro foi adicionado a um grupo de aplicativos básicos.|  
|4788|692|Baixo|Um não-membro foi removido de um grupo de aplicativos básicos.|  
|4789|693|Baixo|Foi excluído um grupo de aplicativos básicos.|  
|4790|694|Baixo|Foi criado um grupo de consulta LDAP.|  
|4793|N/D|Baixo|A API de verificação de política de senha foi chamada.|  
|4800|N/D|Baixo|A estação de trabalho foi bloqueada.|  
|4801|N/D|Baixo|A estação de trabalho foi desbloqueada.|  
|4802|N/D|Baixo|O protetor de tela foi invocado.|  
|4803|N/D|Baixo|O protetor de tela descartado.|  
|4864|N/D|Baixo|Foi detectada uma colisão de namespace.|  
|4869|773|Baixo|Os serviços de certificado receberam uma solicitação de certificado enviada novamente.|  
|4871|775|Baixo|Os serviços de certificado receberam uma solicitação para publicar a lista de certificados revogados (CRL).|  
|4872|776|Baixo|Os serviços de certificado publicaram a lista de certificados revogados (CRL).|  
|4873|777|Baixo|Uma extensão de solicitação de certificado foi alterada.|  
|4874|778|Baixo|Um ou mais atributos de solicitação de certificado é alterado.|  
|4875|779|Baixo|Os serviços de certificado receberam uma solicitação de desligá-lo.|  
|4876|780|Baixo|Backup de serviços de certificado iniciado.|  
|4877|781|Baixo|Backup de serviços de certificado concluído.|  
|4878|782|Baixo|Restauração de serviços de certificado iniciada.|  
|4879|783|Baixo|Concluída a restauração dos serviços de certificado.|  
|4880|784|Baixo|Os serviços de certificado iniciados.|  
|4881|785|Baixo|Serviços de certificado parados.|  
|4883|787|Baixo|Os serviços de certificado recuperaram uma chave arquivada.|  
|4884|788|Baixo|Os serviços de certificado importaram um certificado para o banco de dados.|  
|4886|790|Baixo|Serviços de certificado receberam uma solicitação de certificado.|  
|4887|791|Baixo|Os serviços de certificado aprovaram uma solicitação de certificado e emitiram um certificado.|  
|4888|792|Baixo|Serviços de certificado negaram uma solicitação de certificado.|  
|4889|793|Baixo|Serviços de certificado definiram o status de uma solicitação de certificado como pendente.|  
|4891|795|Baixo|Uma entrada de configuração foi alterada nos serviços de certificado.|  
|4893|797|Baixo|Os serviços de certificado arquivaram uma chave.|  
|4894|798|Baixo|Os serviços de certificado importaram e arquivaram uma chave.|  
|4895|799|Baixo|Serviços de certificado publicaram o certificado da CA nos serviços de domínio do Active Directory.|  
|4898|802|Baixo|Os serviços de certificado carregado um modelo.|  
|4902|N/D|Baixo|A tabela de política de auditoria por usuário foi criada.|  
|4904|N/D|Baixo|Foi feita uma tentativa de registrar uma origem de evento de segurança.|  
|4905|N/D|Baixo|Foi feita uma tentativa de cancelar o registro de uma origem de evento de segurança.|  
|4909|N/D|Baixo|As configurações de política local do TBS foram alteradas.|  
|4910|N/D|Baixo|As configurações de política de grupo do TBS foram alteradas.|  
|4928|N/D|Baixo|Um contexto de nomenclatura de origem de réplica do Active Directory estabelecido.|  
|4929|N/D|Baixo|Um contexto de nomenclatura de origem de réplica do Active Directory foi removido.|  
|4930|N/D|Baixo|Um contexto de nomenclatura de origem de réplica do Active Directory foi modificado.|  
|4931|N/D|Baixo|Um contexto de nomenclatura do destino de réplica do Active Directory foi modificado.|  
|4932|N/D|Baixo|Sincronização de uma réplica de um contexto de nomenclatura do Active Directory foi iniciada.|  
|4933|N/D|Baixo|Sincronização de uma réplica de um contexto de nomenclatura do Active Directory terminou.|  
|4934|N/D|Baixo|Atributos de um objeto do Active Directory foram replicados.|  
|4935|N/D|Baixo|Falha na replicação inicia.|  
|4936|N/D|Baixo|Falha na replicação termina.|  
|4937|N/D|Baixo|Um objeto remanescente foi removido de uma réplica.|  
|4944|N/D|Baixo|A seguinte política estava ativa quando o Firewall do Windows iniciou.|  
|4945|N/D|Baixo|Uma regra foi listada quando o Firewall do Windows iniciou.|  
|4946|N/D|Baixo|Uma alteração foi feita a lista de exceções do Firewall do Windows. Uma regra foi adicionada.|  
|4947|N/D|Baixo|Uma alteração foi feita a lista de exceções do Firewall do Windows. Uma regra foi modificada.|  
|4948|N/D|Baixo|Uma alteração foi feita a lista de exceções do Firewall do Windows. Uma regra foi excluída.|  
|4949|N/D|Baixo|Configurações do Firewall do Windows foram restauradas para os valores padrão.|  
|4950|N/D|Baixo|Uma configuração do Firewall do Windows foi alterada.|  
|4951|N/D|Baixo|Uma regra foi ignorada porque o número de versão principal não foi reconhecido pelo Firewall do Windows.|  
|4952|N/D|Baixo|Partes de uma regra foram ignoradas por seu número de versão secundária não foi reconhecido pelo Firewall do Windows. As demais partes da regra serão impostas.|  
|4953|N/D|Baixo|Uma regra foi ignorada pelo Firewall do Windows porque ele não foi possível analisar a regra.|  
|4954|N/D|Baixo|Configurações de política de grupo de Firewall do Windows foram alteradas. As novas configurações foram aplicadas.|  
|4956|N/D|Baixo|O Firewall do Windows alterou o perfil ativo.|  
|4957|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra:|  
|4958|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra porque a regra se referia a itens que não estão configurados neste computador:|  
|4979|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e modo estendido.|  
|4980|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e modo estendido.|  
|4981|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e modo estendido.|  
|4982|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e modo estendido.|  
|4985|N/D|Baixo|O estado de uma transação mudou.|  
|5024|N/D|Baixo|O serviço do Firewall do Windows iniciado com sucesso.|  
|5025|N/D|Baixo|O serviço do Firewall do Windows interrompido.|  
|5031|N/D|Baixo|O serviço do Firewall do Windows bloqueou um aplicativo aceitar as conexões recebidas na rede.|  
|5032|N/D|Baixo|O Firewall do Windows não pôde notificar o usuário que bloqueou um aplicativo para aceitar as conexões recebidas na rede.|  
|5033|N/D|Baixo|O Driver do Firewall do Windows iniciado com sucesso.|  
|5034|N/D|Baixo|O Driver do Firewall do Windows interrompido.|  
|5039|N/D|Baixo|Uma chave do registro foi virtualizada.|  
|5040|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi adicionado.|  
|5041|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi modificado.|  
|5042|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi excluído.|  
|5043|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de Conexão foi adicionada.|  
|5044|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de Conexão foi modificada.|  
|5045|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de Conexão foi excluída.|  
|5046|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi adicionado.|  
|5047|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi modificado.|  
|5048|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi excluído.|  
|5050|N/D|Baixo|Uma tentativa de forma programática desabilitar o Firewall do Windows usando uma chamada para InetFwProfile.FirewallEnabled(False)|  
|5051|N/D|Baixo|Um arquivo foi virtualizado.|  
|5056|N/D|Baixo|Um autoteste criptográfico foi executado.|  
|5057|N/D|Baixo|Falha de uma operação criptográfica primitiva.|  
|5058|N/D|Baixo|Operação de arquivo de chave.|  
|5059|N/D|Baixo|Operação de migração de chave.|  
|5060|N/D|Baixo|Falha na operação de verificação.|  
|5061|N/D|Baixo|Operação criptográfica.|  
|5062|N/D|Baixo|Um modo kernel autoteste criptográfico foi executado.|  
|5063|N/D|Baixo|Tentativa de operação do provedor criptográfico.|  
|5064|N/D|Baixo|Tentativa de operação do provedor criptográfico.|  
|5065|N/D|Baixo|Tentativa de modificação de contexto criptográfico.|  
|5066|N/D|Baixo|Tentativa de operação da função criptográfica.|  
|5067|N/D|Baixo|Tentativa de modificação da função criptográfica.|  
|5068|N/D|Baixo|Tentativa de uma operação de provedor da função criptográfica.|  
|5069|N/D|Baixo|Tentativa de uma operação de propriedade da função criptográfica.|  
|5070|N/D|Baixo|Tentativa de modificação de propriedade da função criptográfica.|  
|5125|N/D|Baixo|Uma solicitação foi enviada para o serviço Respondente OCSP|  
|5126|N/D|Baixo|Certificado de assinatura foi atualizado automaticamente pelo serviço Respondente OCSP|  
|5127|N/D|Baixo|O provedor de revogação OCSP atualizado com êxito as informações de revogação|  
|5136|566|Baixo|Um objeto de serviço de diretório foi modificado.|  
|5137|566|Baixo|Um objeto de serviço de diretório foi criado.|  
|5138|N/D|Baixo|Um objeto de serviço de diretório teve sua exclusão cancelado.|  
|5139|N/D|Baixo|Um objeto de serviço de diretório foi movido.|  
|5140|N/D|Baixo|Um objeto de compartilhamento de rede foi acessado.|  
|5141|N/D|Baixo|Um objeto de serviço de diretório foi excluído.|  
|5152|N/D|Baixo|A plataforma para filtros do Windows bloqueou um pacote.|  
|5153|N/D|Baixo|Um filtro mais restritivo da plataforma de filtragem do Windows bloqueou um pacote.|  
|5154|N/D|Baixo|A plataforma para filtros do Windows permitiu que um aplicativo ou serviço para escutar conexões de entrada em uma porta.|  
|5155|N/D|Baixo|A plataforma para filtros do Windows bloqueou um aplicativo ou serviço ouvisse uma porta conexões de entrada.|  
|5156|N/D|Baixo|A plataforma para filtros do Windows permitiu uma conexão.|  
|5157|N/D|Baixo|A plataforma para filtros do Windows bloqueou uma conexão.|  
|5158|N/D|Baixo|A plataforma para filtros do Windows permitiu uma associação a uma porta local.|  
|5159|N/D|Baixo|A plataforma para filtros do Windows bloqueou uma ligação a uma porta local.|  
|5378|N/D|Baixo|A delegação das credenciais solicitadas não foi permitida pela política.|  
|5440|N/D|Baixo|O texto explicativo seguir estava presente quando o mecanismo Windows Filtering Platform base de dados de filtragem iniciou.|  
|5441|N/D|Baixo|O filtro a seguir estava presente quando o mecanismo Windows Filtering Platform base de dados de filtragem iniciou.|  
|5442|N/D|Baixo|O seguinte provedor estava presente quando o mecanismo Windows Filtering Platform base de dados de filtragem iniciou.|  
|5443|N/D|Baixo|O seguinte contexto de provedor estava presente quando o mecanismo Windows Filtering Platform base de dados de filtragem iniciou.|  
|5444|N/D|Baixo|A seguinte subcamada estava presente quando o mecanismo Windows Filtering Platform base de dados de filtragem iniciou.|  
|5446|N/D|Baixo|Um texto explicativo de plataforma de filtragem do Windows foi alterado.|  
|5447|N/D|Baixo|Um filtro de plataforma de filtragem do Windows foi alterado.|  
|5448|N/D|Baixo|Um provedor de plataforma de filtragem do Windows foi alterado.|  
|5449|N/D|Baixo|Um contexto de provedor de plataforma de filtragem do Windows foi alterado.|  
|5450|N/D|Baixo|Foi alterada uma subcamada da Windows Filtering Platform.|  
|5451|N/D|Baixo|Uma associação de segurança de modo rápido IPsec estabelecida.|  
|5452|N/D|Baixo|Uma associação de segurança de modo rápido IPsec finalizada.|  
|5456|N/D|Baixo|O PAStore Engine aplicou a diretiva IPsec de armazenamento do Active Directory no computador.|  
|5457|N/D|Baixo|O PAStore Engine não conseguiu aplicar na máquina a diretiva IPsec de armazenamento do Active Directory.|  
|5458|N/D|Baixo|O PAStore Engine aplicou cópia localmente em cache da diretiva IPsec de armazenamento do Active Directory no computador.|  
|5459|N/D|Baixo|O PAStore Engine não pôde aplicar cópia localmente em cache da diretiva IPsec de armazenamento do Active Directory no computador.|  
|5460|N/D|Baixo|O PAStore Engine aplicou a diretiva IPsec de armazenamento de registro local no computador.|  
|5461|N/D|Baixo|O PAStore Engine não conseguiu aplicar na máquina a diretiva IPsec de armazenamento de registro local.|  
|5462|N/D|Baixo|O PAStore Engine não pôde aplicar algumas regras da diretiva IPsec ativa no computador. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|  
|5463|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPsec ativa e não detectou alterações.|  
|5464|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPsec ativa, detectou alterações e aplicou aos Serviços IPsec.|  
|5465|N/D|Baixo|O PAStore Engine recebeu um controle para a recarga forçada da diretiva IPsec e processou o controle com êxito.|  
|5466|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPsec do Active Directory, determinada que o Active Directory não pode ser acessado e usará a cópia em cache da diretiva IPsec do Active Directory, em vez disso. Todas as alterações feitas na diretiva IPsec do Active Directory desde a última sondagem não puderam ser aplicada.|  
|5467|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPsec do Active Directory, determinada que o Active Directory pode ser atingido e não localizou alterações na diretiva. A cópia em cache da diretiva IPsec do Active Directory não está sendo usada.|  
|5468|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPsec do Active Directory, determinada que o Active Directory pode ser acessado, localizou alterações na diretiva e aplicou tais alterações. A cópia em cache da diretiva IPsec do Active Directory não está sendo usada.|  
|5471|N/D|Baixo|O PAStore Engine carregou na máquina a diretiva IPsec de armazenamento local.|  
|5472|N/D|Baixo|O PAStore Engine não pôde carregar na máquina a diretiva IPsec de armazenamento local.|  
|5473|N/D|Baixo|O PAStore Engine carregou na máquina a diretiva IPsec de armazenamento de diretório.|  
|5474|N/D|Baixo|O PAStore Engine não pôde carregar na máquina a diretiva IPsec de armazenamento de diretório.|  
|5477|N/D|Baixo|O PAStore Engine não pôde adicionar filtro de modo rápido.|  
|5479|N/D|Baixo|Os Serviços IPsec foram desligados com êxito. O desligamento dos Serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança.|  
|5632|N/D|Baixo|Foi feita uma solicitação para autenticar uma rede sem fio.|  
|5633|N/D|Baixo|Foi feita uma solicitação para autenticar uma rede com fio.|  
|5712|N/D|Baixo|Tentativa de um procedimento remoto (RPC).|  
|5888|N/D|Baixo|Um objeto no catálogo COM+ foi modificado.|  
|5889|N/D|Baixo|Um objeto foi excluído do catálogo COM+.|  
|5890|N/D|Baixo|Um objeto foi adicionado ao catálogo COM+.|  
|6008|N/D|Baixo|O desligamento anterior do sistema foi inesperado|  
|6144|N/D|Baixo|Política de segurança nos objetos de política de grupo foi aplicada com êxito.|  
|6272|N/D|Baixo|Servidor de política de rede concedeu acesso a um usuário.|  
|N/D|561|Baixo|Um identificador de um objeto foi solicitado.|  
|N/D|563|Baixo|Objeto aberto para exclusão|  
|N/D|625|Baixo|Tipo de conta de usuário mudado|  
|N/D|613|Baixo|Agente de política IPsec iniciado|  
|N/D|614|Baixo|Agente de política IPsec desabilitado|  
|N/D|615|Baixo|Agente de política IPsec|  
|N/D|616|Baixo|Agente de política IPsec encontrou uma falha grave potencial|  
|24577|N/D|Baixo|Criptografia do volume iniciado|  
|24578|N/D|Baixo|Criptografia do volume parado|  
|24579|N/D|Baixo|Criptografia do volume concluída|  
|24580|N/D|Baixo|Descriptografia do volume iniciado|  
|24581|N/D|Baixo|Descriptografia do volume parado|  
|24582|N/D|Baixo|Descriptografia do volume concluída|  
|24583|N/D|Baixo|Thread de trabalho de conversão de volume iniciado|  
|24584|N/D|Baixo|Thread de trabalho de conversão de volume interrompido temporariamente|  
|24588|N/D|Baixo|A operação de conversão no volume %2 encontrou um erro de setor inválido. Valide os dados neste volume|  
|24595|N/D|Baixo|Volume %2 contém clusters inválidos. Esses clusters serão ignorados durante a conversão.|  
|24621|N/D|Baixo|Verificação de estado inicial: Implantando transação de conversão de volume em %2.|  
|5049|N/D|Baixo|Uma associação de segurança IPsec foi excluída.|  
|5478|N/D|Baixo|Serviços IPsec foram iniciados com êxito.|  
  
> [!NOTE]  
> Consulte [artigo Microsoft Support 947226](https://support.microsoft.com/kb/947226) para listas de muitos IDs de evento de segurança e seus significados.  
>   
> Executar **wevtutil gp Microsoft-Windows--a auditoria de segurança /ge /gm:true** para obter uma lista bem detalhada de segurança todos os IDs de evento  
  
Para obter mais informações sobre IDs de evento de segurança do Windows e seus significados, consulte os artigos do Microsoft Support [descrição de eventos de segurança no Windows Vista e no Windows Server 2008](https://support.microsoft.com/kb/947226) e [descrição de eventos de segurança no Windows 7 e no Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Você também pode baixar [eventos de auditoria de segurança para o 7 do Windows e Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e [Windows 8 e detalhes do evento de segurança do Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que fornecem informações de eventos para os sistemas operacionais referenciados no formato planilha detalhadas.  
  


