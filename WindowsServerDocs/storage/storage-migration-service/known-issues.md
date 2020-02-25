---
title: Problemas conhecidos do serviço de migração de armazenamento
description: Problemas conhecidos e suporte de solução de problemas para o serviço de migração de armazenamento, por exemplo, como coletar logs para Suporte da Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/10/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 92742929e3826fca3cf87cb84341d3aecec0d55d
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517491"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conhecidos do serviço de migração de armazenamento

Este tópico contém respostas a problemas conhecidos ao usar o [serviço de migração de armazenamento](overview.md) para migrar servidores.

O serviço de migração de armazenamento é lançado em duas partes: o serviço no Windows Server e a interface do usuário no centro de administração do Windows. O serviço está disponível no Windows Server, no canal de manutenção de longo prazo, bem como no Windows Server, canal semestral; enquanto o centro de administração do Windows está disponível como um download separado. Também incluímos periodicamente alterações em atualizações cumulativas para o Windows Server, lançadas por meio de Windows Update. 

Por exemplo, o Windows Server, versão 1903 inclui novos recursos e correções para o serviço de migração de armazenamento, que também estão disponíveis para o Windows Server 2019 e o Windows Server, versão 1809, instalando [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Como coletar arquivos de log ao trabalhar com Suporte da Microsoft

O serviço de migração de armazenamento contém logs de eventos para o serviço Orchestrator e o serviço de proxy. O servidor Orchestrator sempre contém ambos os logs de eventos, e os servidores de destino com o serviço de proxy instalado contêm os logs de proxy. Esses logs estão localizados em:

- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService
- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService-proxy

Se você precisar reunir esses logs para exibição offline ou para enviar para Suporte da Microsoft, há um script do PowerShell de software livre disponível no GitHub:

 [Auxiliar do serviço de migração de armazenamento](https://aka.ms/smslogs) 

Examine o LEIAme para uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>O serviço de migração de armazenamento não aparece no centro de administração do Windows, a menos que o gerenciamento do Windows Server 2019

Ao usar a versão 1809 do centro de administração do Windows para gerenciar um Orchestrator do Windows Server 2019, você não vê a opção de ferramenta para o serviço de migração de armazenamento. 

A extensão de serviço de migração de armazenamento do Windows Admin Center é associada à versão somente para gerenciar o Windows Server 2019 versão 1809 ou sistemas operacionais posteriores. Se você usá-lo para gerenciar sistemas operacionais Windows Server mais antigos ou visualizações Insider, a ferramenta não será exibida. Esse comportamento é previsto no design. 

Para resolver, use ou atualize para o Windows Server 2019 Build 1809 ou posterior.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>A validação de transferência do serviço de migração de armazenamento falha com o erro "o acesso foi negado para a política de filtro de token no computador de destino"

Ao executar a validação de transferência, você receberá o erro "falha: acesso negado para a política de filtro de token no computador de destino". Isso ocorre mesmo que você tenha fornecido credenciais de administrador local corretas para os computadores de origem e de destino.

Esse problema foi corrigido na atualização do [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . 

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>O serviço de migração de armazenamento não está incluído na avaliação do Windows Server 2019 ou no Windows Server 2019 Essentials Edition

Ao usar o centro de administração do Windows para se conectar a uma versão de avaliação do Windows [server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou do windows Server 2019 Essentials, não há uma opção para gerenciar o serviço de migração de armazenamento. O serviço de migração de armazenamento também não está incluído em funções e recursos.

Esse problema é causado por um problema de manutenção na mídia de avaliação do Windows Server 2019 e do Windows Server 2019 Essentials. 

Para contornar esse problema para avaliação, instale uma versão comercial, MSDN, OEM ou licença de volume do Windows Server 2019 e não a ative. Sem a ativação, todas as edições do Windows Server operam no modo de avaliação por 180 dias. 

Corrigimos esse problema em uma versão posterior do Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>O serviço de migração de armazenamento atinge o tempo limite de download do CSV de erro de transferência

Ao usar o centro de administração do Windows ou o PowerShell para baixar o log CSV somente erros detalhados de operações de transferência, você recebe o erro:

 >   Log de transferência-Verifique se o compartilhamento de arquivos é permitido em seu firewall. : Esta operação de solicitação enviada para net. TCP://localhost: 28940/SMS/Service/1/Transfer não recebeu uma resposta dentro do tempo limite configurado (00:01:00). O tempo alocado para esta operação pode ter sido uma parte de um tempo limite maior. Isso pode ocorrer porque o serviço ainda está processando a operação ou porque o serviço não pôde enviar uma mensagem de resposta. Considere aumentar o tempo limite da operação (convertendo o canal/proxy para IContextChannel e definindo a propriedade OperationTimeout) e verifique se o serviço é capaz de se conectar ao cliente.

Esse problema é causado por um número muito grande de arquivos transferidos que não podem ser filtrados no tempo limite de um minuto padrão permitido pelo serviço de migração de armazenamento. 

Para contornar este problema:

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

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avisos de validação para proxy de destino e privilégios administrativos de credencial

Ao validar um trabalho de transferência, você verá os seguintes avisos:

 > **A credencial tem privilégios administrativos.**
 > Aviso: a ação não está disponível remotamente.
 > **O proxy de destino está registrado.**
 > Aviso: o proxy de destino não foi encontrado.

Se você não tiver instalado o serviço de proxy de serviço de migração de armazenamento no computador de destino do Windows Server 2019 ou o computador de destino for o Windows Server 2016 ou o Windows Server 2012 R2, esse comportamento será por design. É recomendável migrar para um computador com Windows Server 2019 com o proxy instalado para melhorar significativamente o desempenho da transferência.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Determinados arquivos não fazem inventário ou transferência, erro 5 "acesso negado"

Ao inventariar ou transferir arquivos da origem para os computadores de destino, os arquivos dos quais um usuário removeu as permissões do grupo de administradores falharão ao migrar. Examinando o serviço de migração de armazenamento-depuração de proxy mostra:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/26/2019 9:00:04 AM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      srv1.contoso.com
    Description:

    02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
    Stack Trace:
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()   

Esse problema é causado por um defeito de código no serviço de migração de armazenamento em que o privilégio de backup não estava sendo invocado. 

Para resolver esse problema, instale [Windows Update 2 de abril de 2019 — KB4490481 (Build do sistema operacional 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) no computador do Orchestrator e no computador de destino se o serviço de proxy estiver instalado lá. Verifique se a conta de usuário de migração de origem é um administrador local no computador de origem e o orquestrador do serviço de migração de armazenamento. Verifique se a conta de usuário de migração de destino é um administrador local no computador de destino e o orquestrador do serviço de migração de armazenamento. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Hashes DFSR incompatíveis ao usar o serviço de migração de armazenamento para propagar dados

Ao usar o serviço de migração de armazenamento para transferir arquivos para um novo destino, em seguida, configurar o Replicação do DFS (DFSR) para replicar esses dados com um servidor DFSR existente por meio de replicação pré-propagada ou clonagem de banco de dados DFSR, todos os arquivos terão um hash incompatibilidade e rereplicado. Os fluxos de dados, os fluxos de segurança, os tamanhos e os atributos parecem ser perfeitamente correspondidos depois de usar o SMS para transferi-los. Examinar os arquivos com ICACLS ou o log de depuração de clonagem de banco de dados DFSR revela:

Arquivo de origem:

  d:\test\Source de icacls:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; UNIDADES

Arquivo de destino:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S: PAINO_ACCESS_CONTROL**

Log de depuração DFSR:

    20190308 10:18:53.116 3948 DBCL  4045 [WARN] DBClone::IDTableImportUpdate Mismatch record was found. 

    Local ACL hash:1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

    Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

Esse problema é corrigido pela atualização do [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Erro "não foi possível transferir o armazenamento em nenhum dos pontos de extremidade" ao transferir do Windows Server 2008 R2

Ao tentar transferir dados de um computador de origem do Windows Server 2008 R2, não há transferências de dados e você recebe o erro:  

    Couldn't transfer storage on any of the endpoints.
    0x9044

Esse erro será esperado se o computador Windows Server 2008 R2 não tiver sido totalmente corrigido com todas as atualizações críticas e importantes do Windows Update. Independentemente do serviço de migração de armazenamento, sempre recomendamos aplicar patches em um computador com Windows Server 2008 R2 para fins de segurança, pois esse sistema operacional não contém as melhorias de segurança das versões mais recentes do Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erro "não foi possível transferir o armazenamento em nenhum dos pontos de extremidade" e "Verifique se o dispositivo de origem está online-não foi possível acessá-lo."

Ao tentar transferir dados de um computador de origem, alguns ou todos os compartilhamentos não são transferidos, com erro de Resumo:

    Couldn't transfer storage on any of the endpoints.
    0x9044

Examinar os detalhes da transferência SMB mostra o erro:

    Check if the source device is online - we couldn't access it.

Examinar o log de eventos de StorageMigrationService/admin mostra:

    Couldn't transfer storage.

    Job: Job1
    ID:  
    State: Failed
    Error: 36931
    Error Message: 

   Orientação: Verifique o erro detalhado e verifique se os requisitos de transferência foram atendidos. O trabalho de transferência não pôde transferir nenhum computador de origem e de destino. Isso pode ser devido ao fato de o computador Orchestrator não conseguir acessar os computadores de origem ou de destino, possivelmente devido a uma regra de firewall ou permissões ausentes.

Examinar o log StorageMigrationService-proxy/Debug mostra:

    07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)    

Esse era um defeito de código que seria manifestado se sua conta de migração não tiver pelo menos permissões de leitura para os compartilhamentos SMB. Esse problema foi corrigido primeiro na atualização cumulativa [4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062). 

## <a name="error-0x80005000-when-running-inventory"></a>Erro 0x80005000 ao executar o inventário

Depois de instalar o [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) e tentar executar o inventário, o inventário falhará com erros:

    EXCEPTION FROM HRESULT: 0x80005000
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2503
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory the computers.
    Job: foo2
    ID: 20ac3f75-4945-41d1-9a79-d11dbb57798b
    State: Failed
    Error: 36934
    Error Message: Inventory failed for all devices
    Guidance: Check the detailed error and make sure the inventory requirements are met. The job couldn't inventory any of the specified source computers. This could be because the orchestrator computer couldn't reach it over the network, possibly due to a firewall rule or missing permissions.
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2509
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory a computer.
    Job: foo2
    Computer: FS01.TailwindTraders.net
    State: Failed
    Error: -2147463168
    Error Message: 
    Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.
  
    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/14/2020 1:18:21 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      2019-rtm-orc.ned.contoso.com
    Description:
    02/14/2020-13:18:21.097 [Erro] Failed device discovery stage SystemInfo with error: (0x80005000) Unknown error (0x80005000)   
  
Esse erro é causado por um defeito de código no serviço de migração de armazenamento quando você fornece credenciais de migração na forma de um UPN (nome principal do usuário), como 'meghan@contoso.com'. O serviço Orchestrator do serviço de migração de armazenamento não analisa corretamente esse formato, o que leva a uma falha em uma pesquisa de domínio que foi adicionada para suporte de migração de cluster em KB4512534 e 19H1.

Para solucionar esse problema, forneça credenciais no formato domínio \ usuário, como ' Contoso\Meghan '.

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Erro "ServiceError0x9006" ou "o proxy não está disponível no momento". ao migrar para um cluster de failover do Windows Server

Ao tentar transferir dados para um servidor de arquivos clusterizado, você recebe erros como: 

    Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
    0x9006
    ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand

Esse erro será esperado se o recurso de servidor de arquivos movido de seu nó original do proprietário do cluster do Windows Server 2019 para um novo nó e o recurso de proxy de serviço de migração de armazenamento não tiver sido instalado nesse nó.

Como alternativa, mova o recurso de servidor de arquivos de destino de volta para o nó de cluster do proprietário original que estava em uso quando você configurou os emparelhamentos de transferência pela primeira vez.

Como alternativa alternativa:

1. Instale o recurso de proxy de serviço de migração de armazenamento em todos os nós em um cluster.
2. Execute o seguinte comando do PowerShell do serviço de migração de armazenamento no computador do Orchestrator: 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Erro "dll não encontrado" ao executar o inventário de um nó de cluster

Ao tentar executar o inventário com o serviço de migração de armazenamento e direcionar para uma fonte de servidor de arquivos de uso geral do cluster de failover do Windows Server, você receberá os seguintes erros:

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

Para solucionar esse problema, instale o "ferramentas de gerenciamento de cluster de failover" (RSAT-clustering-MGMT) no servidor que executa o Orchestrator do serviço de migração de armazenamento. 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>Erro "não há mais pontos de extremidade disponíveis no mapeador de ponto de extremidades" ao executar o inventário em um computador de origem do Windows Server 2003

Ao tentar executar o inventário com o servidor do Orchestrator do serviço de migração de armazenamento corrigido com a atualização cumulativa do [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou posterior, você receberá o seguinte erro:

    There are no more endpoints available from the endpoint mapper  

Para solucionar esse problema, desinstale temporariamente a atualização cumulativa KB4512534 (e qualquer uma que o tenha substituído) do computador Orchestrator do serviço de migração de armazenamento. Quando a migração for concluída, reinstale a atualização cumulativa mais recente.  

Observe que, em algumas circunstâncias, a desinstalação de KB4512534 ou de suas atualizações substitutas pode fazer com que o serviço de migração de armazenamento não seja mais iniciado. Para resolver esse problema, você pode fazer backup e excluir o banco de dados do serviço de migração de armazenamento:

1.  Abra um prompt cmd elevado, no qual você é membro de administradores no servidor Orchestrator do serviço de migração de armazenamento e execute:

     ```
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  Inicie o serviço de serviço de migração de armazenamento, que criará um novo banco de dados.

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>Erro "falha na CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO em relação ao recurso do NetName" e a transferência de cluster do Windows Server 2008 R2 falha

Ao tentar executar o corte de uma origem de cluster do Windows Server 2008 R2, a sobreCorte fica presa na fase "renomeando o computador de origem..." e você receberá o seguinte erro:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

Esse problema é causado por uma API ausente em versões mais antigas do Windows Server. Atualmente, não há como migrar clusters do Windows Server 2008 e do Windows Server 2003. Você pode executar o inventário e a transferência sem problemas em clusters do Windows Server 2008 R2 e, em seguida, executar a transferência manualmente alterando manualmente o recurso de servidor de arquivos de origem do cluster e o endereço IP e, em seguida, alterando o IP e o cluster de destino de NetName Endereço para corresponder à fonte original. 

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>A transferência trava em "38% mapeando interfaces de rede no computador de origem..." 

Ao tentar executar o recorte de um computador de origem, ter definido o computador de origem para usar um novo endereço IP estático (não DHCP) em uma ou mais interfaces de rede, o corte é paralisado na fase "38% mapeando interfaces de rede no comnputer de origem..." e você receberá o seguinte erro no log de eventos do SMS:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          11/13/2019 3:47:06 PM
    Event ID:      20494
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      orc2019-rtm.corp.contoso.com
    Description:
    Couldn't set the IP address on the network adapter.

    Computer: fs12.corp.contoso.com
    Adapter: microsoft hyper-v network adapter
    IP address: 10.0.0.99
    Network mask: 16
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

Examinar o computador de origem mostra que o endereço IP original não é alterado. 

Esse problema não ocorrerá se você selecionou "usar DHCP" na tela do centro de administração do Windows "configurar a transferência", somente se você especificar um novo endereço IP estático, sub-rede e gateway. 

Esse problema é causado por uma regressão na atualização do [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . Atualmente, há duas soluções alternativas para esse problema:

  - Antes de recortar: em vez de definir um novo endereço IP estático na transferência, selecione "usar DHCP" e certifique-se de que um escopo DHCP cubra essa sub-rede. O SMS configurará o computador de origem para usar o DHCP em interfaces do computador de origem e substituirá normalmente. 
  
  - Se a recorte já estiver paralisada: faça logon no computador de origem e habilite o DHCP em suas interfaces de rede, depois de garantir que um escopo DHCP cubra essa sub-rede. Quando o computador de origem adquire um endereço IP fornecido pelo DHCP, o SMS continuará com a recortar normalmente.
  
Em ambas as soluções alternativas, após a conclusão da recorte, você pode definir um endereço IP estático no computador de origem antigo, como você verá ajustar e parar de usar o DHCP.   

## <a name="slower-than-expected-re-transfer-performance"></a>Mais lento do que o esperado desempenho de retransferência

Depois de concluir uma transferência e, em seguida, executar uma retransferência subsequente dos mesmos dados, talvez você não veja muita melhoria no tempo de transferência, mesmo quando poucos dados forem alterados enquanto isso no servidor de origem.

Esse é o comportamento esperado ao transferir um número muito grande de arquivos e pastas aninhadas. O tamanho dos dados não é relevante. Primeiro, fizemos melhorias nesse comportamento no [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) e continuamos a otimizar o desempenho da transferência. Para ajustar ainda mais o desempenho, examine [otimizando o desempenho de inventário e transferência](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#optimizing-inventory-and-transfer-performance).

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>Os dados não são transferidos, o usuário renomeou ao migrar de ou para um controlador de domínio

Depois de iniciar a transferência de ou para um controlador de domínio:

 1. Nenhum dado é migrado e nenhum compartilhamento é criado no destino.
 2. Há um símbolo de erro vermelho mostrado no centro de administração do Windows sem mensagem de erro
 3. Um ou mais usuários do AD e grupos locais de domínio têm seu nome e/ou atributo de logon anterior ao Windows 2000 alterado
 4. Você verá o evento 3509 no Orchestrator do SMS:
 
        Log Name:      Microsoft-Windows-StorageMigrationService/Admin
        Source:        Microsoft-Windows-StorageMigrationService
        Date:          1/10/2020 2:53:48 PM
        Event ID:      3509
        Task Category: None
        Level:         Error
        Keywords:      
        User:          NETWORK SERVICE
        Computer:      orc2019-rtm.corp.contoso.com
        Description:
        Couldn't transfer storage for a computer.

        Job: dctest3
        Computer: dc02-2019.corp.contoso.com
        Destination Computer: dc03-2019.corp.contoso.com
        State: Failed
        Error: 53251
        Error Message: Local accounts migration failed with error System.Exception: -2147467259
           at Microsoft.StorageMigration.Service.DeviceHelper.MigrateSecurity(IDeviceRecord sourceDeviceRecord, IDeviceRecord destinationDeviceRecord, TransferConfiguration config, Guid proxyId, CancellationToken cancelToken)

Esse é o comportamento esperado se você tentou migrar do ou para um controlador de domínio com o serviço de migração de armazenamento e usou a opção "migrar usuários e grupos" para renomear ou reutilizar contas. em vez de selecionar "não transferir usuários e grupos". [Não há suporte para a migração de DC com o serviço de migração de armazenamento](faq.md). Como um controlador de domínio não tem usuários e grupos locais verdadeiros, o serviço de migração de armazenamento trata essas entidades de segurança como faria ao migrar entre dois servidores membros e tenta ajustar as ACLs conforme instruído, levando a erros e contas descontadas ou copiadas. 

Se você já executou a transferência uma ou mais vezes:

 1. Use o seguinte comando do PowerShell do AD em um DC para localizar usuários ou grupos modificados (alterando SearchBase para corresponder ao nome do dinstringuished do domínio): 

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```
   
 2. Para todos os usuários retornados com seu nome original, edite seu "nome de logon do usuário (anterior ao Windows 2000)" para remover o sufixo de caractere aleatório adicionado pelo serviço de migração de armazenamento, para que esse perdedor possa fazer logon.
 3. Para todos os grupos retornados com seu nome original, edite seu "nome de grupo (anterior ao Windows 2000)" para remover o sufixo de caractere aleatório adicionado pelo serviço de migração de armazenamento.
 4. Para todos os usuários ou grupos desabilitados com nomes que agora contêm um sufixo adicionado pelo serviço de migração de armazenamento, você pode excluir essas contas. Você pode confirmar que as contas de usuário foram adicionadas posteriormente, pois elas só conterão o grupo de usuários de domínio e terão uma data/hora de criação que corresponda à hora de início da transferência do serviço de migração de armazenamento.
 
 Se você quiser usar o serviço de migração de armazenamento com controladores de domínio para fins de transferência, certifique-se de sempre selecionar "não transferir usuários e grupos" na página Configurações de transferência no centro de administração do Windows.
 
 ## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>Erro 53, "falha ao inventariar todos os dispositivos especificados" ao executar o inventário, 

Ao tentar executar o inventário, você recebe:

    Failed to inventory all specified devices 
    
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          1/16/2020 8:31:17 AM
    Event ID:      2516
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    Couldn't inventory files on the specified endpoint.
    Job: ned1
    Computer: ned.corp.contoso.com
    Endpoint: hithere
    State: Failed
    File Count: 0
    File Size in KB: 0
    Error: 53
    Error Message: Endpoint scan failed
    Guidance: Check the detailed error and make sure the inventory requirements are met. This could be because of missing permissions on the source computer.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/16/2020 8:31:17 AM
    Event ID:      10004
    Task Category: None
    Level:         Critical
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    01/16/2020-08:31:17.031 [Crit] Consumer Task failed with error:The network path was not found.
    . StackTrace=   at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)
       at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetEnvironmentPathFolders(String ServerName, Boolean IsServerLocal)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.ScanUtils.<ScanSMBEndpoint>d__3.MoveNext()
       at Microsoft.StorageMigration.Proxy.EndpointScanOperation.Run()
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(EndpointScanRequest scanRequest, Guid operationId)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(Object request)
       at Microsoft.StorageMigration.Proxy.Common.ProducerConsumerManager`3.Consume(CancellationToken token)    
       
    01/16/2020-08:31:10.015 [Erro] Endpoint Scan failed. Error: (53) The network path was not found.
    Stack trace:
       at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)

Neste estágio, o orquestrador do serviço de migração de armazenamento está tentando ler o registro remoto para determinar a configuração do computador de origem, mas está sendo rejeitado pelo servidor de origem informando que o caminho do registro não existe. Isso pode ser causado por:

 - O serviço de registro remoto não está em execução no computador de origem.
 - o firewall não permite conexões de registro remoto para o servidor de origem do Orchestrator.
 - A conta de migração de origem não tem permissões de registro remoto para se conectar ao computador de origem.
 - A conta de migração de origem não tem permissões de leitura no registro do computador de origem, em "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\CurrentVersion" ou em "HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\ LanManServer

## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
