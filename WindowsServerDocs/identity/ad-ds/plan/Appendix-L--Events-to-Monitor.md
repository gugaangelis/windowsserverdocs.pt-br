---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Apêndice L-eventos a serem monitorados
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e069fe004d9256682e5754fc90ae6cba88ee7cb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402728"
---
# <a name="appendix-l-events-to-monitor"></a>Apêndice L: Eventos a Monitorar

>Aplica-se a: Windows Server

A tabela a seguir lista os eventos que você deve monitorar em seu ambiente, de acordo com as recomendações fornecidas em [monitoramento Active Directory para sinais de comprometimento](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Na tabela a seguir, a coluna "ID do evento atual do Windows" lista a ID do evento conforme é implementada em versões do Windows e do Windows Server que estão atualmente no suporte base.  
  
A coluna "ID de evento herdado do Windows" lista a ID de evento correspondente em versões herdadas do Windows, como computadores cliente que executam o Windows XP ou anterior e servidores que executam o Windows Server 2003 ou anterior. A coluna "crítico potencial" identifica se o evento deve ser considerado de baixa, média ou alta importância na detecção de ataques, e a coluna "Resumo do evento" fornece uma breve descrição do evento.  
  
Uma possível criticalidade de alto significa que uma ocorrência do evento deve ser investigada. A criticalidade potencial de médio ou baixo significa que esses eventos só devem ser investigados se ocorrerem inesperadamente ou em números que excedam significativamente a linha de base esperada em um período de tempo medido. Todas as organizações devem testar essas recomendações em seus ambientes antes de criar alertas que exijam respostas investigativas obrigatórias. Cada ambiente é diferente, e alguns dos eventos classificados com uma possível criticalidade de alta podem ocorrer devido a outros eventos inofensivos.  
  
|||||  
|-|-|-|-|  
|**ID de evento do Windows atual**|**ID de evento herdado do Windows**|**Criticalidade potencial**|**Resumo do evento**|  
|4618|N/D|Alto|Ocorreu um padrão de evento de segurança monitorado.|  
|4649|N/D|Alto|Um ataque de repetição foi detectado. Pode ser um falso negativo inofensivo devido a erro de configuração incorreta.|  
|4719|612|Alto|A política de auditoria do sistema foi alterada.|  
|4765|N/D|Alto|Histórico SID adicionado a uma conta.|  
|4766|N/D|Alto|Falha na tentativa de adicionar histórico SID a uma conta.|  
|4794|N/D|Alto|Tentativa de definir a senha do administrador do modo de restauração de serviços de diretório.|  
|4897|801|Alto|Separação de Função Habilitada:|  
|4964|N/D|Alto|Grupos especiais atribuídos a um novo logon.|  
|5124|N/D|Alto|Uma configuração de segurança foi atualizada no serviço de respondente OCSP|  
|N/D|550|Médio a alto|Possível ataque de DoS (negação de serviço)|  
|1102|517|Médio a alto|O log de auditoria foi limpo|  
|4621|N/D|Média|O administrador recuperou o sistema de CrashOnAuditFail. Os usuários que não sejam administradores não poderão efetuar o logon. Alguma atividade para verificação pode não ter sido registrada.|  
|4675|N/D|Média|SIDs filtradas.|  
|4692|N/D|Média|Tentativa de backup da chave mestre de proteção dos dados.|  
|4693|N/D|Média|Tentativa de recuperação da chave mestre de proteção dos dados.|  
|4706|610|Média|Foi criada uma nova confiança para um domínio.|  
|4713|617|Média|A política Kerberos foi alterada.|  
|4714|618|Média|A diretiva de recuperação de dados criptografados foi alterada.|  
|4715|N/D|Média|A política de auditoria (SACL) de um objeto foi alterada.|  
|4716|620|Média|As informações de um domínio confiável foram modificadas.|  
|4724|628|Média|Foi feita uma tentativa de redefinir a senha de uma conta.|  
|4727|631|Média|Foi criado um grupo global com a segurança ativada.|  
|4735|639|Média|Foi alterado um grupo local com a segurança ativada.|  
|4737|641|Média|Foi alterado um grupo global com a segurança ativada.|  
|4739|643|Média|Política de domínio alterada.|  
|4754|658|Média|Foi criado um grupo universal com a segurança ativada.|  
|4755|659|Média|Foi alterado um grupo universal com a segurança ativada.|  
|4764|667|Média|Um grupo desabilitado para segurança foi excluído|  
|4764|668|Média|Um tipo do grupo foi alterado.|  
|4780|684|Média|O ACL foi definido em contas que são membros de grupos de administradores.|  
|4816|N/D|Média|O RPC detectou uma violação de integridade ao descriptografar uma mensagem recebida.|  
|4865|N/D|Média|Adicionada entrada de informações de floresta confiável.|  
|4866|N/D|Média|Removida entrada de informações de floresta confiável.|  
|4867|N/D|Média|Modificada entrada de informações de floresta confiável.|  
|4868|772|Média|O gerenciador de certificados negou uma solicitação de certificado pendente.|  
|4870|774|Média|Serviços de Certificados revogaram um certificado.|  
|4882|786|Média|As permissões de segurança para Serviços de Certificados mudaram.|  
|4885|789|Média|O filtro de auditoria dos Serviços de Certificados mudou.|  
|4890|794|Média|As configurações do gerenciador de certificados para Serviços de Certificados mudaram.|  
|4892|796|Média|Uma propriedade dos Serviços de Certificados mudou.|  
|4896|800|Média|Uma ou mais linhas foram excluídas do banco de dados do certificado.|  
|4906|N/D|Média|O valor CrashOnAuditFail foi alterado.|  
|4907|N/D|Média|As configurações de auditoria do objeto foram alteradas.|  
|4908|N/D|Média|Tabela de logon de grupos especiais modificada.|  
|4912|807|Média|A política de auditoria por usuário foi alterada.|  
|4960|N/D|Média|O IPsec ignorou um pacote recebido que não passou pela verificação de integridade. Se este problema persistir, pode indicar um erro na rede ou que os pacotes estão sendo modificados durante a transferência para este computador. Verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Este erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec.|  
|4961|N/D|Média|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. Se este problema persistir, pode indicar um ataque por repetição contra este computador.|  
|4962|N/D|Média|O IPsec ignorou um pacote recebido que falhou na verificação de repetição. O pacote recebido tinha um número de sequência muito baixo para garantir que não era uma repetição.|  
|4963|N/D|Média|O IPsec ignorou um pacote recebido de texto simples que deveria ter protegido. Geralmente, isso ocorre devido a um computador remoto que altera a sua diretiva do IPsec sem informar este computador. Também pode ser uma tentativa de falsificação da transmissão.|  
|4965|N/D|Média|O IPsec removeu um pacote de um computador remoto com um índice de parâmetro de segurança (SPI) incorreto. Geralmente, esse é causado pelo mau funcionamento de um hardware que está corrompendo os pacotes. Se esses erros persistirem, verifique se os pacotes enviados pelo computador remoto são os mesmos recebidos por este computador. Este erro também pode indicar problemas de interoperabilidade com outras implementações do IPsec. Neste caso, se a conectividade não estiver impedida, esses eventos podem ser ignorados.|  
|4976|N/D|Média|Durante a negociação do modo principal, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4977|N/D|Média|Durante a negociação do modo rápido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4978|N/D|Média|Durante a negociação do modo estendido, o IPsec recebeu um pacote de negociação inválido. Se este problema persistir, pode indicar um problema na rede ou uma tentativa de modificar ou repetir esta negociação.|  
|4983|N/D|Média|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|4984|N/D|Média|Falha na negociação do modo estendido do IPsec. A associação de segurança correspondente do modo principal foi excluída.|  
|5027|N/D|Média|O serviço de Firewall do Windows não pôde recuperar a política de segurança do armazenamento local. O serviço continuará executando a diretiva atual.|  
|5028|N/D|Média|Serviço do Firewall do Windows foi incapaz de analisar a nova diretiva de segurança. O serviço continuará com a diretiva atualmente executada.|  
|5029|N/D|Média|O serviço de Firewall do Windows não pôde inicializar o driver. O serviço continuará executando a diretiva atual.|  
|5030|N/D|Média|Não foi possível iniciar o serviço do Firewall do Windows.|  
|5035|N/D|Média|Driver do Firewall do Windows falhou ao inicializar.|  
|5037|N/D|Média|Driver do Firewall do Windows detectou um erro crítico de tempo de execução. Terminação.|  
|5038|N/D|Média|A integridade do código determinou que o hash de imagem de um arquivo não é válido. O arquivo pode estar corrompido devido a uma modificação não-autorizada, ou o hash inválido pode indicar um erro em potencial do dispositivo de disco.|  
|5120|N/D|Média|Serviço de respondente OCSP iniciado|  
|5121|N/D|Média|Serviço de respondente OCSP interrompido|  
|5122|N/D|Média|Uma entrada de configuração alterada no serviço de respondente OCSP|  
|5123|N/D|Média|Uma entrada de configuração alterada no serviço de respondente OCSP|  
|5376|N/D|Média|Backup efetuado das credenciais do Gerenciador de Credenciais.|  
|5377|N/D|Média|As credenciais do Gerenciador de Credenciais foram restauradas a partir de um backup.|  
|5453|N/D|Média|Negociação IPsec com computador local falhou porque o serviço de Módulos de Criação de Chaves IKE e AuthIP IPsec (IKEEXT) não foi iniciado.|  
|5480|N/D|Média|Os serviços IPSec não puderam obter a lista completa de interfaces de rede na máquina. Esta pode ser uma situação de risco para a segurança da máquina, porque algumas interfaces de rede talvez não obtenham a proteção desejada com os filtros IPSec aplicados. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|5483|N/D|Média|Os serviços IPSec não puderam inicializar o servidor RPC. Os serviços IPSec não puderam ser iniciados.|  
|5484|N/D|Média|Falha grave com os serviços IPSec. Eles foram desativados. O desligamento dos serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança em potencial.|  
|5485|N/D|Média|Os serviços IPSec não puderam processar alguns filtros IPSec em um evento plug and play para interfaces de rede. Esta pode ser uma situação de risco para a segurança da máquina, porque algumas interfaces de rede talvez não obtenham a proteção desejada com os filtros IPSec aplicados. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|6145|N/D|Média|Um ou mais erros ocorreram ao processar a política de segurança nos objetos de Política de Grupo.|  
|6273|N/D|Média|O Servidor de Políticas de Rede negou acesso a um usuário.|  
|6274|N/D|Média|O Servidor de Políticas de Rede descartou a solicitação de um usuário.|  
|6275|N/D|Média|O Servidor de Políticas de Rede descartou a solicitação de contabilização de um usuário.|  
|6276|N/D|Média|O Servidor de Política de Rede colocou um usuário em quarentena.|  
|6277|N/D|Média|O Servidor de Política de Rede concedeu acesso a um usuário, mas colocou-o em experiência porque o host não atendeu à política de integridade.|  
|6278|N/D|Média|O Servidor de Política de Rede concedeu acesso total a um usuário porque o host atendeu a política de integridade definida.|  
|6279|N/D|Média|O Servidor de Política de Rede bloqueou a conta do usuário devido a constantes falhas das tentativas de atualização.|  
|6280|N/D|Média|O Servidor de Política de Rede desbloqueou a conta do usuário.|  
|-|640|Média|Banco de dados de conta geral alterado|  
|-|619|Média|Política de qualidade de serviço alterada|  
|24586|N/D|Média|Foi encontrado um erro ao converter o volume|  
|24592|N/D|Média|Falha ao tentar reiniciar automaticamente a conversão no volume% 2.|  
|24593|N/D|Média|Gravação de metadados: O volume% 2 retorna erros ao tentar modificar os metadados. Se as falhas continuarem, descriptografe o volume|  
|24594|N/D|Média|Recompilação de metadados: Falha ao tentar gravar uma cópia de metadados no volume% 2 e pode aparecer como corrupção do disco. Se as falhas continuarem, descriptografe o volume.|  
|4608|512|Baixa|Windows está iniciando.|  
|4609|513|Baixa|O Windows está sendo desligado.|  
|4610|514|Baixa|Um pacote de autenticação foi carregado pela autoridade de segurança local.|  
|4611|515|Baixa|Um processo de logon confiável foi registrado com a autoridade de segurança local.|  
|4612|516|Baixa|Os recursos internos alocados para a fila de mensagens de auditoria se esgotaram, provocando a perda de algumas auditorias.|  
|4614|518|Baixa|Um pacote de notificação foi carregado pelo Gerente de Contas de Segurança.|  
|4615|519|Baixa|Uso inválido da porta LPC.|  
|4616|520|Baixa|Hora do sistema alterada.|  
|4622|N/D|Baixa|Um pacote de segurança foi carregado pela autoridade de segurança local.|  
|4624|528.540|Baixa|O logon de uma conta foi efetuado com êxito.|  
|4625|529-537539|Baixa|Falha no logon de uma conta.|  
|4634|538|Baixa|Foi efetuado o logoff de uma conta.|  
|4646|N/D|Baixa|Do IKE DoS – modo de prevenção iniciado.|  
|4647|551|Baixa|O usuário iniciou logoff.|  
|4648|552|Baixa|Tentativa de logon com uso de credenciais explícitas.|  
|4650|N/D|Baixa|Foi estabelecida uma associação de segurança do modo IPsec principal. O modo estendido não foi ativado. A autenticação do certificado não foi usada.|  
|4651|N/D|Baixa|Foi estabelecida uma associação de segurança do modo IPsec principal. O modo estendido não foi ativado. Um certificado foi usado na autenticação.|  
|4652|N/D|Baixa|Falha na negociação do modo IPsec principal.|  
|4653|N/D|Baixa|Falha na negociação do modo IPsec principal.|  
|4654|N/D|Baixa|Uma negociação de modo rápido IPsec falhou.|  
|4655|N/D|Baixa|Uma associação de segurança do modo IPsec principal foi terminada.|  
|4656|560|Baixa|Foi solicitado o identificador de um objeto.|  
|4657|567|Baixa|Foi modificado um valor do registro.|  
|4658|562|Baixa|O identificador de um objeto foi fechado.|  
|4659|N/D|Baixa|O identificador de um objeto foi solicitado, com a intenção de excluir.|  
|4660|564|Baixa|Um objeto foi excluído.|  
|4661|565|Baixa|Foi solicitado o identificador de um objeto.|  
|4662|566|Baixa|Foi realizada uma operação em um objeto.|  
|4663|567|Baixa|Houve uma tentativa de acessar um objeto.|  
|4664|N/D|Baixa|Houve a tentativa de criar um vínculo físico.|  
|4665|N/D|Baixa|Houve a tentativa de criar um contexto de cliente de aplicativos.|  
|4666|N/D|Baixa|Um aplicativo tentou realizar uma operação:|  
|4667|N/D|Baixa|O contexto do cliente do aplicativo foi excluído.|  
|4668|N/D|Baixa|Um aplicativo foi inicializado.|  
|4670|N/D|Baixa|As permissões de um objeto foram alteradas.|  
|4671|N/D|Baixa|Um aplicativo tentou acessar um ordinal bloqueado através do TBS.|  
|4672|576|Baixa|Privilégios especiais atribuídos a um novo logon.|  
|4673|577|Baixa|Um serviço de privilégios foi chamado.|  
|4674|578|Baixa|Tentativa de operação em um objeto privilegiado.|  
|4688|592|Baixa|Foi criado um novo processo.|  
|4689|593|Baixa|O processo foi encerrado.|  
|4690|594|Baixa|Foi feita a tentativa de duplicar o identificador de um objeto.|  
|4691|595|Baixa|Foi solicitado o acesso indireto a um objeto.|  
|4694|N/D|Baixa|Tentativa de proteção contra verificação dos dados protegidos.|  
|4695|N/D|Baixa|Tentativa de desproteção contra verificação dos dados protegidos.|  
|4696|600|Baixa|Um token primário foi atribuído ao processo.|  
|4697|601|Baixa|Tentar instalar um serviço|  
|4698|602|Baixa|Uma tarefa agendada foi criada.|  
|4699|602|Baixa|Uma tarefa agendada foi excluída.|  
|4700|602|Baixa|Uma tarefa agendada estava habilitada.|  
|4701|602|Baixa|Uma tarefa agendada estava desabilitada.|  
|4702|602|Baixa|Uma tarefa agendada foi atualizada.|  
|4704|608|Baixa|Um direito de usuário foi atribuído.|  
|4705|609|Baixa|Um direito de usuário foi removido.|  
|4707|611|Baixa|Foi removida a confiança de um domínio.|  
|4709|N/D|Baixa|Os serviços IPsec foram iniciados.|  
|4710|N/D|Baixa|Os serviços IPsec foram desabilitados.|  
|4711|N/D|Baixa|Pode conter qualquer um dos seguintes: O PAStore Engine aplicou localmente na máquina local a cópia em cache da diretiva IPSec de armazenamento do Active Directory. O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento do Active Directory. O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento de Registro local. O PAStore Engine não pôde aplicar localmente na máquina a cópia em cache da diretiva IPSec de armazenamento do Active Directory. O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento do Active Directory. O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento de Registro local. O PAStore Engine não pôde aplicar na máquina algumas regras da diretiva IPSec ativa. O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento de diretórios. O PAStore Engine carregou na máquina a diretiva IPSec de armazenamento de diretórios. O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento local. O PAStore Engine carregou a política IPsec de armazenamento local no computador. O PAStore Engine pesquisou as alterações na política de IPsec ativa e não detectou nenhuma alteração. |  
|4712|N/D|Baixa|Os serviços IPSec encontraram uma falha possivelmente séria.|  
|4717|621|Baixa|O acesso de segurança do sistema foi concedido a uma conta.|  
|4718|622|Baixa|O acesso de segurança do sistema foi removido de uma conta.|  
|4720|624|Baixa|Conta de usuário criada.|  
|4722|626|Baixa|Conta de usuário ativada.|  
|4723|Etapas de resolução para o seguinte evento ID 627|Baixa|Foi feita uma tentativa de alterar a senha de uma conta.|  
|4725|629|Baixa|Conta de usuário desabilitada.|  
|4726|630|Baixa|Conta de usuário excluída.|  
|4728|632|Baixa|Um membro foi adicionado a um grupo global com segurança ativada.|  
|4729|633|Baixa|Um membro foi removido de um grupo global com segurança ativada.|  
|4730|634|Baixa|Foi excluído um grupo global com a segurança ativada.|  
|4731|635|Baixa|Foi criado um grupo local com a segurança ativada.|  
|4732|636|Baixa|Um membro foi adicionado a um grupo local com segurança ativada.|  
|4733|637|Baixa|Um membro foi removido de um grupo local com segurança ativada.|  
|4734|638|Baixa|Foi excluído um grupo local com a segurança ativada.|  
|4738|642|Baixa|Conta de usuário alterada.|  
|4740|644|Baixa|Conta de usuário bloqueada.|  
|4741|645|Baixa|Conta de computador alterada.|  
|4742|646|Baixa|Conta de computador alterada.|  
|4743|647|Baixa|Conta de computador excluída.|  
|4744|648|Baixa|Foi criado um grupo local com a segurança desativada.|  
|4745|649|Baixa|Foi alterado um grupo local com a segurança desativada.|  
|4746|650|Baixa|Um membro foi adicionado a um grupo local com segurança desativada.|  
|4747|651|Baixa|Um membro foi removido de um grupo local com segurança desativada.|  
|4748|652|Baixa|Foi excluído um grupo local com a segurança desativada.|  
|4749|653|Baixa|Foi criado um grupo global com a segurança desativada.|  
|4750|654|Baixa|Foi alterado um grupo global com a segurança desativada.|  
|4751|655|Baixa|Um membro foi adicionado a um grupo global com segurança desativada.|  
|4752|656|Baixa|Um membro foi removido de um grupo global com segurança desativada.|  
|4753|657|Baixa|Foi excluído um grupo global com a segurança desativada.|  
|4756|660|Baixa|Um membro foi adicionado a um grupo universal com segurança ativada.|  
|4757|661|Baixa|Um membro foi removido de um grupo universal com segurança ativada.|  
|4758|662|Baixa|Foi excluído um grupo universal com a segurança ativada.|  
|4759|663|Baixa|Foi criado um grupo universal com a segurança desativada.|  
|4760|664|Baixa|Foi alterado um grupo universal com a segurança desativada.|  
|4761|665|Baixa|Um membro foi adicionado a um grupo universal com segurança desativada.|  
|4762|666|Baixa|Um membro foi removido de um grupo universal com segurança desativada.|  
|4767|671|Baixa|Conta de usuário desbloqueada.|  
|4768|672.676|Baixa|Permissão de autenticação Kerberos (TGT) solicitada.|  
|4769|673|Baixa|Tíquete de serviço Kerberos solicitado.|  
|4770|674|Baixa|Tíquete de serviço Kerberos renovado.|  
|4771|675|Baixa|Falha na pré-autenticação de Kerberos.|  
|4772|672|Baixa|Falha na solicitação de permissão de autenticação Kerberos.|  
|4774|678|Baixa|Conta mapeada para o logon.|  
|4775|679|Baixa|Impossível mapear conta para o logon.|  
|4776|680.681|Baixa|O controlador de domínio tentou validar as credenciais de uma conta.|  
|4777|N/D|Baixa|O controlador de domínio falhou ao validar as credenciais de uma conta.|  
|4778|682|Baixa|Uma sessão foi reconectada a uma Estação de janela.|  
|4779|683|Baixa|Uma sessão foi desconectada de uma Estação de janela.|  
|4781|685|Baixa|O nome de uma conta foi alterado:|  
|4782|N/D|Baixa|O hash de senha que uma conta foi acessada.|  
|4783|667|Baixa|Grupo de aplicativos básicos criado.|  
|4784|N/D|Baixa|Grupo de aplicativos básicos alterado.|  
|4785|689|Baixa|Um membro foi adicionado a um grupo de aplicativos básicos.|  
|4786|690|Baixa|Um membro foi removido de um grupo de aplicativos básicos.|  
|4787|691|Baixa|Um não membro foi adicionado a um grupo de aplicativos básico.|  
|4788|692|Baixa|Um não membro foi removido de um grupo de aplicativos básico.|  
|4789|693|Baixa|Grupo de aplicativos básicos excluído.|  
|4790|694|Baixa|Grupo de consultas LDAP criado.|  
|4793|N/D|Baixa|API de verificação de política de senha chamada.|  
|4800|N/D|Baixa|Estação de trabalho bloqueada.|  
|4801|N/D|Baixa|Estação de trabalho desbloqueada.|  
|4802|N/D|Baixa|Proteção de tela invocada.|  
|4803|N/D|Baixa|Proteção de tela descartada.|  
|4864|N/D|Baixa|Detectada colisão de namespaces.|  
|4869|773|Baixa|Serviços de Certificados receberam uma solicitação de certificado reenviada.|  
|4871|775|Baixa|Serviços de certificados receberam uma solicitação para publicar a CRL (lista de certificados revogados).|  
|4872|776|Baixa|Serviços de certificados publicaram a CRL (lista de certificados revogados).|  
|4873|777|Baixa|Uma extensão de solicitação de certificado foi alterada.|  
|4874|778|Baixa|Um ou mais atributos de solicitação de certificado mudou.|  
|4875|779|Baixa|Serviços de Certificados receberam uma solicitação para desligar.|  
|4876|780|Baixa|Backup de Serviços de Certificado iniciado.|  
|4877|781|Baixa|Backup de Serviços de Certificados concluído.|  
|4878|782|Baixa|Restauração de Serviços de Certificado iniciado.|  
|4879|783|Baixa|Restauração de Serviços de Certificados concluída.|  
|4880|784|Baixa|Serviços de Certificados iniciados.|  
|4881|785|Baixa|Serviços de Certificados interrompidos.|  
|4883|787|Baixa|Serviços de Certificados recuperaram uma chave arquivada.|  
|4884|788|Baixa|Serviços de Certificados importaram um certificado para o banco de dados.|  
|4886|790|Baixa|Serviços de certificados receberam uma solicitação de certificado.|  
|4887|791|Baixa|Serviços de certificados aprovaram uma solicitação de certificado e emitiram um certificado.|  
|4888|792|Baixa|Os Serviços de Certificados negaram uma solicitação de certificado.|  
|4889|793|Baixa|Os Serviços de Certificados definiram o status de solicitação de certificado para pendente.|  
|4891|795|Baixa|Uma entrada de configuração mudou nos Serviços de Certificados.|  
|4893|797|Baixa|Os Serviços de Certificados arquivaram uma chave.|  
|4894|798|Baixa|Os Serviços de Certificados importaram e arquivaram uma chave.|  
|4895|799|Baixa|Os 'Serviços de certificado' publicaram o certificado CA para os serviços de domínio do Active Directory.|  
|4898|802|Baixa|Os Serviços de Certificados carregaram um modelo.|  
|4902|N/D|Baixa|Criada tabela de políticas de auditoria por usuário.|  
|4904|N/D|Baixa|Tentativa de registrar uma origem de evento de segurança.|  
|4905|N/D|Baixa|Tentativa de remover o registro de uma origem de evento de segurança.|  
|4909|N/D|Baixa|As configurações da política local do TBS foram alteradas.|  
|4910|N/D|Baixa|As configurações de Política de Grupo para o TBS foram alteradas.|  
|4928|N/D|Baixa|Estabelecido o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4929|N/D|Baixa|Removido o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4930|N/D|Baixa|Modificado o contexto de nomenclatura da origem da réplica do Active Directory.|  
|4931|N/D|Baixa|Modificado o contexto de nomenclatura do destino da réplica do Active Directory.|  
|4932|N/D|Baixa|Iniciada a sincronização de uma réplica de um contexto de nomenclatura do Active Directory.|  
|4933|N/D|Baixa|Terminada a sincronização de uma réplica de um contexto de nomenclatura do Active Directory.|  
|4934|N/D|Baixa|Os atributos de um objeto do Active Directory foram replicados.|  
|4935|N/D|Baixa|Falha na replicação inicia.|  
|4936|N/D|Baixa|Falha na replicação termina.|  
|4937|N/D|Baixa|Um objeto remanescente foi removido de uma réplica.|  
|4944|N/D|Baixa|A seguinte política estava ativa quando o Firewall do Windows iniciou.|  
|4945|N/D|Baixa|Uma regra foi listada quando o Firewall do Windows iniciou.|  
|4946|N/D|Baixa|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi adicionada.|  
|4947|N/D|Baixa|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi modificada.|  
|4948|N/D|Baixa|A lista de exceções do Firewall do Windows foi alterada. Uma regra foi excluída.|  
|4949|N/D|Baixa|As configurações do Firewall do Windows foram restauradas para os valores padrão.|  
|4950|N/D|Baixa|Configuração do Firewall do Windows alterada.|  
|4951|N/D|Baixa|Uma regra foi ignorada porque o número de versão principal não foi reconhecido pelo Firewall do Windows.|  
|4952|N/D|Baixa|Partes de uma regra foram ignoradas por que seu número de versão secundária não foi reconhecido pelo Firewall do Windows. As demais partes da regra serão executadas.|  
|4953|N/D|Baixa|Uma regra foi ignorada pelo Firewall do Windows, porque ele não pôde analisá-la.|  
|4954|N/D|Baixa|As configurações de políticas de grupo do Firewall do Windows foram alteradas. Novas configurações foram aplicadas.|  
|4956|N/D|Baixa|O Firewall do Windows alterou o perfil ativo.|  
|4957|N/D|Baixa|O Firewall do Windows não aplicou a seguinte regra:|  
|4958|N/D|Baixa|O Firewall do Windows não aplicou a seguinte regra, porque ela se referia a itens que não estão configurados neste computador:|  
|4979|N/D|Baixa|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4980|N/D|Baixa|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4981|N/D|Baixa|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4982|N/D|Baixa|Foram estabelecidas associações de segurança do modo IPsec principal e estendido.|  
|4985|N/D|Baixa|O estado de uma transação mudou.|  
|5024|N/D|Baixa|Serviço do Firewall do Windows iniciado com sucesso.|  
|5025|N/D|Baixa|Serviço do Firewall do Windows interrompido.|  
|5031|N/D|Baixa|Serviço do Firewall do Windows bloqueou um aplicativo para aceitar as conexões recebidas na rede.|  
|5032|N/D|Baixa|O Firewall do Windows não pôde notificar o usuário de que bloqueou um aplicativo para aceitar as conexões recebidas na rede.|  
|5033|N/D|Baixa|Driver do Firewall do Windows iniciado com sucesso.|  
|5034|N/D|Baixa|Driver do Firewall do Windows interrompido.|  
|5039|N/D|Baixa|Uma chave de registro foi virtualizada.|  
|5040|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi adicionado.|  
|5041|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi modificado.|  
|5042|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Autenticação foi excluído.|  
|5043|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi adicionada.|  
|5044|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi modificada.|  
|5045|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Uma regra de segurança de conexão foi excluída.|  
|5046|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um conjunto de criptografia foi adicionado.|  
|5047|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Criptografia foi modificado.|  
|5048|N/D|Baixa|Foi feita uma alteração nas configurações de IPSec. Um Conjunto de Criptografia foi excluído.|  
|5050|N/D|Baixa|Uma tentativa de desabilitar programaticamente o Firewall do Windows usando uma chamada para InetFwProfile. firewall habilitado (false)|  
|5051|N/D|Baixa|Um arquivo foi virtualizado.|  
|5056|N/D|Baixa|Um autoteste de criptografia foi executado.|  
|5057|N/D|Baixa|Falha em uma operação criptográfica primitiva.|  
|5058|N/D|Baixa|Operação de arquivo de chave.|  
|5059|N/D|Baixa|Operação de migração de chave.|  
|5060|N/D|Baixa|Falha na operação de verificação.|  
|5061|N/D|Baixa|Operação de criptografia.|  
|5062|N/D|Baixa|Um autoteste de criptografia de modo kernel foi executado.|  
|5063|N/D|Baixa|Tentativa de operação do provedor criptográfico.|  
|5064|N/D|Baixa|Tentativa de operação do provedor criptográfico.|  
|5065|N/D|Baixa|Tentativa de modificação de contexto criptográfico.|  
|5066|N/D|Baixa|Tentativa de operação da função criptográfica.|  
|5067|N/D|Baixa|Tentativa de modificação da função criptográfica.|  
|5068|N/D|Baixa|Tentativa de operação de provedor da função criptográfica.|  
|5069|N/D|Baixa|Tentativa de operação de propriedade da função criptográfica.|  
|5070|N/D|Baixa|Tentativa de modificação de propriedade da função criptográfica.|  
|5125|N/D|Baixa|Uma solicitação foi enviada ao serviço de respondente OCSP|  
|5126|N/D|Baixa|O certificado de autenticação foi atualizado automaticamente pelo serviço respondente OCSP|  
|5127|N/D|Baixa|O provedor de revogação OCSP atualizou com êxito as informações de revogação|  
|5136|566|Baixa|Um objeto de serviço de diretório foi modificado.|  
|5137|566|Baixa|Um objeto de serviço de diretório foi criado.|  
|5138|N/D|Baixa|Um objeto de serviço de diretório teve sua exclusão cancelada.|  
|5139|N/D|Baixa|Um objeto de serviço de diretório foi movido.|  
|5140|N/D|Baixa|Um objeto de compartilhamento de rede foi acessado.|  
|5141|N/D|Baixa|Um objeto de serviço de diretório foi excluído.|  
|5152|N/D|Baixa|A Plataforma para Filtros do Windows bloqueou um pacote.|  
|5153|N/D|Baixa|Um filtro mais restrito da Plataforma para Filtros do Windows bloqueou um pacote.|  
|5154|N/D|Baixa|A Plataforma para Filtros do Windows permitiu que um aplicativo ou serviço escutasse conexões de entrada em uma porta.|  
|5155|N/D|Baixa|A Plataforma para Filtros do Windows permitiu que um aplicativo ou serviço ouvisse uma porta, em busca de conexões recebidas.|  
|5156|N/D|Baixa|A Plataforma para Filtros do Windows permitiu uma conexão.|  
|5157|N/D|Baixa|A Plataforma para Filtros do Windows bloqueou uma conexão.|  
|5158|N/D|Baixa|A Plataforma para Filtros do Windows permitiu uma associação a uma porta local.|  
|5159|N/D|Baixa|A Plataforma para Filtros do Windows bloqueou uma ligação a uma porta local.|  
|5378|N/D|Baixa|A delegação das credenciais solicitadas não foi permitida pela política.|  
|5440|N/D|Baixa|O seguinte texto explicativo estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5441|N/D|Baixa|O filtro a seguir estava presente quando o Mecanismo de Filtragem Base da Plataforma de Filtragem do Windows foi iniciado.|  
|5442|N/D|Baixa|O seguinte provedor estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5443|N/D|Baixa|O seguinte contexto de provedor estava presente quando o Mecanismo de filtragem básica da Windows Filtering Platform iniciou.|  
|5444|N/D|Baixa|A subcamada a seguir estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|  
|5446|N/D|Baixa|Texto explicativo de Plataforma de Filtragem do Windows alterado.|  
|5447|N/D|Baixa|O filtro da Plataforma para Filtros do Windows foi alterado.|  
|5448|N/D|Baixa|Provedor de Plataforma de Filtragem do Windows alterado.|  
|5449|N/D|Baixa|Contexto de provedor de Plataforma de Filtragem do Windows alterado.|  
|5450|N/D|Baixa|Uma subcamada da plataforma de filtragem do Windows foi alterada.|  
|5451|N/D|Baixa|Associação de segurança de Modo Rápido IPsec estabelecida.|  
|5452|N/D|Baixa|Associação de segurança de Modo Rápido IPsec finalizada.|  
|5456|N/D|Baixa|O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento do Active Directory.|  
|5457|N/D|Baixa|O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento do Active Directory.|  
|5458|N/D|Baixa|O PAStore Engine aplicou localmente na máquina local a cópia em cache da diretiva IPSec de armazenamento do Active Directory.|  
|5459|N/D|Baixa|O PAStore Engine não pôde aplicar localmente na máquina a cópia em cache da diretiva IPSec de armazenamento do Active Directory.|  
|5460|N/D|Baixa|O PAStore Engine aplicou na máquina a diretiva IPSec de armazenamento de Registro local.|  
|5461|N/D|Baixa|O PAStore Engine não pôde aplicar na máquina a diretiva IPSec de armazenamento de Registro local.|  
|5462|N/D|Baixa|O PAStore Engine não pôde aplicar na máquina algumas regras da diretiva IPSec ativa. Use o snap-in Monitor IPSec para diagnosticar o problema.|  
|5463|N/D|Baixa|O PAStore Engine pesquisou as alterações na diretiva IPSec ativa e não detectou alterações.|  
|5464|N/D|Baixa|O PAStore Engine sondou as alterações na diretiva IPSec ativa, detectou alterações e as aplicou aos serviços IPSec.|  
|5465|N/D|Baixa|O PAStore Engine recebeu um controle para a recarga forçada da diretiva IPSec e processou o controle com êxito.|  
|5466|N/D|Baixa|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está inacessível e usará a cópia em cache da diretiva IPSec do Active Directory. As alterações feitas na diretiva IPSec do Active Directory desde a última sondagem não puderam ser aplicadas.|  
|5467|N/D|Baixa|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está inacessível e não localizou alterações na diretiva. A cópia em cache da diretiva IPSec do Active Directory não está mais sendo usada.|  
|5468|N/D|Baixa|O PAStore Engine sondou as alterações na diretiva IPSec do Active Directory, detectou que o Active Directory está acessível, localizou alterações na diretiva e aplicou tais alterações. A cópia em cache da diretiva IPSec do Active Directory não está mais sendo usada.|  
|5471|N/D|Baixa|O PAStore Engine carregou na máquina a diretiva IPSec de armazenamento local.|  
|5472|N/D|Baixa|O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento local.|  
|5473|N/D|Baixa|O PAStore Engine carregou na máquina a diretiva IPSec de armazenamento de diretórios.|  
|5474|N/D|Baixa|O PAStore Engine não pôde carregar na máquina a diretiva IPSec de armazenamento de diretórios.|  
|5477|N/D|Baixa|O PAStore Engine não pôde adicionar o filtro de modo rápido.|  
|5479|N/D|Baixa|Os serviços IPSec foram desligados com êxito. O desligamento dos serviços IPsec pode colocar o computador em um risco maior de ataque à rede ou expor o computador a riscos de segurança em potencial.|  
|5632|N/D|Baixa|Efetuada solicitação para autenticar uma rede sem fios.|  
|5633|N/D|Baixa|Efetuada solicitação para autenticar uma rede com fios.|  
|5712|N/D|Baixa|Tentativa de Chamada de procedimento remoto (RPC).|  
|5888|N/D|Baixa|Modificado um objeto do catálogo COM+.|  
|5889|N/D|Baixa|Um objeto foi excluído do catálogo COM+.|  
|5890|N/D|Baixa|Um objeto foi adicionado ao catálogo COM+.|  
|6008|N/D|Baixa|O desligamento anterior do sistema era inesperado|  
|6144|N/D|Baixa|A política de segurança no Política de Grupo objetos foi aplicada com êxito.|  
|6272|N/D|Baixa|O Servidor de Políticas de Rede concedeu acesso a um usuário.|  
|N/D|561|Baixa|Foi solicitado o identificador de um objeto.|  
|N/D|563|Baixa|Objeto aberto para exclusão|  
|N/D|625|Baixa|Tipo de conta de usuário alterado|  
|N/D|613|Baixa|Agente de política IPsec iniciado|  
|N/D|614|Baixa|Agente de política IPsec desabilitado|  
|N/D|615|Baixa|Agente de política IPsec|  
|N/D|616|Baixa|O agente de política IPsec encontrou uma falha grave potencial|  
|24577|N/D|Baixa|Criptografia do volume iniciada|  
|24578|N/D|Baixa|Criptografia do volume interrompida|  
|24579|N/D|Baixa|Criptografia do volume concluída|  
|24580|N/D|Baixa|Descriptografia do volume iniciada|  
|24581|N/D|Baixa|Descriptografia do volume interrompida|  
|24582|N/D|Baixa|Descriptografia do volume concluída|  
|24583|N/D|Baixa|Thread de trabalho de conversão para o volume iniciado|  
|24584|N/D|Baixa|Thread de trabalho de conversão para o volume temporariamente parado|  
|24588|N/D|Baixa|A operação de conversão no volume% 2 encontrou um erro de setor insatisfatório. Valide os dados neste volume|  
|24595|N/D|Baixa|O volume% 2 contém clusters inválidos. Esses clusters serão ignorados durante a conversão.|  
|24621|N/D|Baixa|Verificação de estado inicial: Transvertendo transação de conversão de volume em% 2.|  
|5049|N/D|Baixa|Uma associação de segurança IPSec foi excluída.|  
|5478|N/D|Baixa|Os serviços IPSec foram iniciados com êxito.|  
  
> [!NOTE]  
> Consulte [eventos de auditoria de segurança do Windows](https://www.microsoft.com/download/details.aspx?id=50034) para obter uma lista de muitas IDs de eventos de segurança e seus significados.  
>
> Execute **wevtutil GP Microsoft-Windows-Security-Auditing/GE/GM: true** para obter uma listagem muito detalhada de todas as IDs de evento de segurança  
  
Para obter mais informações sobre as IDs de eventos de segurança do Windows e seus significados, consulte o artigo Suporte da Microsoft [Descrição dos eventos de segurança no Windows 7 e no Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Você também pode baixar [eventos de auditoria de segurança para os detalhes do evento de segurança do Windows 7 e do Windows server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e do Windows [8 e do Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que fornecem informações detalhadas de eventos para os sistemas operacionais referenciados na planilha ao.  
