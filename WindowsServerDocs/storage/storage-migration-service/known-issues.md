---
title: Problemas conhecidos do serviço de migração de armazenamento
description: Problemas conhecidos e suporte de solução de problemas para o serviço de migração de armazenamento, por exemplo, como coletar logs para Suporte da Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 150c9f1e70df4f634886ea65efd9c61ef075f26a
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940699"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conhecidos do serviço de migração de armazenamento

Este tópico contém respostas a problemas conhecidos ao usar o [serviço de migração de armazenamento](overview.md) para migrar servidores.

O serviço de migração de armazenamento é lançado em duas partes: o serviço no Windows Server e a interface do usuário no centro de administração do Windows. O serviço está disponível no Windows Server, no canal de manutenção de longo prazo, bem como no Windows Server, canal semestral; enquanto o centro de administração do Windows está disponível como um download separado. Também incluímos periodicamente alterações em atualizações cumulativas para o Windows Server, lançadas por meio de Windows Update. 

Por exemplo, o Windows Server, versão 1903 inclui novos recursos e correções para o serviço de migração de armazenamento, que também estão disponíveis para o Windows Server 2019 e o Windows Server, versão 1809, instalando [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Como coletar arquivos de log ao trabalhar com Suporte da Microsoft

O serviço de migração de armazenamento contém logs de eventos para o serviço Orchestrator e o serviço de proxy. O servidor urchestrator sempre contém ambos os logs de eventos, e os servidores de destino com o serviço de proxy instalado contêm os logs de proxy. Esses logs estão localizados em:

- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService
- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService-proxy

Se você precisar reunir esses logs para exibição offline ou para enviar para Suporte da Microsoft, há um script do PowerShell de software livre disponível no GitHub:

 [Auxiliar do serviço de migração de armazenamento](https://aka.ms/smslogs) 

Examine o LEIAme para uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>O serviço de migração de armazenamento não aparece no centro de administração do Windows, a menos que o gerenciamento do Windows Server 2019

Ao usar a versão 1809 do centro de administração do Windows para gerenciar um Orchestrator do Windows Server 2019, você não vê a opção de ferramenta para o serviço de migração de armazenamento. 

A extensão de serviço de migração de armazenamento do Windows Admin Center é associada à versão somente para gerenciar o Windows Server 2019 versão 1809 ou sistemas operacionais posteriores. Se você usá-lo para gerenciar sistemas operacionais Windows Server mais antigos ou visualizações Insider, a ferramenta não será exibida. Esse comportamento é previsto no design. 

Para resolver, use ou atualize para o Windows Server 2019 Build 1809 ou posterior.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>O serviço de migração de armazenamento não permite que você escolha o IP estático na transferência

Ao usar a versão 0,57 da extensão de serviço de migração de armazenamento no centro de administração do Windows e você chegar à fase de transferência, não será possível selecionar um IP estático para um endereço. Você é forçado a usar o DHCP.

Para resolver esse problema, no centro de administração do Windows, procure por **configurações** > **extensões** para um alerta informando que o serviço de migração de armazenamento de versão atualizado 0.57.2 está disponível para instalação. Talvez seja necessário reiniciar a guia do navegador para o centro de administração do Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>A validação de transferência do serviço de migração de armazenamento falha com o erro "o acesso foi negado para a política de filtro de token no computador de destino"

Ao executar a validação de transferência, você receberá um erro "falha: Acesso negado para a política de filtro de token no computador de destino. " Isso ocorre mesmo que você tenha fornecido credenciais de administrador local corretas para os computadores de origem e de destino.

Esse problema é causado por um defeito de código no Windows Server 2019. O problema ocorrerá quando você estiver usando o computador de destino como um orquestrador de serviço de migração de armazenamento.

Para contornar esse problema, instale o serviço de migração de armazenamento em um computador com Windows Server 2019 que não seja o destino de migração pretendido e, em seguida, conecte-se a esse servidor com o centro de administração do Windows e execute a migração.

Corrigimos isso em uma versão posterior do Windows Server. Abra um caso de suporte por meio de [suporte da Microsoft](https://support.microsoft.com) para solicitar que uma backport dessa correção seja criada.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>O serviço de migração de armazenamento não está incluído na avaliação do Windows Server 2019 ou no Windows Server 2019 Essentials Edition

Ao usar o centro de administração do Windows para se conectar a uma versão de avaliação do Windows [server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou do windows Server 2019 Essentials, não há uma opção para gerenciar o serviço de migração de armazenamento. O serviço de migração de armazenamento também não está incluído em funções e recursos.

Esse problema é causado por um problema de manutenção na mídia de avaliação do Windows Server 2019 e do Windows Server 2019 Essentials. 

Para contornar esse problema para avaliação, instale uma versão comercial, MSDN, OEM ou licença de volume do Windows Server 2019 e não a ative. Sem a ativação, todas as edições do Windows Server operam no modo de avaliação por 180 dias. 

Corrigimos esse problema em uma versão posterior do Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>O serviço de migração de armazenamento atinge o tempo limite de download do CSV de erro de transferência

Ao usar o centro de administração do Windows ou o PowerShell para baixar o log CSV somente erros detalhados de operações de transferência, você recebe o erro:

 >   Log de transferência-Verifique se o compartilhamento de arquivos é permitido em seu firewall. : Esta operação de solicitação enviada para net. TCP://localhost: 28940/SMS/Service/1/Transfer não recebeu uma resposta dentro do tempo limite configurado (00:01:00). O tempo alocado para esta operação pode ter sido uma parte de um tempo limite maior. Isso pode ocorrer porque o serviço ainda está processando a operação ou porque o serviço não pôde enviar uma mensagem de resposta. Considere aumentar o tempo limite da operação (convertendo o canal/proxy para IContextChannel e definindo a propriedade OperationTimeout) e verifique se o serviço é capaz de se conectar ao cliente.

Esse problema é causado por um número muito grande de arquivos transferidos que não podem ser filtrados no tempo limite de um minuto padrão permitido pelo serviço de migração de armazenamento. 

Para contornar esse problema:

1. No computador do Orchestrator, edite o arquivo *%systemroot%\SMS\Microsoft.StorageMigration.Service.exe.config* usando o notepad. exe para alterar o "SendTimeout" de seu padrão de 1 minuto para 10 minutos

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie o serviço "serviço de migração de armazenamento" no computador do Orchestrator. 
3. No computador do Orchestrator, inicie regedit. exe
4. Localize e clique na seguinte subchave do Registro: 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. No menu Editar, aponte para Novo e clique em Valor DWORD. 
6. Digite "WcfOperationTimeoutInMinutes" para o nome do DWORD e pressione ENTER.
7. Clique com o botão direito do mouse em "WcfOperationTimeoutInMinutes" e clique em Modificar. 
8. Na caixa dados base, clique em "decimal"
9. Na caixa dados do valor, digite "10" e clique em OK.
10. Saia do editor do registro.
11. Tente baixar o arquivo CSV somente erros novamente. 

Pretendemos alterar esse comportamento em uma versão posterior do Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>A transferência falha ao migrar entre redes

Ao migrar para um computador de destino executado em uma rede diferente da origem, como uma instância de IaaS do Azure, a transferência não será concluída quando a origem estiver usando um endereço IP estático. 

Esse comportamento é por design, para evitar problemas de conectividade após a migração de usuários, aplicativos e scripts que se conectam por meio do endereço IP. Quando o endereço IP for movido do computador de origem antigo para o novo destino de destino, ele não corresponderá às novas informações de sub-rede de rede e, talvez, DNS e WINS.

Para solucionar esse problema, execute uma migração para um computador na mesma rede. Em seguida, mova esse computador para uma nova rede e reatribua suas informações de IP. Por exemplo, se estiver migrando para o Azure IaaS, primeiro migre para uma VM local e, em seguida, use migrações para Azure para mudar a VM para o Azure.  

Corrigimos esse problema em uma versão posterior do centro de administração do Windows. Agora, permitimos que você especifique migrações que não alteram as configurações de rede do servidor de destino. A extensão atualizada será listada aqui quando liberada. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avisos de validação para proxy de destino e privilégios administrativos de credencial

Ao validar um trabalho de transferência, você verá os seguintes avisos:

 > **A credencial tem privilégios administrativos.**
 > Aviso: A ação não está disponível remotamente.
 > **O proxy de destino está registrado.**
 > Aviso: O proxy de destino não foi encontrado.

Se você não tiver instalado o serviço de proxy de serviço de migração de armazenamento no computador de destino do Windows Server 2019 ou o computador de destino for o Windows Server 2016 ou o Windows Server 2012 R2, esse comportamento será por design. É recomendável migrar para um computador com Windows Server 2019 com o proxy instalado para melhorar significativamente o desempenho da transferência.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Determinados arquivos não fazem inventário ou transferência, erro 5 "acesso negado"

Ao inventariar ou transferir arquivos da origem para os computadores de destino, os arquivos dos quais um usuário removeu as permissões do grupo de administradores falharão ao migrar. Examinando o serviço de migração de armazenamento-depuração de proxy mostra:

  Nome do Log:      Microsoft-Windows-StorageMigrationService-proxy/depurar origem:        Microsoft-Windows-StorageMigrationService-data do proxy:          ID do evento 2/26/2019 9:00:04 AM:      10000 categoria da tarefa: Nível de nenhum:         Palavras-chave de erro:      
  Usuário:          Computador do serviço de rede: srv1.contoso.com descrição:

  02/26/2019-09:00:04.860 [erro] erro de transferência para \\srv1. contoso. com\public\indy.png: (5) acesso negado.
Rastreamento de pilha: em Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode ShareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) em Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. gettargetfile (caminho da cadeia de caracteres) em Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. gettargetfile (arquivo FileInfo) em Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. InitializeSourceFileInfo () em Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. Transfer () em Microsoft. StorageMigration. proxy. Service. Transfer. FileTransfer. TryTransfer () [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs:: TryTransfer:: 55]


Esse problema é causado por um defeito de código no serviço de migração de armazenamento em que o privilégio de backup não estava sendo invocado. 

Para resolver esse problema, instale [Windows Update 2 de abril de 2019 — KB4490481 (Build do sistema operacional 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) no computador do Orchestrator e no computador de destino se o serviço de proxy estiver instalado lá. Verifique se a conta de usuário de migração de origem é um administrador local no computador de origem e o orquestrador do serviço de migração de armazenamento. Verifique se a conta de usuário de migração de destino é um administrador local no computador de destino e o orquestrador do serviço de migração de armazenamento. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Hashes DFSR incompatíveis ao usar o serviço de migração de armazenamento para propagar dados

Ao usar o serviço de migração de armazenamento para transferir arquivos para um novo destino e, em seguida, configurar o Replicação do DFS (DFSR) para replicar esses dados com um servidor DFSR existente por meio de replicação pré-propagada ou clonagem de banco de dado DFSR, todos os arquivos experiemce um hash incompatibilidade e rereplicado. Os fluxos de dados, os fluxos de segurança, os tamanhos e os atributos parecem ser perfeitamente correspondidos depois de usar o SMS para transferi-los. Examinar os arquivos com ICACLS ou o log de depuração de clonagem de banco de dados DFSR revela:

Arquivo de origem:

  d:\test\Source de icacls:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; UNIDADES

Arquivo de destino:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S:PAINO_ACCESS_CONTROL**

Log de depuração DFSR:

  20190308 10:18:53.116 3948 DBCL 4045 [WARN] DBClone:: IDTableImportUpdate registro de incompatibilidade foi encontrado. 

  Hash de ACL local: 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Atributos: 32 

  Clonar hash de ACL:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Atributos: 32 

Esse problema é causado por um defeito de código em uma biblioteca usada pelo serviço de migração de armazenamento para definir SACL (ACLs de auditoria de segurança). Uma SACL não nula não é definida intencionalmente quando a SACL estava vazia, liderando o DFSR para identificar corretamente uma incompatibilidade de hash. 

Para solucionar esse problema, continue usando o Robocopy para [as operações de pré-propagação de DFSR e clonagem de banco de dados DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) em vez do serviço de migração de armazenamento. Estamos investigando esse problema e pretendemos resolver isso em uma versão mais recente do Windows Server e, possivelmente, em uma Windows Update com backport. 

## <a name="error-404-when-downloading-csv-logs"></a>Erro 404 ao baixar logs CSV

Ao tentar baixar os logs de transferência ou de erro no final de uma operação de transferência, você recebe o erro:

  $jobname: Log de transferência: erro do AJAX 404

Esse erro será esperado se você não tiver habilitado a regra de firewall "compartilhamento de arquivos e impressoras (SMB-in)" no servidor do Orchestrator. Os downloads de arquivos do centro de administração do Windows exigem a porta TCP/445 (SMB) em computadores conectados.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Erro "não foi possível transferir o armazenamento em nenhum dos pontos de extremidade" ao transferir do Windows Server 2008 R2

Ao tentar transferir dados de um computador de origem do Windows Server 2008 R2, não há transferências de dados e você recebe o erro:  

  Não foi possível transferir o armazenamento em nenhum dos pontos de extremidade.
0x9044

Esse erro será esperado se o computador Windows Server 2008 R2 não tiver sido totalmente corrigido com todas as atualizações críticas e importantes do Windows Update. Independentemente do serviço de migração de armazenamento, sempre recomendamos aplicar patches em um computador com Windows Server 2008 R2 para fins de segurança, pois esse sistema operacional não contém as melhorias de segurança das versões mais recentes do Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erro "não foi possível transferir o armazenamento em nenhum dos pontos de extremidade" e "Verifique se o dispositivo de origem está online-não foi possível acessá-lo."

Ao tentar transferir dados de um computador de origem, alguns ou todos os compartilhamentos não são transferidos, com erro de Resumo:

   Não foi possível transferir o armazenamento em nenhum dos pontos de extremidade.
0x9044

Examinar os detalhes da transferência SMB mostra o erro:

   Verifique se o dispositivo de origem está online-não foi possível acessá-lo.

Examinar o log de eventos de StorageMigrationService/admin mostra:

   Não foi possível transferir o armazenamento.

   Trabalho ID do Job1:  
   Estado: Erro com falha: 36931 mensagem de erro: 

   Diretrizes: Verifique o erro detalhado e verifique se os requisitos de transferência foram atendidos. O trabalho de transferência não pôde transferir nenhum computador de origem e de destino. Isso pode ser devido ao fato de o computador Orchestrator não conseguir acessar os computadores de origem ou de destino, possivelmente devido a uma regra de firewall ou permissões ausentes.

Examinar o log StorageMigrationService-proxy/Debug mostra:

   07/02/2019-13:35:57.231 [erro] falha na validação da transferência. ErrorCode 40961, o ponto de extremidade de origem não está acessível ou não existe ou as credenciais de origem são inválidas ou o usuário autenticado não tem permissões suficientes para acessá-lo.
em Microsoft. StorageMigration. proxy. Service. Transfer. TransferOperation. Validate () em Microsoft. StorageMigration. proxy. Service. Transfer. TransferRequestHandler. ProcessRequest (FileTransferRequest fileTransferRequest, GUID operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Esse erro será esperado se sua conta de migração não tiver pelo menos permissões de acesso de leitura para os compartilhamentos SMB. Para solucionar esse erro, adicione um grupo de segurança que contém a conta de migração de origem aos compartilhamentos SMB no computador de origem e conceda a ele leitura, alteração ou controle total. Após a conclusão da migração, você poderá remover esse grupo.

## <a name="error-0x80005000-when-running-inventory"></a>Erro 0x80005000 ao executar o inventário

Depois de instalar o [KB4512534](https://support.microsoft.com/en-us/help/4512534/windows-10-update-kb4512534) e tentar executar o inventário, o inventário falhará com erros:

  EXCEÇÃO DE HRESULT: 0x80005000
  
  Nome do Log:      Origem Microsoft-Windows-StorageMigrationService/admin:        Microsoft-Windows-StorageMigrationService Data:          ID do evento 9/9/2019 5:21:42 PM:      2503 categoria da tarefa: Nível de nenhum:         Palavras-chave de erro:      
  Usuário:          Computador do serviço de rede:      FS02. Descrição do TailwindTraders.net: Não foi possível inventariar os computadores.
Trabalho: ID de Foo2: Estado de 20ac3f75-4945-41d1-9a79-d11dbb57798b: Erro com falha: 36934 mensagem de erro: Diretrizes de inventário com falha para todos os dispositivos: Verifique o erro detalhado e verifique se os requisitos de inventário foram atendidos. O trabalho não pôde inventariar nenhum dos computadores de origem especificados. Isso pode ocorrer porque o computador Orchestrator não conseguiu acessá-lo pela rede, possivelmente devido a uma regra de firewall ou a permissões ausentes.
  
  Nome do Log:      Origem Microsoft-Windows-StorageMigrationService/admin:        Microsoft-Windows-StorageMigrationService Data:          ID do evento 9/9/2019 5:21:42 PM:      2509 categoria da tarefa: Nível de nenhum:         Palavras-chave de erro:      
  Usuário:          Computador do serviço de rede:      FS02. Descrição do TailwindTraders.net: Não foi possível inventariar um computador.
Trabalho: computador Foo2: FS01. Estado de TailwindTraders.net: Erro com falha:-2147463168 mensagem de erro: Diretrizes: Verifique o erro detalhado e verifique se os requisitos de inventário foram atendidos. O inventário não pôde determinar os aspectos do computador de origem especificado. Isso pode ser devido à falta de permissões ou privilégios na origem ou em uma porta de firewall bloqueada.
  
Esse erro é causado por um defeito de código no serviço de migração de armazenamento quando você fornece credenciais de migração na forma de um UPN (nome principal do usuário)meghan@contoso.com, como ' '. O serviço Orchestrator do serviço de migração de armazenamento não analisa corretamente esse formato, o que leva a uma falha em uma pesquisa de domínio que foi adicionada para suporte de migração de cluster em KB4512534 e 19H1.

Para solucionar esse problema, forneça credenciais no formato domínio \ usuário, como ' Contoso\Meghan '.

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Erro "ServiceError0x9006" ou "o proxy não está disponível no momento". ao migrar para um cluster de failover do Windows Server

Ao tentar transferir dados para um servidor de arquivos clusterizado, você recebe erros como: 

   Verifique se o serviço de proxy está instalado e em execução e tente novamente. O proxy não está disponível no momento.
0x9006 ServiceError0x9006, Microsoft. StorageMigration. Commands. UnregisterSmsProxyCommand

Esse erro será esperado se o recurso de servidor de arquivos movido de seu nó original do proprietário do cluster do Windows Server 2019 para um novo nó e o recurso de proxy de serviço de migração de armazenamento não tiver sido instalado nesse nó.

Como alternativa, mova o recurso de servidor de arquivos de destino de volta para o nó de cluster do proprietário original que estava em uso quando você configurou os emparelhamentos de transferência pela primeira vez.

Como alternativa alternativa:

1. Instale o recurso de proxy de serviço de migração de armazenamento em todos os nós em um cluster.
2. Execute o seguinte comando do PowerShell do serviço de migração de armazenamento no computador do Orchestrator: 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Erro "dll não encontrado" ao executar o inventário de um nó de cluster

Ao tentar executar o inventário com o serviço de migração de armazenamento Orchestrator instalado em um nó de cluster de failover do Windows Server 2019 e direcionar a origem do servidor de arquivos de uso geral do cluster de failover do Windows Server, você receberá o seguinte erro:

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

Para solucionar esse problema, instale o "ferramentas de gerenciamento de cluster de failover" (RSAT-clustering-MGMT) no servidor que executa o Orchestrator do serviço de migração de armazenamento. 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>Erro "não há mais pontos de extremidade disponíveis no mapeador de ponto de extremidades" ao executar o inventário em um computador de origem do Windows Server 2003

Ao tentar executar o inventário com o servidor do Orchestrator do serviço de migração de armazenamento corrigido com a atualização cumulativa do [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou posterior, você receberá o seguinte erro:

    There are no more endpoints available from the endpoint mapper  

Para solucionar esse problema, desinstale temporariamente a atualização cumulativa KB4512534 (e qualquer uma que o tenha substituído) do computador Orchestrator do serviço de migração de armazenamento. Quando a migração for concluída, reinstale a atualização cumulativa mais recente.  

## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
