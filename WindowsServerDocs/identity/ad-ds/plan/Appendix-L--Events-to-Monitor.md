---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: Apêndice L-eventos a serem monitorados
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0764c9471608b1062a6e868931219bf65e3e668c
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518643"
---
# <a name="appendix-l-events-to-monitor"></a>Apêndice L: Eventos a serem monitorados

>Aplica-se a: Windows Server

A tabela a seguir lista os eventos que você deve monitorar em seu ambiente, de acordo com as recomendações fornecidas em [monitoramento Active Directory para sinais de comprometimento](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Na tabela a seguir, a coluna "ID do evento atual do Windows" lista a ID do evento conforme é implementada em versões do Windows e do Windows Server que estão atualmente no suporte base.

A coluna "ID de evento herdado do Windows" lista a ID de evento correspondente em versões herdadas do Windows, como computadores cliente que executam o Windows XP ou anterior e servidores que executam o Windows Server 2003 ou anterior. A coluna "crítico potencial" identifica se o evento deve ser considerado de baixa, média ou alta importância na detecção de ataques, e a coluna "Resumo do evento" fornece uma breve descrição do evento.

Uma possível criticalidade de alto significa que uma ocorrência do evento deve ser investigada. A criticalidade potencial de médio ou baixo significa que esses eventos só devem ser investigados se ocorrerem inesperadamente ou em números que excedam significativamente a linha de base esperada em um período de tempo medido. Todas as organizações devem testar essas recomendações em seus ambientes antes de criar alertas que exijam respostas investigativas obrigatórias. Cada ambiente é diferente, e alguns dos eventos classificados com uma possível criticalidade de alta podem ocorrer devido a outros eventos inofensivos.

|**ID de Evento Atual do Windows**|**ID de evento herdado do Windows**|**Criticidade Potencial**|**Resumo do Evento**|
|--|--|--|--|
|4618|N/D|Alta|Um padrão de evento de segurança monitorado ocorreu.|
|4649|N/D|Alta|Foi detectado um ataque de reprodução. Pode ser um falso negativo inofensivo devido a erro de configuração incorreta.|
|4719|612|Alta|A política de auditoria do sistema foi alterada.|
|4765|N/D|Alta|O histórico de SID foi adicionado a uma conta.|
|4766|N/D|Alta|Falha ao tentar adicionar o histórico SID a uma conta.|
|4794|N/D|Alta|Foi feita uma tentativa de definir o Modo de Restauração dos Serviços de Diretório.|
|4897|801|Alta|Separação de funções habilitada:|
|4964|N/D|Alta|Grupos especiais foram atribuídos a um novo logon.|
|5124|N/D|Alta|Uma configuração de segurança foi atualizada no serviço de respondente OCSP|
|N/D|550|Médio a alto|Possível ataque de DoS (negação de serviço)|
|1102|517|Médio a alto|O log de auditoria foi limpo|
|4621|N/D|Médio|O administrador recuperou o sistema do CrashOnAuditFail. Os usuários que não forem administradores agora terão permissão para fazer logon. Algumas atividades auditáveis podem não ter sido registradas.|
|4675|N/D|Médio|Os SIDs foram filtrados.|
|4692|N/D|Médio|Tentativa de backup da chave mestra de proteção de dados.|
|4693|N/D|Médio|Tentativa de recuperação da chave mestra de proteção de dados.|
|4706|610|Médio|Uma nova relação de confiança foi criada para um domínio.|
|4713|617|Médio|A política do Kerberos foi alterada.|
|4714|618|Médio|A política de recuperação de dados criptografados foi alterada.|
|4715|N/D|Médio|A política de auditoria (SACL) em um objeto foi alterada.|
|4716|620|Médio|As informações de domínio confiável foram modificadas.|
|4724|628|Médio|Foi feita uma tentativa de redefinir a senha de uma conta.|
|4727|631|Médio|Um grupo global habilitado para segurança foi criado.|
|4735|639|Médio|Um grupo local habilitado para segurança foi alterado.|
|4737|641|Médio|Um grupo global habilitado para segurança foi alterado.|
|4739|643|Médio|A política de domínio foi alterada.|
|4754|658|Médio|Um grupo universal habilitado para segurança foi criado.|
|4755|659|Médio|Um grupo universal habilitado para segurança foi alterado.|
|4764|667|Médio|Um grupo desabilitado para segurança foi excluído|
|4764|668|Médio|O tipo de um grupo foi alterado.|
|4780|684|Médio|A ACL foi definida em contas que são membros de grupos de administradores.|
|4816|N/D|Médio|O RPC detectou uma violação de integridade ao descriptografar uma mensagem de entrada.|
|4865|N/D|Médio|Foi adicionada uma entrada de informações de floresta confiável.|
|4866|N/D|Médio|Uma entrada de informações de floresta confiável foi removida.|
|4867|N/D|Médio|Uma entrada de informações de floresta confiável foi modificada.|
|4868|772|Médio|O gerenciador de certificados negou uma solicitação de certificado pendente.|
|4870|774|Médio|Serviços de Certificados revogaram um certificado.|
|4882|786|Médio|As permissões de segurança para Serviços de Certificados mudaram.|
|4885|789|Médio|O filtro de auditoria dos Serviços de Certificados mudou.|
|4890|794|Médio|As configurações do gerenciador de certificados para Serviços de Certificados mudaram.|
|4892|796|Médio|Uma propriedade dos Serviços de Certificados mudou.|
|4896|800|Médio|Uma ou mais linhas foram excluídas do banco de dados do certificado.|
|4906|N/D|Médio|O valor de CrashOnAuditFail foi alterado.|
|4907|N/D|Médio|As configurações de auditoria no objeto foram alteradas.|
|4908|N/D|Médio|Tabela de logon de grupos especiais modificada.|
|4912|807|Médio|A política de auditoria por usuário foi alterada.|
|4960|N/D|Médio|O IPsec removeu um pacote de entrada que falhou em uma verificação de integridade. Se esse problema persistir, isso pode indicar um problema de rede ou que os pacotes estão sendo modificados em trânsito para este computador. Verifique se os pacotes enviados do computador remoto são os mesmos recebidos por este computador. Esse erro também pode indicar problemas de interoperabilidade com outras implementações de IPsec.|
|4961|N/D|Médio|O IPsec removeu um pacote de entrada que falhou em uma verificação de reprodução. Se esse problema persistir, ele poderá indicar um ataque de reprodução nesse computador.|
|4962|N/D|Médio|O IPsec removeu um pacote de entrada que falhou em uma verificação de reprodução. O pacote de entrada tinha um número de sequência muito baixo para garantir que ele não era uma reprodução.|
|4963|N/D|Médio|O IPsec removeu um pacote de texto não criptografado de entrada que deveria ter sido protegido. Isso geralmente ocorre devido ao computador remoto alterar sua política IPsec sem informar este computador. Isso também pode ser uma tentativa de ataque de falsificação.|
|4965|N/D|Médio|O IPsec recebeu um pacote de um computador remoto com um SPI (índice de parâmetro de segurança) incorreto. Isso geralmente é causado por hardware com defeito que está corrompindo os pacotes. Se esses erros persistirem, verifique se os pacotes enviados do computador remoto são os mesmos recebidos por este computador. Esse erro também pode indicar problemas de interoperabilidade com outras implementações de IPsec. Nesse caso, se a conectividade não for impedida, esses eventos poderão ser ignorados.|
|4976|N/D|Médio|Durante a negociação de modo principal, o IPsec recebeu um pacote de negociação inválido. Se esse problema persistir, isso pode indicar um problema de rede ou uma tentativa de modificar ou reproduzir essa negociação.|
|4977|N/D|Médio|Durante a negociação de modo rápido, o IPsec recebeu um pacote de negociação inválido. Se esse problema persistir, isso pode indicar um problema de rede ou uma tentativa de modificar ou reproduzir essa negociação.|
|4978|N/D|Médio|Durante a negociação em modo estendido, o IPsec recebeu um pacote de negociação inválido. Se esse problema persistir, isso pode indicar um problema de rede ou uma tentativa de modificar ou reproduzir essa negociação.|
|4983|N/D|Médio|Uma negociação de modo estendido IPsec falhou. A associação de segurança do modo principal correspondente foi excluída.|
|4984|N/D|Médio|Uma negociação de modo estendido IPsec falhou. A associação de segurança do modo principal correspondente foi excluída.|
|5027|N/D|Médio|O serviço de firewall do Windows não pôde recuperar a política de segurança do armazenamento local. O serviço continuará executando a diretiva atual.|
|5028|N/D|Médio|O serviço de firewall do Windows não pôde analisar a nova política de segurança. O serviço continuará com a diretiva atualmente executada.|
|5029|N/D|Médio|Falha do serviço de firewall do Windows ao inicializar o driver. O serviço continuará executando a diretiva atual.|
|5030|N/D|Médio|Falha ao iniciar o serviço de firewall do Windows.|
|5035|N/D|Médio|Falha ao iniciar o driver do firewall do Windows.|
|5037|N/D|Médio|O driver do firewall do Windows detectou um erro crítico de tempo de execução Terminação.|
|5038|N/D|Médio|A integridade do código determinou que o hash de imagem de um arquivo não é válido. O arquivo pode estar corrompido devido a uma modificação não autorizada ou o hash inválido pode indicar um erro de dispositivo de disco potencial.|
|5120|N/D|Médio|Serviço de respondente OCSP iniciado|
|5121|N/D|Médio|Serviço de respondente OCSP interrompido|
|5122|N/D|Médio|Uma entrada de configuração alterada no serviço de respondente OCSP|
|5123|N/D|Médio|Uma entrada de configuração alterada no serviço de respondente OCSP|
|5376|N/D|Médio|O backup das credenciais do Gerenciador de credenciais foi feito.|
|5377|N/D|Médio|As credenciais do Gerenciador de credenciais foram restauradas a partir de um backup.|
|5453|N/D|Médio|Uma negociação IPsec com um computador remoto falhou porque o serviço de módulos de chaveamento de IPsec IKE e AuthIP (IKEEXT) não foi iniciado.|
|5480|N/D|Médio|Os serviços IPsec não puderam obter a lista completa de interfaces de rede no computador. Isso representa um risco de segurança potencial, pois algumas das interfaces de rede podem não obter a proteção fornecida pelos filtros IPsec aplicados. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|
|5483|N/D|Médio|Os serviços IPsec não inicializaram o servidor RPC. Não foi possível iniciar os serviços IPsec.|
|5484|N/D|Médio|Os serviços IPsec tiveram uma falha crítica e foram desligados. O desligamento de serviços IPsec pode colocar o computador em um risco maior de ataque de rede ou expor o computador a possíveis riscos de segurança.|
|5485|N/D|Médio|Os serviços IPsec falharam ao processar alguns filtros IPsec em um evento plug-and-Play para interfaces de rede. Isso representa um risco de segurança potencial, pois algumas das interfaces de rede podem não obter a proteção fornecida pelos filtros IPsec aplicados. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|
|6145|N/D|Médio|Um ou mais erros ocorreram ao processar a política de segurança nos objetos de Política de Grupo.|
|6273|N/D|Médio|O servidor de políticas de rede negou o acesso a um usuário.|
|6274|N/D|Médio|O servidor de políticas de rede descartou a solicitação de um usuário.|
|6275|N/D|Médio|O servidor de políticas de rede descartou a solicitação de contabilização de um usuário.|
|6276|N/D|Médio|O servidor de políticas de rede colocou em quarentena um usuário.|
|6277|N/D|Médio|O servidor de políticas de rede concedeu acesso a um usuário, mas o colocou em experiência porque o host não atendeu à política de integridade definida.|
|6278|N/D|Médio|O servidor de políticas de rede concedeu acesso completo a um usuário porque o host atendeu à política de integridade definida.|
|6279|N/D|Médio|O servidor de políticas de rede bloqueou a conta de usuário devido a tentativas de autenticação com falha repetidas.|
|6280|N/D|Médio|O servidor de políticas de rede desbloqueou a conta de usuário.|
|-|640|Médio|Banco de dados de conta geral alterado|
|-|619|Médio|Política de qualidade de serviço alterada|
|24586|N/D|Médio|Foi encontrado um erro ao converter o volume|
|24592|N/D|Médio|Falha ao tentar reiniciar automaticamente a conversão no volume %2.|
|24593|N/D|Médio|Gravação de metadados: o volume %2 retorna erros ao tentar modificar os metadados. Se as falhas continuarem, descriptografe o volume|
|24594|N/D|Médio|Recompilação de metadados: uma tentativa de gravar uma cópia de metadados no volume %2 falhou e pode aparecer como corrupção do disco. Se as falhas continuarem, descriptografe o volume.|
|4608|512|Baixo|O Windows está sendo inicializado.|
|4609|513|Baixo|O Windows está sendo desligado.|
|4610|514|Baixo|Um pacote de autenticação foi carregado pela autoridade de segurança local.|
|4611|515|Baixo|Um processo de logon confiável foi registrado com a autoridade de segurança local.|
|4612|516|Baixo|Recursos internos alocados para o enfileiramento de mensagens de auditoria foram esgotados, levando à perda de algumas auditorias.|
|4614|518|Baixo|Um pacote de notificação foi carregado pelo Gerenciador de contas de segurança.|
|4615|519|Baixo|Uso inválido da porta LPC.|
|4616|520|Baixo|A hora do sistema foi alterada.|
|4622|N/D|Baixo|Um pacote de segurança foi carregado pela autoridade de segurança local.|
|4624|528.540|Baixo|Uma conta foi registrada com êxito.|
|4625|529-537539|Baixo|Falha ao fazer logon em uma conta.|
|4634|538|Baixo|Uma conta foi desconectada.|
|4646|N/D|Baixo|Do IKE DoS – modo de prevenção iniciado.|
|4647|551|Baixo|Logoff iniciado pelo usuário.|
|4648|552|Baixo|Houve uma tentativa de logon usando credenciais explícitas.|
|4650|N/D|Baixo|Uma associação de segurança de modo principal do IPsec foi estabelecida. O modo estendido não foi habilitado. A autenticação de certificado não foi usada.|
|4651|N/D|Baixo|Uma associação de segurança de modo principal do IPsec foi estabelecida. O modo estendido não foi habilitado. Um certificado foi usado para autenticação.|
|4652|N/D|Baixo|Uma negociação de modo principal IPsec falhou.|
|4653|N/D|Baixo|Uma negociação de modo principal IPsec falhou.|
|4654|N/D|Baixo|Uma negociação de modo rápido IPsec falhou.|
|4655|N/D|Baixo|Uma associação de segurança de modo principal do IPsec foi encerrada.|
|4656|560|Baixo|Um identificador para um objeto foi solicitado.|
|4657|567|Baixo|Um valor de registro foi modificado.|
|4658|562|Baixo|O identificador de um objeto foi fechado.|
|4659|N/D|Baixo|Um identificador para um objeto foi solicitado com tentativa de exclusão.|
|4660|564|Baixo|Um objeto foi excluído.|
|4661|565|Baixo|Um identificador para um objeto foi solicitado.|
|4662|566|Baixo|Uma operação foi executada em um objeto.|
|4663|567|Baixo|Foi feita uma tentativa de acessar um objeto.|
|4664|N/D|Baixo|Foi feita uma tentativa de criar um link físico.|
|4665|N/D|Baixo|Foi feita uma tentativa de criar um contexto de cliente de aplicativo.|
|4666|N/D|Baixo|Um aplicativo tentou uma operação:|
|4667|N/D|Baixo|Um contexto de cliente de aplicativo foi excluído.|
|4668|N/D|Baixo|Um aplicativo foi inicializado.|
|4670|N/D|Baixo|As permissões em um objeto foram alteradas.|
|4671|N/D|Baixo|Um aplicativo tentou acessar um ordinal bloqueado por meio do TBS.|
|4672|576|Baixo|Privilégios especiais atribuídos a um novo logon.|
|4673|577|Baixo|Um serviço com privilégios foi chamado.|
|4674|578|Baixo|Uma operação foi tentada em um objeto com privilégios.|
|4688|592|Baixo|Um novo processo foi criado.|
|4689|593|Baixo|Um processo foi encerrado.|
|4690|594|Baixo|Foi feita uma tentativa de duplicar um identificador para um objeto.|
|4691|595|Baixo|Foi solicitado o acesso indireto a um objeto.|
|4694|N/D|Baixo|Tentativa de proteção de dados protegidos auditáveis.|
|4695|N/D|Baixo|Tentativa de desproteção de dados protegidos auditáveis.|
|4696|600|Baixo|Um token primário foi atribuído ao processo.|
|4697|601|Baixo|Tentar instalar um serviço|
|4698|602|Baixo|Uma tarefa agendada foi criada.|
|4699|602|Baixo|Uma tarefa agendada foi excluída.|
|4700|602|Baixo|Uma tarefa agendada foi habilitada.|
|4701|602|Baixo|Uma tarefa agendada foi desabilitada.|
|4702|602|Baixo|Uma tarefa agendada foi atualizada.|
|4704|608|Baixo|Um direito de usuário foi atribuído.|
|4705|609|Baixo|Um direito de usuário foi removido.|
|4707|611|Baixo|Uma relação de confiança com um domínio foi removida.|
|4709|N/D|Baixo|Os serviços IPsec foram iniciados.|
|4710|N/D|Baixo|Os serviços IPsec foram desabilitados.|
|4711|N/D|Baixo|Pode conter qualquer um dos seguintes: o PAStore Engine aplicou uma cópia armazenada localmente em cache da política IPsec de armazenamento de Active Directory no computador. O PAStore Engine aplicado Active Directory política IPsec de armazenamento no computador. O PAStore Engine aplicou a política IPsec de armazenamento do Registro local no computador. O PAStore Engine não pôde aplicar a cópia armazenada em cache localmente da política IPsec de armazenamento de Active Directory no computador. O PAStore Engine não pôde aplicar Active Directory política IPsec de armazenamento no computador. O PAStore Engine não pôde aplicar a política IPsec de armazenamento do Registro local no computador. O PAStore Engine não pôde aplicar algumas regras da política IPsec ativa no computador. O PAStore Engine não pôde carregar a política IPsec de armazenamento de diretório no computador. O PAStore Engine carregou a política IPsec de armazenamento de diretório no computador. O PAStore Engine não pôde carregar a política IPsec de armazenamento local no computador. O PAStore Engine carregou a política IPsec de armazenamento local no computador. O PAStore Engine pesquisou as alterações na política de IPsec ativa e não detectou nenhuma alteração. |
|4712|N/D|Baixo|Os serviços IPsec encontraram uma falha potencialmente séria.|
|4717|621|Baixo|O acesso à segurança do sistema foi concedido a uma conta.|
|4718|622|Baixo|O acesso à segurança do sistema foi removido de uma conta.|
|4720|624|Baixo|Uma conta de usuário foi criada.|
|4722|626|Baixo|Uma conta de usuário foi habilitada.|
|4723|627|Baixo|Foi feita uma tentativa de alterar a senha de uma conta.|
|4725|629|Baixo|Uma conta de usuário foi desabilitada.|
|4726|630|Baixo|Uma conta de usuário foi excluída.|
|4728|632|Baixo|Um membro foi adicionado a um grupo global habilitado para segurança.|
|4729|633|Baixo|Um membro foi removido de um grupo global habilitado para segurança.|
|4730|634|Baixo|Um grupo global habilitado para segurança foi excluído.|
|4731|635|Baixo|Um grupo local habilitado para segurança foi criado.|
|4732|636|Baixo|Um membro foi adicionado a um grupo local habilitado para segurança.|
|4733|637|Baixo|Um membro foi removido de um grupo local habilitado para segurança.|
|4734|638|Baixo|Um grupo local habilitado para segurança foi excluído.|
|4738|642|Baixo|Uma conta de usuário foi alterada.|
|4740|644|Baixo|Uma conta de usuário foi bloqueada.|
|4741|645|Baixo|Uma conta de computador foi alterada.|
|4742|646|Baixo|Uma conta de computador foi alterada.|
|4743|647|Baixo|Uma conta de computador foi excluída.|
|4744|648|Baixo|Um grupo local com segurança desabilitada foi criado.|
|4745|649|Baixo|Um grupo local com segurança desabilitada foi alterado.|
|4746|650|Baixo|Um membro foi adicionado a um grupo local com segurança desabilitada.|
|4747|651|Baixo|Um membro foi removido de um grupo local com segurança desabilitada.|
|4748|652|Baixo|Um grupo local com segurança desabilitada foi excluído.|
|4749|653|Baixo|Um grupo global desabilitado para segurança foi criado.|
|4750|654|Baixo|Um grupo global desabilitado para segurança foi alterado.|
|4751|655|Baixo|Um membro foi adicionado a um grupo global desabilitado para segurança.|
|4752|656|Baixo|Um membro foi removido de um grupo global desabilitado para segurança.|
|4753|657|Baixo|Um grupo global desabilitado para segurança foi excluído.|
|4756|660|Baixo|Um membro foi adicionado a um grupo universal habilitado para segurança.|
|4757|661|Baixo|Um membro foi removido de um grupo universal habilitado para segurança.|
|4758|662|Baixo|Um grupo universal habilitado para segurança foi excluído.|
|4759|663|Baixo|Um grupo universal desabilitado para segurança foi criado.|
|4760|664|Baixo|Um grupo universal desabilitado para segurança foi alterado.|
|4761|665|Baixo|Um membro foi adicionado a um grupo universal desabilitado para segurança.|
|4762|666|Baixo|Um membro foi removido de um grupo universal com segurança desabilitada.|
|4767|671|Baixo|Uma conta de usuário foi desbloqueada.|
|4768|672.676|Baixo|Foi solicitado um tíquete de autenticação Kerberos (TGT).|
|4769|673|Baixo|Um tíquete de serviço Kerberos foi solicitado.|
|4770|674|Baixo|Um tíquete de serviço Kerberos foi renovado.|
|4771|675|Baixo|Falha na pré-autenticação de Kerberos.|
|4772|672|Baixo|Falha em uma solicitação de tíquete de autenticação Kerberos.|
|4774|678|Baixo|Uma conta foi mapeada para logon.|
|4775|679|Baixo|Não foi possível mapear uma conta para logon.|
|4776|680.681|Baixo|O controlador de domínio tentou validar as credenciais de uma conta.|
|4777|N/D|Baixo|Falha do controlador de domínio ao validar as credenciais de uma conta.|
|4778|682|Baixo|Uma sessão foi reconectada a uma estação de janela.|
|4779|683|Baixo|Uma sessão foi desconectada de uma estação de janela.|
|4781|685|Baixo|O nome de uma conta foi alterado:|
|4782|N/D|Baixo|O hash de senha que uma conta foi acessada.|
|4783|667|Baixo|Um grupo de aplicativos básico foi criado.|
|4784|N/D|Baixo|Um grupo de aplicativos básico foi alterado.|
|4785|689|Baixo|Um membro foi adicionado a um grupo de aplicativos básico.|
|4786|690|Baixo|Um membro foi removido de um grupo de aplicativos básico.|
|4787|691|Baixo|Um não membro foi adicionado a um grupo de aplicativos básico.|
|4788|692|Baixo|Um não membro foi removido de um grupo de aplicativos básico.|
|4789|693|Baixo|Um grupo de aplicativos básico foi excluído.|
|4790|694|Baixo|Um grupo de consulta LDAP foi criado.|
|4793|N/D|Baixo|A API de verificação de política de senha foi chamada.|
|4800|N/D|Baixo|A estação de trabalho foi bloqueada.|
|4801|N/D|Baixo|A estação de trabalho foi desbloqueada.|
|4802|N/D|Baixo|A proteção de tela foi invocada.|
|4803|N/D|Baixo|A proteção de tela foi ignorada.|
|4864|N/D|Baixo|Foi detectada uma colisão de namespace.|
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
|4895|799|Baixo|Os serviços de certificados publicaram o certificado de autoridade de certificação para Active Directory Domain Services.|
|4898|802|Baixo|Os Serviços de Certificados carregaram um modelo.|
|4902|N/D|Baixo|A tabela de política de auditoria por usuário foi criada.|
|4904|N/D|Baixo|Foi feita uma tentativa de registrar uma fonte de eventos de segurança.|
|4905|N/D|Baixo|Foi feita uma tentativa de cancelar o registro de uma fonte de eventos de segurança.|
|4909|N/D|Baixo|As configurações de política local para o TBS foram alteradas.|
|4910|N/D|Baixo|As configurações de Política de Grupo para o TBS foram alteradas.|
|4928|N/D|Baixo|Foi estabelecido um contexto de nomenclatura de origem de Active Directory réplica.|
|4929|N/D|Baixo|Um contexto de nomenclatura de origem Active Directory réplica foi removido.|
|4930|N/D|Baixo|Um contexto de nomenclatura de origem de réplica Active Directory foi modificado.|
|4931|N/D|Baixo|Um contexto de nomenclatura de destino de réplica Active Directory foi modificado.|
|4932|N/D|Baixo|A sincronização de uma réplica de um contexto de nomenclatura Active Directory foi iniciada.|
|4933|N/D|Baixo|A sincronização de uma réplica de um contexto de nomenclatura de Active Directory foi encerrada.|
|4934|N/D|Baixo|Os atributos de um objeto Active Directory foram replicados.|
|4935|N/D|Baixo|A falha na replicação é iniciada.|
|4936|N/D|Baixo|A falha de replicação é encerrada.|
|4937|N/D|Baixo|Um objeto remanescente foi removido de uma réplica.|
|4944|N/D|Baixo|A política a seguir estava ativa quando o Firewall do Windows foi iniciado.|
|4945|N/D|Baixo|Uma regra foi listada quando o Firewall do Windows foi iniciado.|
|4946|N/D|Baixo|Foi feita uma alteração na lista de exceções do firewall do Windows. Uma regra foi adicionada.|
|4947|N/D|Baixo|Foi feita uma alteração na lista de exceções do firewall do Windows. Uma regra foi modificada.|
|4948|N/D|Baixo|Foi feita uma alteração na lista de exceções do firewall do Windows. Uma regra foi excluída.|
|4949|N/D|Baixo|As configurações do firewall do Windows foram restauradas para os valores padrão.|
|4950|N/D|Baixo|Uma configuração do firewall do Windows foi alterada.|
|4951|N/D|Baixo|Uma regra foi ignorada porque seu número de versão principal não foi reconhecido pelo firewall do Windows.|
|4952|N/D|Baixo|Partes de uma regra foram ignoradas porque seu número de versão secundária não foi reconhecido pelo firewall do Windows. As demais partes da regra serão executadas.|
|4953|N/D|Baixo|Uma regra foi ignorada pelo firewall do Windows porque não pôde analisar a regra.|
|4954|N/D|Baixo|As configurações de Política de Grupo do firewall do Windows foram alteradas. Novas configurações foram aplicadas.|
|4956|N/D|Baixo|O Firewall do Windows alterou o perfil ativo.|
|4957|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra:|
|4958|N/D|Baixo|O Firewall do Windows não aplicou a seguinte regra porque a regra se referia a itens não configurados neste computador:|
|4979|N/D|Baixo|As associações de segurança do modo principal do IPsec e do modo estendido foram estabelecidas.|
|4980|N/D|Baixo|As associações de segurança do modo principal do IPsec e do modo estendido foram estabelecidas.|
|4981|N/D|Baixo|As associações de segurança do modo principal do IPsec e do modo estendido foram estabelecidas.|
|4982|N/D|Baixo|As associações de segurança do modo principal do IPsec e do modo estendido foram estabelecidas.|
|4985|N/D|Baixo|O estado de uma transação foi alterado.|
|5024|N/D|Baixo|O serviço de firewall do Windows foi iniciado com êxito.|
|5025|N/D|Baixo|O serviço de firewall do Windows foi interrompido.|
|5031|N/D|Baixo|O serviço de firewall do Windows bloqueou um aplicativo de aceitar conexões de entrada na rede.|
|5032|N/D|Baixo|O Firewall do Windows não pôde notificar o usuário de que o aplicativo bloqueou a aceitação de conexões de entrada na rede.|
|5033|N/D|Baixo|O driver do firewall do Windows foi iniciado com êxito.|
|5034|N/D|Baixo|O driver do firewall do Windows foi interrompido.|
|5039|N/D|Baixo|Uma chave do registro foi virtualizada.|
|5040|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi adicionado.|
|5041|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi modificado.|
|5042|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de autenticação foi excluído.|
|5043|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de conexão foi adicionada.|
|5044|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de conexão foi modificada.|
|5045|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Uma regra de segurança de conexão foi excluída.|
|5046|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi adicionado.|
|5047|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi modificado.|
|5048|N/D|Baixo|Foi feita uma alteração nas configurações de IPsec. Um conjunto de criptografia foi excluído.|
|5050|N/D|Baixo|Uma tentativa de desabilitar programaticamente o Firewall do Windows usando uma chamada para InetFwProfile. firewall habilitado (false)|
|5051|N/D|Baixo|Um arquivo foi virtualizado.|
|5056|N/D|Baixo|Um autoteste de criptografia foi executado.|
|5057|N/D|Baixo|Uma operação primitiva de criptografia falhou.|
|5058|N/D|Baixo|Operação de arquivo de chave.|
|5059|N/D|Baixo|Operação de migração de chave.|
|5060|N/D|Baixo|Falha na operação de verificação.|
|5061|N/D|Baixo|Operação de criptografia.|
|5062|N/D|Baixo|Um autoteste de criptografia de modo kernel foi executado.|
|5063|N/D|Baixo|Foi tentada uma operação de provedor criptográfico.|
|5064|N/D|Baixo|Tentativa de operação de contexto criptográfico.|
|5065|N/D|Baixo|Tentativa de modificação de contexto criptográfico.|
|5066|N/D|Baixo|Foi tentada uma operação de função criptográfica.|
|5067|N/D|Baixo|Tentativa de modificação de uma função criptográfica.|
|5068|N/D|Baixo|Foi tentada uma operação de provedor de funções criptográficas.|
|5069|N/D|Baixo|Tentativa de operação de propriedade de função criptográfica.|
|5070|N/D|Baixo|Tentativa de modificação de propriedade de função criptográfica.|
|5125|N/D|Baixo|Uma solicitação foi enviada ao serviço de respondente OCSP|
|5126|N/D|Baixo|O certificado de autenticação foi atualizado automaticamente pelo serviço respondente OCSP|
|5127|N/D|Baixo|O provedor de revogação OCSP atualizou com êxito as informações de revogação|
|5136|566|Baixo|Um objeto de serviço de diretório foi modificado.|
|5137|566|Baixo|Um objeto de serviço de diretório foi criado.|
|5138|N/D|Baixo|Um objeto de serviço de diretório não foi excluído.|
|5139|N/D|Baixo|Um objeto de serviço de diretório foi movido.|
|5140|N/D|Baixo|Um objeto de compartilhamento de rede foi acessado.|
|5141|N/D|Baixo|Um objeto de serviço de diretório foi excluído.|
|5152|N/D|Baixo|A plataforma de filtragem do Windows bloqueou um pacote.|
|5153|N/D|Baixo|Um filtro mais restritivo da plataforma de filtragem do Windows bloqueou um pacote.|
|5154|N/D|Baixo|A plataforma de filtragem do Windows permitiu que um aplicativo ou serviço escutasse em uma porta para conexões de entrada.|
|5155|N/D|Baixo|A plataforma de filtragem do Windows bloqueou um aplicativo ou serviço de escutar em uma porta para conexões de entrada.|
|5156|N/D|Baixo|A plataforma de filtragem do Windows permitiu uma conexão.|
|5157|N/D|Baixo|A plataforma de filtragem do Windows bloqueou uma conexão.|
|5158|N/D|Baixo|A plataforma de filtragem do Windows permitiu uma ligação a uma porta local.|
|5159|N/D|Baixo|A plataforma de filtragem do Windows bloqueou uma associação a uma porta local.|
|5378|N/D|Baixo|A delegação de credenciais solicitada não foi permitida pela política.|
|5440|N/D|Baixo|O balão a seguir estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|
|5441|N/D|Baixo|O filtro a seguir estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|
|5442|N/D|Baixo|O provedor a seguir estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|
|5443|N/D|Baixo|O seguinte contexto de provedor estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|
|5444|N/D|Baixo|A subcamada a seguir estava presente quando o mecanismo de filtragem base da plataforma de filtragem do Windows foi iniciado.|
|5446|N/D|Baixo|Um texto explicativo da plataforma de filtragem do Windows foi alterado.|
|5447|N/D|Baixo|Um filtro da plataforma de filtragem do Windows foi alterado.|
|5448|N/D|Baixo|Um provedor de plataforma de filtragem do Windows foi alterado.|
|5449|N/D|Baixo|Um contexto do provedor da plataforma de filtragem do Windows foi alterado.|
|5450|N/D|Baixo|Uma subcamada da plataforma de filtragem do Windows foi alterada.|
|5451|N/D|Baixo|Uma associação de segurança de modo rápido do IPsec foi estabelecida.|
|5452|N/D|Baixo|Uma associação de segurança de modo rápido do IPsec foi encerrada.|
|5456|N/D|Baixo|O PAStore Engine aplicado Active Directory política IPsec de armazenamento no computador.|
|5457|N/D|Baixo|O PAStore Engine não pôde aplicar Active Directory política IPsec de armazenamento no computador.|
|5458|N/D|Baixo|O PAStore Engine aplicou uma cópia armazenada em cache localmente da política IPsec de armazenamento de Active Directory no computador.|
|5459|N/D|Baixo|O PAStore Engine não pôde aplicar a cópia armazenada em cache localmente da política IPsec de armazenamento de Active Directory no computador.|
|5460|N/D|Baixo|O PAStore Engine aplicou a política IPsec de armazenamento do Registro local no computador.|
|5461|N/D|Baixo|O PAStore Engine não pôde aplicar a política IPsec de armazenamento do Registro local no computador.|
|5462|N/D|Baixo|O PAStore Engine não pôde aplicar algumas regras da política IPsec ativa no computador. Use o snap-in Monitor de segurança IP para diagnosticar o problema.|
|5463|N/D|Baixo|O PAStore Engine pesquisou as alterações na política de IPsec ativa e não detectou nenhuma alteração.|
|5464|N/D|Baixo|O PAStore Engine pesquisou as alterações na política de IPsec ativa, detectou alterações e as aplicou aos serviços IPsec.|
|5465|N/D|Baixo|O PAStore Engine recebeu um controle para o recarregamento forçado da política IPsec e processou o controle com êxito.|
|5466|N/D|Baixo|O PAStore Engine pesquisou as alterações feitas na política IPsec Active Directory, determinou que Active Directory não pode ser acessado e usará a cópia armazenada em cache da política de IPsec do Active Directory em vez disso. Não foi possível aplicar as alterações feitas na política IPsec Active Directory desde que a última sondagem não pôde ser aplicada.|
|5467|N/D|Baixo|O PAStore Engine pesquisou as alterações feitas na política IPsec Active Directory, determinou que Active Directory pode ser acessado e não encontrou nenhuma alteração na política. A cópia armazenada em cache da política IPsec Active Directory não está mais sendo usada.|
|5468|N/D|Baixo|O PAStore Engine pesquisou as alterações feitas na política IPsec Active Directory, determinou que Active Directory pode ser alcançado, encontrou alterações na política e aplicou essas alterações. A cópia armazenada em cache da política IPsec Active Directory não está mais sendo usada.|
|5471|N/D|Baixo|O PAStore Engine carregou a política IPsec de armazenamento local no computador.|
|5472|N/D|Baixo|O PAStore Engine não pôde carregar a política IPsec de armazenamento local no computador.|
|5473|N/D|Baixo|O PAStore Engine carregou a política IPsec de armazenamento de diretório no computador.|
|5474|N/D|Baixo|O PAStore Engine não pôde carregar a política IPsec de armazenamento de diretório no computador.|
|5477|N/D|Baixo|O PAStore Engine não pôde adicionar o filtro de modo rápido.|
|5479|N/D|Baixo|Os serviços IPsec foram desligados com êxito. O desligamento de serviços IPsec pode colocar o computador em um risco maior de ataque de rede ou expor o computador a possíveis riscos de segurança.|
|5632|N/D|Baixo|Foi feita uma solicitação para autenticar em uma rede sem fio.|
|5633|N/D|Baixo|Foi feita uma solicitação para autenticar em uma rede com fio.|
|5712|N/D|Baixo|Uma chamada de procedimento remoto (RPC) foi tentada.|
|5888|N/D|Baixo|Um objeto no catálogo COM+ foi modificado.|
|5889|N/D|Baixo|Um objeto foi excluído do catálogo COM+.|
|5890|N/D|Baixo|Um objeto foi adicionado ao catálogo COM+.|
|6008|N/D|Baixo|O desligamento anterior do sistema era inesperado|
|6144|N/D|Baixo|A política de segurança no Política de Grupo objetos foi aplicada com êxito.|
|6272|N/D|Baixo|O servidor de políticas de rede concedeu acesso a um usuário.|
|N/D|561|Baixo|Um identificador para um objeto foi solicitado.|
|N/D|563|Baixo|Objeto aberto para exclusão|
|N/D|625|Baixo|Tipo de conta de usuário alterado|
|N/D|613|Baixo|Agente de política IPsec iniciado|
|N/D|614|Baixo|Agente de política IPsec desabilitado|
|N/D|615|Baixo|Agente de política IPsec|
|N/D|616|Baixo|O agente de política IPsec encontrou uma falha grave potencial|
|24577|N/D|Baixo|Criptografia do volume iniciada|
|24578|N/D|Baixo|Criptografia do volume interrompida|
|24579|N/D|Baixo|Criptografia do volume concluída|
|24580|N/D|Baixo|Descriptografia do volume iniciada|
|24581|N/D|Baixo|Descriptografia do volume interrompida|
|24582|N/D|Baixo|Descriptografia do volume concluída|
|24583|N/D|Baixo|Thread de trabalho de conversão para o volume iniciado|
|24584|N/D|Baixo|Thread de trabalho de conversão para o volume temporariamente parado|
|24588|N/D|Baixo|A operação de conversão no volume %2 encontrou um erro de setor insatisfatório. Valide os dados neste volume|
|24595|N/D|Baixo|O volume %2 contém clusters inválidos. Esses clusters serão ignorados durante a conversão.|
|24621|N/D|Baixo|Verificação de estado inicial: transação de conversão de volume sem interrupção em %2.|
|5049|N/D|Baixo|Uma associação de segurança IPsec foi excluída.|
|5478|N/D|Baixo|Os serviços IPsec foram iniciados com êxito.|

> [!NOTE]
> Consulte [eventos de auditoria de segurança do Windows](https://www.microsoft.com/download/details.aspx?id=50034) para obter uma lista de muitas IDs de eventos de segurança e seus significados.
>
> Execute **wevtutil GP Microsoft-Windows-Security-Auditing/GE/GM: true** para obter uma listagem muito detalhada de todas as IDs de evento de segurança

Para obter mais informações sobre as IDs de eventos de segurança do Windows e seus significados, consulte o artigo Suporte da Microsoft [Descrição dos eventos de segurança no Windows 7 e no Windows Server 2008 R2](https://support.microsoft.com/kb/977519). Você também pode baixar [eventos de auditoria de segurança para os detalhes do evento de segurança do Windows 7 e do Windows server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) e do Windows [8 e do Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que fornecem informações detalhadas de eventos para os sistemas operacionais referenciados no formato de planilha.
