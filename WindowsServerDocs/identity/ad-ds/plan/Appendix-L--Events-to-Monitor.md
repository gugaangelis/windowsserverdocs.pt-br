---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Apêndice L - eventos a serem monitorados
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b937debf2d9156c50f3c0ae51fdab8bd2a2bf2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863477"
---
# <a name="appendix-l-events-to-monitor"></a>Apêndice l: Eventos a Monitorar

>Aplica-se a: Windows Server

A tabela a seguir lista os eventos que devem ser monitoradas em seu ambiente, de acordo com as recomendações fornecidas [monitoramento do Active Directory para sinais de comprometimento](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Na tabela a seguir, a coluna "ID do evento atual Windows" lista a ID do evento conforme ele é implementado em versões do Windows e Windows Server que estão atualmente com suporte base.  
  
A coluna de "ID do evento Windows herdados" lista a ID de evento correspondente em versões herdadas do Windows como computadores cliente que executam o Windows XP ou anterior e servidores que executam o Windows Server 2003 ou anterior. O "criticidade potencial" coluna identifica se o evento deve ser considerado de baixa, média ou alta prioridade na detecção de ataques e a coluna "Resumo do evento" fornece uma breve descrição do evento.  
  
Uma criticidade potencial alta significa que uma ocorrência do evento deve ser investigada. Criticidade potencial média ou baixa significa que esses eventos só devem ser investigados se ocorrerem inesperadamente ou em números que excedam de modo significativo a linha de base esperada em um período de tempo medido. Todas as organizações devem testar essas recomendações em seus ambientes antes de criar alertas que exijam respostas investigativas obrigatórias. Cada ambiente é diferente e alguns dos eventos classificados com uma criticidade potencial alta podem ocorrer devido a outros eventos inofensivos.  
  
|||||  
|-|-|-|-|  
|**ID de evento do Windows atual**|**ID de evento do Windows herdado**|**Criticidade potencial**|**Resumo de eventos**|  
|4618|N/D|Alto|Ocorreu um padrão de evento de segurança monitorado.|  
|4649|N/D|Alto|Um ataque de repetição foi detectado. Pode ser um falso positivo inofensivo devido a erro de configuração incorreta.|  
|4719|612|Alto|A política de auditoria do sistema foi alterada.|  
|4765|N/D|Alto|Histórico SID adicionado a uma conta.|  
|4766|N/D|Alto|Falha na tentativa de adicionar histórico SID a uma conta.|  
|4794|N/D|Alto|Tentativa de definir a senha do administrador do modo de restauração de serviços de diretório.|  
|4897|801|Alto|Separação de Função Habilitada:|  
|4964|N/D|Alto|Grupos especiais atribuídos a um novo logon.|  
|5124|N/D|Alto|Uma configuração de segurança foi atualizada no serviço de Respondente OCSP|  
|N/D|550|Médio a alto|Ataque possível negação de serviço (DoS)|  
|1102|517|Médio a alto|O log de auditoria foi limpo|  
|4621|N/D|Médio|O administrador recuperou o sistema de CrashOnAuditFail. Os usuários que não sejam administradores não poderão efetuar o logon. Alguma atividade para verificação pode não ter sido registrada.|  
|4675|N/D|Médio|SIDs filtradas.|  
|4692|N/D|Médio|Tentativa de backup da chave mestre de proteção dos dados.|  
|4693|N/D|Médio|Tentativa de recuperação da chave mestre de proteção dos dados.|  
|4706|610|Médio|Foi criada uma nova confiança para um domínio.|  
|4713|617|Médio|A política Kerberos foi alterada.|  
|4714|618|Médio|A diretiva de recuperação de dados criptografados foi alterada.|  
|4715|N/D|Médio|A política de auditoria (SACL) de um objeto foi alterada.|  
|4716|620|Médio|As informações de um domínio confiável foram modificadas.|  
|4724|628|Médio|Foi feita uma tentativa de redefinir a senha de uma conta.|  
|4727|631|Médio|Foi criado um grupo global com a segurança ativada.|  
|4735|639|Médio|Foi alterado um grupo local com a segurança ativada.|  
|4737|641|Médio|Foi alterado um grupo global com a segurança ativada.|  
|4739|643|Médio|Política de domínio alterada.|  
|4754|658|Médio|Foi criado um grupo universal com a segurança ativada.|  
|4755|659|Médio|Foi alterado um grupo universal com a segurança ativada.|  
|4764|667|Médio|Um grupo de segurança desabilitada foi excluído|  
|4764|668|Médio|Um tipo do grupo foi alterado.|  
|4780|684|Médio|O ACL foi definido em contas que são membros de grupos de administradores.|  
|4816|N/D|Médio|O RPC detectou uma violação de integridade ao descriptografar uma mensagem recebida.|  
|4865|N/D|Médio|Adicionada entrada de informações de floresta confiável.|  
|4866|N/D|Médio|Removida entrada de informações de floresta confiável.|  
|4867|N/D|Médio|Modificada entrada de informações de floresta confiável.|  
|4868|772|Médio|O gerenciador de certificados negou uma solicitação de certificado pendente.|  
|4870|774|Médio|Serviços de Certificados revogaram um certificado.|  
|4882|786|Médio|As permissões de segurança para Serviços de Certificados mudaram.|  
|4885|789|Médio|O filtro de auditoria dos Serviços de Certificados mudou.|  
|4890|794|Médio|As configurações do gerenciador de certificados para Serviços de Certificados mudaram.|  
|4892|796|Médio|Uma propriedade dos Serviços de Certificados mudou.|  
|4896|800|Médio|Uma ou mais linhas foram excluídas do banco de dados do certificado.|  
|4906|N/D|Médio|O valor CrashOnAuditFail foi alterado.|  
|4907|N/D|Médio|As configurações de auditoria do objeto foram alteradas.|  
|4908|N/D|Médio|Tabela de logon de grupos especiais modificada.|  
|4912|807|Médio|A política de auditoria por usuário foi alterada.|  
|4960|N/D|Médio|O IPsec ignorou um pacote recebido que não passou pela verificação de integridade. Se este problema persistir, pode indicar um erro na rede ou que os pacotes estão sendo modificados durante a transferência para este computador. Verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Este erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec.|  
|4961|N/D|Médio|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. Se este problema persistir, pode indicar um ataque por repetição contra este computador.|  
|4962|N/D|Médio|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. O pacote recebido tinha um número de sequência muito baixo para garantir que não era uma repetição.|  
|4963|N/D|Médio|O IPsec ignorou um pacote recebido de texto simples que deveria ter protegido. Geralmente, isso ocorre devido a um computador remoto que altera a sua diretiva do IPsec sem informar este computador. Também pode ser uma tentativa de falsificação da transmissão.|  
|4965|N/D|Médio|O IPsec removeu um pacote de um computador remoto com um índice de parâmetro de segurança (SPI) incorreto. Geralmente, esse é causado pelo mau funcionamento de um hardware que está corrompendo os pacotes. Se esses erros persistirem, verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Este erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec. Neste caso, se a conectividade não estiver impedida, esses eventos podem ser ignorados.|  
|4976|N/D|Médio|Durante a negociação do modo principal, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4977|N/D|Médio|Durante a negociação do modo rápido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4978|N/D|Médio|Durante a negociação do modo estendido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4983|N/D|Médio|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|4984|N/D|Médio|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|5027|N/D|Médio|O serviço de Firewall do Windows não pôde recuperar a política de segurança do armazenamento local. O serviço continuará executando a diretiva atual.|  
|5028|N/D|Médio|Serviço do Firewall do Windows foi incapaz de analisar a nova diretiva de segurança. O serviço continuará com a diretiva atualmente executada.|  
|5029|N/D|Médio|O serviço de Firewall do Windows não pôde inicializar o driver. O serviço continuará executando a diretiva atual.|  
|5030|N/D|Médio|Não foi possível iniciar o serviço do Firewall do Windows.|  
|5035|N/D|Médio|Driver do Firewall do Windows falhou ao inicializar.|  
|5037|N/D|Médio|Driver do Firewall do Windows detectou um erro crítico de tempo de execução. Terminação.|  
|5038|N/D|Médio|A integridade do código determinou que o hash de imagem de um arquivo não é válido. O arquivo pode estar corrompido devido a uma modificação não-autorizada, ou o hash inválido pode indicar um erro em potencial do dispositivo de disco.|  
|5120|N/D|Médio|Serviço de Respondente OCSP foi iniciado|  
|5121|N/D|Médio|Serviço de Respondente OCSP interrompido|  
|5122|N/D|Médio|Uma entrada de configuração alterada no serviço de Respondente OCSP|  
|5123|N/D|Médio|Uma entrada de configuração alterada no serviço de Respondente OCSP|  
|5376|N/D|Médio|Backup efetuado das credenciais do Gerenciador de Credenciais.|  
|5377|N/D|Médio|As credenciais do Gerenciador de Credenciais foram restauradas a partir de um backup.|  
|5453|N/D|Médio|Negociação IPsec com computador local falhou porque o serviço de Módulos de Criação de Chaves IKE e AuthIP IPsec (IKEEXT) não foi iniciado.|  
|5480|N/D|Médio|Os serviços IPSec não puderam obter a lista completa de interfaces de rede na máquina. Esta pode ser uma situação de risco para a segurança da máquina, porque algumas interfaces de rede talvez não obtenham a proteção desejada com os filtros IPSec aplicados. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|5483|N/D|Médio|Os serviços IPSec não puderam inicializar o servidor RPC. Os serviços IPSec não puderam ser iniciados.|  
|5484|N/D|Médio|Falha grave com os serviços IPSec. Eles foram desativados. O desligamento dos serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança em potencial.|  
|5485|N/D|Médio|Os serviços IPSec não puderam processar alguns filtros IPSec em um evento plug and play para interfaces de rede. Esta pode ser uma situação de risco para a segurança da máquina, porque algumas interfaces de rede talvez não obtenham a proteção desejada com os filtros IPSec aplicados. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|6145|N/D|Médio|Um ou mais erros ocorreram durante o processamento de diretiva de segurança nos objetos de diretiva de grupo.|  
|6273|N/D|Médio|O Servidor de Políticas de Rede negou acesso a um usuário.|  
|6274|N/D|Médio|O Servidor de Políticas de Rede descartou a solicitação de um usuário.|  
|6275|N/D|Médio|O Servidor de Políticas de Rede descartou a solicitação de contabilização de um usuário.|  
|6276|N/D|Médio|O Servidor de Política de Rede colocou um usuário em quarentena.|  
|6277|N/D|Médio|O Servidor de Política de Rede concedeu acesso a um usuário, mas colocou-o em experiência porque o host não atendeu à política de integridade.|  
|6278|N/D|Médio|O Servidor de Política de Rede concedeu acesso total a um usuário porque o host atendeu a política de integridade definida.|  
|6279|N/D|Médio|O Servidor de Política de Rede bloqueou a conta do usuário devido a constantes falhas das tentativas de atualização.|  
|6280|N/D|Médio|O Servidor de Política de Rede desbloqueou a conta do usuário.|  
|-|640|Médio|Banco de dados de conta geral alterado|  
|-|619|Médio|Qualidade de serviço política alterada|  
|24586|N/D|Médio|Ocorreu um erro conversão de volume|  
|24592|N/D|Médio|Falha ao tentar reiniciar automaticamente a conversão no volume %2.|  
|24593|N/D|Médio|Gravação de metadados: Volume %2 retorno de erros durante a tentativa de modificar metadados. Se a falha persistir, descriptografar o volume|  
|24594|N/D|Médio|Recriação de metadados: Uma tentativa de gravar uma cópia dos metadados no volume %2 falha e pode ser exibidas como corrupção de disco. Se a falha persistir, descriptografe o volume.|  
|4608|512|Baixo|Windows está iniciando.|  
|4609|513|Baixo|O Windows está sendo desligado.|  
|4610|514|Baixo|Um pacote de autenticação foi carregado pela autoridade de segurança local.|  
|4611|515|Baixo|Um processo de logon confiável foi registrado com a autoridade de segurança local.|  
|4612|516|Baixo|Os recursos internos alocados para a fila de mensagens de auditoria se esgotaram, provocando a perda de algumas auditorias.|  
|4614|518|Baixo|Um pacote de notificação foi carregado pelo Gerente de Contas de Segurança.|  
|4615|519|Baixo|Uso inválido da porta LPC.|  
|4616|520|Baixo|Hora do sistema alterada.|  
|4622|N/D|Baixo|Um pacote de segurança foi carregado pela autoridade de segurança local.|  
|4624|528,540|Baixo|O logon de uma conta foi efetuado com êxito.|  
|4625|529-537,539|Baixo|Falha no logon de uma conta.|  
|4634|538|Baixo|Foi efetuado o logoff de uma conta.|  
|4646|N/D|Baixo|Modo de prevenção IKE DoS iniciado.|  
|4647|551|Baixo|O usuário iniciou logoff.|  
|4648|552|Baixo|Tentativa de logon com uso de credenciais explícitas.|  
|4650|N/D|Baixo|Foi estabelecida uma associação de segurança do modo IPsec principal. O modo estendido não foi ativado. A autenticação do certificado não foi usada.|  
|4651|N/D|Baixo|Foi estabelecida uma associação de segurança do modo IPsec principal. O modo estendido não foi ativado. Um certificado foi usado na autenticação.|  
|4652|N/D|Baixo|Falha na negociação do modo IPsec principal.|  
|4653|N/D|Baixo|Falha na negociação do modo IPsec principal.|  
|4654|N/D|Baixo|Falha de uma negociação IPsec de modo rápido.|  
|4655|N/D|Baixo|Uma associação de segurança do modo IPsec principal foi terminada.|  
|4656|560|Baixo|Foi solicitado o identificador de um objeto.|  
|4657|567|Baixo|Foi modificado um valor do registro.|  
|4658|562|Baixo|O identificador de um objeto foi fechado.|  
|4659|N/D|Baixo|O identificador de um objeto foi solicitado, com a intenção de excluir.|  
|4660|564|Baixo|Um objeto foi excluído.|  
|4661|565|Baixo|Foi solicitado o identificador de um objeto.|  
|4662|566|Baixo|Foi realizada uma operação em um objeto.|  
|4663|567|Baixo|Houve uma tentativa de acessar um objeto.|  
|4664|N/D|Baixo|Houve a tentativa de criar um vínculo físico.|  
|4665|N/D|Baixo|Houve a tentativa de criar um contexto de cliente de aplicativos.|  
|4666|N/D|Baixo|Um aplicativo tentou realizar uma operação:|  
|4667|N/D|Baixo|O contexto do cliente do aplicativo foi excluído.|  
|4668|N/D|Baixo|Um aplicativo foi inicializado.|  
|4670|N/D|Baixo|As permissões de um objeto foram alteradas.|  
|4671|N/D|Baixo|Um aplicativo tentou acessar um ordinal bloqueado através do TBS.|  
|4672|576|Baixo|Privilégios especiais atribuídos a um novo logon.|  
|4673|577|Baixo|Um serviço de privilégios foi chamado.|  
|4674|578|Baixo|Tentativa de operação em um objeto privilegiado.|  
|4688|592|Baixo|Foi criado um novo processo.|  
|4689|593|Baixo|O processo foi encerrado.|  
|4690|594|Baixo|Foi feita a tentativa de duplicar o identificador de um objeto.|  
|4691|595|Baixo|Foi solicitado o acesso indireto a um objeto.|  
|4694|N/D|Baixo|Tentativa de proteção contra verificação dos dados protegidos.|  
|4695|N/D|Baixo|Tentativa de desproteção contra verificação dos dados protegidos.|  
|4696|600|Baixo|Um token primário foi atribuído a processar.|  
|4697|601|Baixo|Tentativa de instalar um serviço|  
|4698|602|Baixo|Uma tarefa agendada foi criada.|  
|4699|602|Baixo|Uma tarefa agendada foi excluída.|  
|4700|602|Baixo|Uma tarefa agendada estava habilitada.|  
|4701|602|Baixo|Uma tarefa agendada estava desabilitada.|  
|4702|602|Baixo|Uma tarefa agendada foi atualizada.|  
|4704|608|Baixo|Um direito de usuário foi atribuído.|  
|4705|609|Baixo|Um direito de usuário foi removido.|  
|4707|611|Baixo|Foi removida a confiança de um domínio.|  
|4709|N/D|Baixo|Os serviços IPsec foram iniciados.|  
|4710|N/D|Baixo|Os serviços IPsec foram desabilitados.|  
|4711|N/D|Baixo|Pode conter qualquer um dos seguintes: PAStore Engine aplicou a cópia em cache local de armazenamento do Active Directory a diretiva IPsec no computador. Mecanismo PAStore aplicou a diretiva IPsec de armazenamento do Active Directory no computador. Mecanismo PAStore aplicou a diretiva IPsec de armazenamento de registro local no computador. Mecanismo PAStore Falha ao aplicar a cópia armazenada em cache localmente da diretiva IPsec de armazenamento do Active Directory no computador. Mecanismo PAStore Falha ao aplicar a diretiva IPsec de armazenamento do Active Directory no computador. Mecanismo PAStore Falha ao aplicar a política de IPsec do armazenamento de registro local no computador. Mecanismo PAStore Falha ao aplicar algumas regras da diretiva IPsec ativa no computador. Mecanismo PAStore Falha ao carregar o armazenamento de diretório diretiva IPsec no computador. PAStore Engine carregou a diretiva de IPsec no computador de armazenamento de diretório. PAStore Engine Falha ao carregar a política de IPsec no computador do armazenamento local. PAStore Engine carregou a diretiva IPsec no computador do armazenamento local. Mecanismo PAStore sondado para alterações na diretiva de IPsec ativa e não detectou alterações.|  
|4712|N/D|Baixo|Os serviços IPSec encontraram uma falha possivelmente séria.|  
|4717|621|Baixo|O acesso de segurança do sistema foi concedido a uma conta.|  
|4718|622|Baixo|O acesso de segurança do sistema foi removido de uma conta.|  
|4720|624|Baixo|Conta de usuário criada.|  
|4722|626|Baixo|Conta de usuário ativada.|  
|4723|Etapas de resolução para o seguinte evento ID 627|Baixo|Foi feita uma tentativa de alterar a senha de uma conta.|  
|4725|629|Baixo|Conta de usuário desabilitada.|  
|4726|630|Baixo|Conta de usuário excluída.|  
|4728|632|Baixo|Um membro foi adicionado a um grupo global com segurança ativada.|  
|4729|633|Baixo|Um membro foi removido de um grupo global com segurança ativada.|  
|4730|634|Baixo|Foi excluído um grupo global com a segurança ativada.|  
|4731|635|Baixo|Foi criado um grupo local com a segurança ativada.|  
|4732|636|Baixo|Um membro foi adicionado a um grupo local com segurança ativada.|  
|4733|637|Baixo|Um membro foi removido de um grupo local com segurança ativada.|  
|4734|638|Baixo|Foi excluído um grupo local com a segurança ativada.|  
|4738|642|Baixo|Conta de usuário alterada.|  
|4740|644|Baixo|Conta de usuário bloqueada.|  
|4741|645|Baixo|Conta de computador alterada.|  
|4742|646|Baixo|Conta de computador alterada.|  
|4743|647|Baixo|Conta de computador excluída.|  
|4744|648|Baixo|Foi criado um grupo local com a segurança desativada.|  
|4745|649|Baixo|Foi alterado um grupo local com a segurança desativada.|  
|4746|650|Baixo|Um membro foi adicionado a um grupo local com segurança desativada.|  
|4747|651|Baixo|Um membro foi removido de um grupo local com segurança desativada.|  
|4748|652|Baixo|Foi excluído um grupo local com a segurança desativada.|  
|4749|653|Baixo|Foi criado um grupo global com a segurança desativada.|  
|4750|654|Baixo|Foi alterado um grupo global com a segurança desativada.|  
|4751|655|Baixo|Um membro foi adicionado a um grupo global com segurança desativada.|  
|4752|656|Baixo|Um membro foi removido de um grupo global com segurança desativada.|  
|4753|657|Baixo|Foi excluído um grupo global com a segurança desativada.|  
|4756|660|Baixo|Um membro foi adicionado a um grupo universal com segurança ativada.|  
|4757|661|Baixo|Um membro foi removido de um grupo universal com segurança ativada.|  
|4758|662|Baixo|Foi excluído um grupo universal com a segurança ativada.|  
|4759|663|Baixo|Foi criado um grupo universal com a segurança desativada.|  
|4760|664|Baixo|Foi alterado um grupo universal com a segurança desativada.|  
|4761|665|Baixo|Um membro foi adicionado a um grupo universal com segurança desativada.|  
|4762|666|Baixo|Um membro foi removido de um grupo universal com segurança desativada.|  
|4767|671|Baixo|Conta de usuário desbloqueada.|  
|4768|672,676|Baixo|Permissão de autenticação Kerberos (TGT) solicitada.|  
|4769|673|Baixo|Tíquete de serviço Kerberos solicitado.|  
|4770|674|Baixo|Tíquete de serviço Kerberos renovado.|  
|4771|675|Baixo|Falha na pré-autenticação do Kerberos.|  
|4772|672|Baixo|Falha na solicitação de permissão de autenticação Kerberos.|  
|4774|678|Baixo|Conta mapeada para o logon.|  
|4775|679|Baixo|Impossível mapear conta para o logon.|  
|4776|680,681|Baixo|O controlador de domínio tentou validar as credenciais de uma conta.|  
|4777|N/D|Baixo|O controlador de domínio falhou ao validar as credenciais de uma conta.|  
|4778|682|Baixo|Uma sessão foi reconectada a uma Estação de janela.|  
|4779|683|Baixo|Uma sessão foi desconectada de uma Estação de janela.|  
|4781|685|Baixo|O nome de uma conta foi alterado:|  
|4782|N/D|Baixo|O hash de senha de uma conta foi acessado.|  
|4783|667|Baixo|Grupo de aplicativos básicos criado.|  
|4784|N/D|Baixo|Grupo de aplicativos básicos alterado.|  
|4785|689|Baixo|Um membro foi adicionado a um grupo de aplicativos básicos.|  
|4786|690|Baixo|Um membro foi removido de um grupo de aplicativos básicos.|  
|4787|691|Baixo|Não um membro foi adicionado a um grupo de aplicativo básico.|  
|4788|692|Baixo|Não um membro foi removido de um grupo de aplicativo básico.|  
|4789|693|Baixo|Grupo de aplicativos básicos excluído.|  
|4790|694|Baixo|Grupo de consultas LDAP criado.|  
|4793|N/D|Baixo|API de verificação de política de senha chamada.|  
|4800|N/D|Baixo|Estação de trabalho bloqueada.|  
|4801|N/D|Baixo|Estação de trabalho desbloqueada.|  
|4802|N/D|Baixo|Proteção de tela invocada.|  
|4803|N/D|Baixo|Proteção de tela descartada.|  
|4864|N/D|Baixo|Detectada colisão de namespaces.|  
|4869|773|Baixo|Serviços de Certificados receberam uma solicitação de certificado reenviada.|  
|4871|775|Baixo|Serviços de certificados receberam uma solicitação para publicar a CRL (lista de certificados revogados).|  
|4872|776|Baixo|Serviços de certificados publicaram a CRL (lista de certificados revogados).|  
|4873|777|Baixo|Uma extensão de solicitação de certificado foi alterada.|  
|4874|778|Baixo|Um ou mais atributos de solicitação de certificado mudou.|  
|4875|779|Baixo|Serviços de Certificados receberam uma solicitação para desligar.|  
|4876|780|Baixo|Backup de Serviços de Certificado iniciado.|  
|4877|781|Baixo|Backup de Serviços de Certificados concluído.|  
|4878|782|Baixo|Restauração de Serviços de Certificado iniciado.|  
|4879|783|Baixo|Restauração de Serviços de Certificados concluída.|  
|4880|784|Baixo|Serviços de Certificados iniciados.|  
|4881|785|Baixo|Serviços de Certificados interrompidos.|  
|4883|787|Baixo|Serviços de Certificados recuperaram uma chave arquivada.|  
|4884|788|Baixo|Serviços de Certificados importaram um certificado para o banco de dados.|  
|4886|790|Baixo|Serviços de certificados receberam uma solicitação de certificado.|  
|4887|791|Baixo|Serviços de certificados aprovaram uma solicitação de certificado e emitiram um certificado.|  
|4888|792|Baixo|Os Serviços de Certificados negaram uma solicitação de certificado.|  
|4889|793|Baixo|Os Serviços de Certificados definiram o status de solicitação de certificado para pendente.|  
|4891|795|Baixo|Uma entrada de configuração mudou nos Serviços de Certificados.|  
|4893|797|Baixo|Os Serviços de Certificados arquivaram uma chave.|  
|4894|798|Baixo|Os Serviços de Certificados importaram e arquivaram uma chave.|  
|4895|799|Baixo|Os 'Serviços de certificado' publicaram o certificado CA para os serviços de domínio do Active Directory.|  
|4898|802|Baixo|Os Serviços de Certificados carregaram um modelo.|  
|4902|N/D|Baixo|Criada tabela de políticas de auditoria por usuário.|  
|4904|N/D|Baixo|Tentativa de registrar uma origem de evento de segurança.|  
|4905|N/D|Baixo|Tentativa de remover o registro de uma origem de evento de segurança.|  
|4909|N/D|Baixo|As configurações da política local do TBS foram alteradas.|  
|4910|N/D|Baixo|As configurações de diretiva de grupo para o TB foram alteradas.|  
|4928|N/D|Baixo|Estabelecido o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4929|N/D|Baixo|Removido o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4930|N/D|Baixo|Modificado o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4931|N/D|Baixo|Modificado o contexto de nomenclatura do destino da réplica do Active Directory.|  
|4932|N/D|Baixo|Iniciada a sincronização de uma réplica de um contexto de nomenclatura do Active Directory.|  
|4933|N/D|Baixo|Terminada a sincronização de uma réplica de um contexto de nomenclatura do Active Directory.|  
|4934|N/D|Baixo|Os atributos de um objeto do Active Directory foram replicados.|  
|4935|N/D|Baixo|Falha na replicação inicia.|  
|4936|N/D|Baixo|Falha na replicação termina.|  
|4937|N/D|Baixo|Um objeto remanescente foi removido de uma réplica.|  
|4944|N/D|Baixo|A seguinte política estava ativa quando o Firewall do Windows iniciou.|  
|4945|N/D|Baixo|Uma regra foi listada quando o Firewall do Windows iniciou.|  
|4946|N/D|Baixo|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi adicionada.|  
|4947|N/D|Baixo|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi modificada.|  
|4948|N/D|Baixo|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi excluída.|  
|4949|N/D|Baixo|As configurações do Firewall do Windows foram restauradas para os valores padrão.|  
|4950|N/D|Baixo|Configuração do Firewall do Windows alterada.|  
|4951|N/D|Baixo|Uma regra foi ignorada porque o número de versão principal não foi reconhecido pelo Firewall do Windows.|  
|4952|N/D|Baixo|Partes de uma regra foram ignoradas por que seu número de versão secundária não foi reconhecido pelo Firewall do Windows. As demais partes da regra serão executadas.|  
|4953|N/D|Baixo|Uma regra foi ignorada pelo Firewall do Windows, porque ele não pôde analisá-la.|  
|4954|N/D|Baixo|As configurações de políticas de grupo do Firewall do Windows foram alteradas. Novas configurações foram aplicadas.|  
|4956|N/D|Baixo|O Firewall do Windows alterou o perfil ativo.|  
|4957|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra:|  
|4958|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra, porque ela se referia a itens que não estão configurados neste computador:|  
|4979|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4980|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4981|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4982|N/D|Baixo|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4985|N/D|Baixo|O estado de uma transação mudou.|  
|5024|N/D|Baixo|Serviço do Firewall do Windows iniciado com sucesso.|  
|5025|N/D|Baixo|Serviço do Firewall do Windows interrompido.|  
|5031|N/D|Baixo|Serviço do Firewall do Windows bloqueou um aplicativo para aceitar as conexões recebidas na rede.|  
|5032|N/D|Baixo|O Firewall do Windows não pôde notificar o usuário de que bloqueou um aplicativo para aceitar as conexões recebidas na rede.|  
|5033|N/D|Baixo|Driver do Firewall do Windows iniciado com sucesso.|  
|5034|N/D|Baixo|Driver do Firewall do Windows interrompido.|  
|5039|N/D|Baixo|Uma chave de registro foi virtualizada.|  
|5040|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi adicionado.|  
|5041|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi modificado.|  
|5042|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi excluído.|  
|5043|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi adicionada.|  
|5044|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi modificada.|  
|5045|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi excluída.|  
|5046|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um conjunto de criptografia foi adicionado.|  
|5047|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Criptografia foi modificado.|  
|5048|N/D|Baixo|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Criptografia foi excluído.|  
|5050|N/D|Baixo|Uma tentativa de desabilitar programaticamente o Firewall do Windows usando uma chamada para InetFwProfile.FirewallEnabled(False)|  
|5051|N/D|Baixo|Um arquivo foi virtualizado.|  
|5056|N/D|Baixo|Criptografia self o teste foi executado.|  
|5057|N/D|Baixo|Falha em uma operação criptográfica primitiva.|  
|5058|N/D|Baixo|Operação de arquivo de chave.|  
|5059|N/D|Baixo|Operação de migração de chave.|  
|5060|N/D|Baixo|Falha na operação de verificação.|  
|5061|N/D|Baixo|Operação de criptografia.|  
|5062|N/D|Baixo|Um modo de kernel criptográfico autoteste foi executado.|  
|5063|N/D|Baixo|Tentativa de operação do provedor criptográfico.|  
|5064|N/D|Baixo|Tentativa de operação do provedor criptográfico.|  
|5065|N/D|Baixo|Tentativa de modificação de contexto criptográfico.|  
|5066|N/D|Baixo|Tentativa de operação da função criptográfica.|  
|5067|N/D|Baixo|Tentativa de modificação da função criptográfica.|  
|5068|N/D|Baixo|Tentativa de operação de provedor da função criptográfica.|  
|5069|N/D|Baixo|Tentativa de operação de propriedade da função criptográfica.|  
|5070|N/D|Baixo|Tentativa de modificação de propriedade da função criptográfica.|  
|5125|N/D|Baixo|Uma solicitação foi enviada para o serviço de Respondente OCSP|  
|5126|N/D|Baixo|Certificado de assinatura foi atualizado automaticamente pelo serviço de Respondente OCSP|  
|5127|N/D|Baixo|O provedor de revogação OCSP atualizado com êxito as informações de revogação|  
|5136|566|Baixo|Um objeto de serviço de diretório foi modificado.|  
|5137|566|Baixo|Um objeto de serviço de diretório foi criado.|  
|5138|N/D|Baixo|Um objeto de serviço de diretório teve sua exclusão cancelada.|  
|5139|N/D|Baixo|Um objeto de serviço de diretório foi movido.|  
|5140|N/D|Baixo|Um objeto de compartilhamento de rede foi acessado.|  
|5141|N/D|Baixo|Um objeto de serviço de diretório foi excluído.|  
|5152|N/D|Baixo|A Plataforma para Filtros do Windows bloqueou um pacote.|  
|5153|N/D|Baixo|Um filtro mais restrito da Plataforma para Filtros do Windows bloqueou um pacote.|  
|5154|N/D|Baixo|A Plataforma para Filtros do Windows permitiu que um aplicativo ou serviço escutasse conexões de entrada em uma porta.|  
|5155|N/D|Baixo|A Plataforma para Filtros do Windows permitiu que um aplicativo ou serviço ouvisse uma porta, em busca de conexões recebidas.|  
|5156|N/D|Baixo|A Plataforma para Filtros do Windows permitiu uma conexão.|  
|5157|N/D|Baixo|A Plataforma para Filtros do Windows bloqueou uma conexão.|  
|5158|N/D|Baixo|A Plataforma para Filtros do Windows permitiu uma associação a uma porta local.|  
|5159|N/D|Baixo|A Plataforma para Filtros do Windows bloqueou uma ligação a uma porta local.|  
|5378|N/D|Baixo|A delegação das credenciais solicitadas não foi permitida pela política.|  
|5440|N/D|Baixo|O seguinte texto explicativo estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5441|N/D|Baixo|O filtro a seguir estava presente quando o Mecanismo de Filtragem Base da Plataforma de Filtragem do Windows foi iniciado.|  
|5442|N/D|Baixo|O seguinte provedor estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5443|N/D|Baixo|O seguinte contexto de provedor estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5444|N/D|Baixo|A seguir subcamada estava presente quando o Windows filtragem de plataforma de base de dados de mecanismo de filtragem é iniciado.|  
|5446|N/D|Baixo|Texto explicativo de Plataforma de Filtragem do Windows alterado.|  
|5447|N/D|Baixo|O filtro da Plataforma para Filtros do Windows foi alterado.|  
|5448|N/D|Baixo|Provedor de Plataforma de Filtragem do Windows alterado.|  
|5449|N/D|Baixo|Contexto de provedor de Plataforma de Filtragem do Windows alterado.|  
|5450|N/D|Baixo|Uma subcamada Windows Filtering Platform foi alterada.|  
|5451|N/D|Baixo|Associação de segurança de Modo Rápido IPsec estabelecida.|  
|5452|N/D|Baixo|Associação de segurança de Modo Rápido IPsec finalizada.|  
|5456|N/D|Baixo|O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento do Active Directory.|  
|5457|N/D|Baixo|O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento do Active Directory.|  
|5458|N/D|Baixo|O PAStore Engine aplicou localmente na máquina local a cópia em cache da diretiva IPSec de armazenamento do Active Directory.|  
|5459|N/D|Baixo|O PAStore Engine não pôde aplicar localmente na máquina a cópia em cache da diretiva IPSec de armazenamento do Active Directory.|  
|5460|N/D|Baixo|O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento de Registro local.|  
|5461|N/D|Baixo|O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento de Registro local.|  
|5462|N/D|Baixo|O PAStore Engine não pôde aplicar na máquina algumas regras da diretiva IPSec ativa. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|5463|N/D|Baixo|O PAStore Engine pesquisou as alterações na diretiva IPSec ativa e não detectou alterações.|  
|5464|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPSec ativa, detectou alterações e as aplicou aos serviços IPSec.|  
|5465|N/D|Baixo|O PAStore Engine recebeu um controle para a recarga forçada da diretiva IPSec e processou o controle com êxito.|  
|5466|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está inacessível e usará a cópia em cache da diretiva IPSec do Active Directory. As alterações feitas na diretiva IPSec do Active Directory desde a última sondagem não puderam ser aplicadas.|  
|5467|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está inacessível e não localizou alterações na diretiva. A cópia em cache da diretiva IPSec do Active Directory não está mais sendo usada.|  
|5468|N/D|Baixo|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está acessível, localizou alterações na diretiva e aplicou tais alterações. A cópia em cache da diretiva IPSec do Active Directory não está mais sendo usada.|  
|5471|N/D|Baixo|O PAStore Engine carregou na máquina a diretiva IPSec de armazenamento local.|  
|5472|N/D|Baixo|O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento local.|  
|5473|N/D|Baixo|O PAStore Engine carregou na máquina a diretiva IPSec de armazenamento de diretórios.|  
|5474|N/D|Baixo|O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento de diretórios.|  
|5477|N/D|Baixo|O PAStore Engine não pôde adicionar o filtro de modo rápido.|  
|5479|N/D|Baixo|Os serviços IPSec foram desligados com êxito. O desligamento dos serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança em potencial.|  
|5632|N/D|Baixo|Efetuada solicitação para autenticar uma rede sem fios.|  
|5633|N/D|Baixo|Efetuada solicitação para autenticar uma rede com fios.|  
|5712|N/D|Baixo|Tentativa de Chamada de procedimento remoto (RPC).|  
|5888|N/D|Baixo|Modificado um objeto do catálogo COM+.|  
|5889|N/D|Baixo|Um objeto foi excluído do catálogo COM+.|  
|5890|N/D|Baixo|Um objeto foi adicionado ao catálogo COM+.|  
|6008|N/D|Baixo|O desligamento do sistema anterior era inesperado|  
|6144|N/D|Baixo|Política de segurança nos objetos de diretiva de grupo foi aplicada com êxito.|  
|6272|N/D|Baixo|O Servidor de Políticas de Rede concedeu acesso a um usuário.|  
|N/D|561|Baixo|Foi solicitado o identificador de um objeto.|  
|N/D|563|Baixo|Objeto aberto para exclusão|  
|N/D|625|Baixo|Tipo de conta de usuário alterado|  
|N/D|613|Baixo|Agente de diretiva IPsec iniciado|  
|N/D|614|Baixo|Agente de diretiva IPsec desabilitado|  
|N/D|615|Baixo|Agente de diretiva IPsec|  
|N/D|616|Baixo|Agente de diretiva IPsec encontrou uma falha grave potencial|  
|24577|N/D|Baixo|Criptografia de volume iniciada|  
|24578|N/D|Baixo|Criptografia do volume interrompido|  
|24579|N/D|Baixo|Criptografia do volume concluída|  
|24580|N/D|Baixo|Descriptografia do volume iniciada|  
|24581|N/D|Baixo|Descriptografia do volume interrompido|  
|24582|N/D|Baixo|Descriptografia do volume concluída|  
|24583|N/D|Baixo|Thread de trabalho de conversão de volume iniciada|  
|24584|N/D|Baixo|Thread de trabalho de conversão de volume temporariamente interrompido|  
|24588|N/D|Baixo|A operação de conversão de volume %2 encontrou um erro de setor defeituoso. Valide os dados nesse volume|  
|24595|N/D|Baixo|Volume %2 contém clusters inválidos. Esses clusters serão ignorados durante a conversão.|  
|24621|N/D|Baixo|Verificação do estado inicial: Transação de conversão de volume sem interrupção em %2.|  
|5049|N/D|Baixo|Uma associação de segurança IPSec foi excluída.|  
|5478|N/D|Baixo|Os serviços IPSec foram iniciados com êxito.|  
  
> [!NOTE]  
> Consulte a [eventos de auditoria de segurança do Windows](https://www.microsoft.com/download/details.aspx?id=50034) para obter uma lista de muitas IDs de eventos de segurança e seus significados.  
>
> Execute **wevtutil gp Microsoft-Windows-Security-Auditing /ge /gm:true** para obter uma lista muito detalhada de segurança de todos os IDs de eventos  
  
Para obter mais informações sobre IDs de eventos de segurança do Windows e seus significados, consulte o artigo do Microsoft Support [descrição dos eventos de segurança no Windows 7 e no Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Você também pode baixar [eventos de auditoria de segurança para o 7 do Windows e Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e [Windows 8 e detalhes do evento de segurança do Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que fornecem informações detalhadas de evento para o sistemas operacionais referenciados no formato de planilha.  
