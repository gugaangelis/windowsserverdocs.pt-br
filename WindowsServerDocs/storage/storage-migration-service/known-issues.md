---
title: Problemas conhecidos do serviço de migração de armazenamento
description: Problemas conhecidos e solução de problemas de suporte para serviço de migração de armazenamento, por exemplo, como coletar logs do Microsoft Support.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 08156a09491d66016b5fcfe6056ed318d682b987
ms.sourcegitcommit: 514d659c3bcbdd60d1e66d3964ede87b85d79ca9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67735162"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conhecidos do serviço de migração de armazenamento

Este tópico contém respostas para problemas conhecidos ao usar [serviço de migração de armazenamento](overview.md) a migração de servidores.

## <a name="collecting-logs"></a> Como coletar arquivos de log ao trabalhar com o Microsoft Support

O serviço de migração de armazenamento contém logs de eventos para o serviço Orchestrator e o serviço de Proxy. O servidor urchestrator sempre contém os dois logs de eventos e os servidores de destino com o serviço de proxy instalado contêm os logs de proxy. Esses logs estão localizados em:

- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService
- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService Proxy

Se você precisar coletar esses logs para exibição offline ou para enviar ao Microsoft Support, há um script do PowerShell disponível no GitHub de software livre:

 [Auxiliar de serviço de migração de armazenamento](https://aka.ms/smslogs) 

Examine o arquivo Leiame para uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Serviço de migração de armazenamento não apareça no Windows Admin Center, a menos que o gerenciamento do Windows Server 2019

Ao usar a versão de 1809 do Windows Admin Center para gerenciar um orquestrador de 2019 do Windows Server, você não vir a opção de ferramenta para o serviço de migração de armazenamento. 

A extensão de serviço de migração de armazenamento do Windows Admin Center é associado com a versão para gerenciar somente a versão do Windows Server 2019 1809 ou sistemas operacionais posteriores. Se você usá-lo para gerenciar sistemas de operacionais mais antigos do Windows Server ou visualizações insider, a ferramenta não será exibida. Esse comportamento é padrão. 

Para resolver, usar ou atualizar para o Windows Server 2019 build 1809 ou posterior.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Serviço de migração de armazenamento não permitem que você escolha um IP estático na substituição

Ao usar a versão 0.57 do serviço de migração de armazenamento de extensão no Windows Admin Center e você atingir a fase de transferência do sistema, você não pode selecionar um endereço IP estático para um endereço. Você é forçado a usar o DHCP.

Para resolver esse problema, no do Windows Admin Center, procure **as configurações** > **extensões** para um alerta informando que o serviço de migração de armazenamento 0.57.2 a versão atualizada está disponível para instalação. Você talvez precise reiniciar o guia do navegador para o Centro de administração do Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Falha de validação de substituição de serviço de migração de armazenamento com o erro "Acesso negado para a política de filtro de token no computador de destino"

Ao executar a validação de substituição, você receberá o erro "Falha: Acesso negado para a política de filtro de token no computador de destino." Isso ocorre mesmo se você forneceu credenciais de administrador local correto para computadores de origem e de destino.

Esse problema é causado por um defeito de código no Windows Server 2019. O problema ocorrerá quando você estiver usando o computador de destino como um orquestrador de serviços de migração de armazenamento.

Para contornar esse problema, instale o serviço de migração de armazenamento em um computador Windows Server 2019 que não é o destino de migração pretendido, em seguida, se conectar ao servidor com Windows Admin Center e executar a migração.

Corrigimos isso em uma versão posterior do Windows Server. Abra um caso de suporte por meio [Microsoft Support](https://support.microsoft.com) para solicitar um backport dessa correção ser criado.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Serviço de migração de armazenamento não está incluído na edição de avaliação do Windows Server de 2019

Ao usar o Windows Admin Center para se conectar a um [versão de avaliação do Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) não existe uma opção para gerenciar o serviço de migração de armazenamento. Serviço de migração de armazenamento também não está incluído nas funções e recursos.

Esse problema é causado por um problema de manutenção na mídia de avaliação do Windows Server 2019. 

Para contornar esse problema, instale um varejo, a versão de licença de Volume, OEM ou MSDN do Windows Server 2019 e não ativá-lo. Sem ativação, todas as edições do Windows Server operam no modo de avaliação de 180 dias. 

Podemos corrigir esse problema em uma versão posterior do Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Serviço de migração de armazenamento atingir o tempo limite baixando o erro de transferência de CSV

Ao usar o Windows Admin Center ou PowerShell para baixar o operações detalhadas somente erros CSV log de transferência, você recebe o erro:

 >   Log de transferência - Verifique se o compartilhamento de arquivos é permitido em seu firewall. : Esta operação de solicitação enviada para baseaddress="NET.TCP://localhost:6080/vmmhelperservice/: 28940/sms/service/1/transferência não recebeu uma resposta dentro do tempo limite configurado (00: 01:00). O tempo alocado para essa operação pode ter sido uma parte de um tempo limite maior. Isso pode ser porque o serviço ainda está processando a operação ou porque o serviço não conseguiu enviar uma mensagem de resposta. Por favor, considere aumentar o tempo limite da operação (por conversão o canal/proxy para IContextChannel e definindo a propriedade OperationTimeout) e certifique-se de que o serviço é capaz de se conectar ao cliente.

Esse problema é causado por um número muito grande de arquivos transferidos que não podem ser filtrados em um minuto o tempo de limite padrão permitido pelo serviço de migração de armazenamento. 

Para contornar esse problema:

1. No computador do orchestrator, edite o *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* de arquivos usando o Notepad.exe para alterar o "sendTimeout" do seu padrão de 1 minuto para 10 minutos

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie o serviço de "Serviço de migração de armazenamento" no computador do orchestrator. 
3. No computador do orchestrator, inicie Regedit.exe
4. Localize e clique na seguinte subchave do Registro: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. No menu Editar, aponte para Novo e clique em Valor DWORD. 
6. Digite "WcfOperationTimeoutInMinutes" para o nome do DWORD e pressione ENTER.
7. Clique com botão direito "WcfOperationTimeoutInMinutes" e, em seguida, clique em modificar. 
8. Na caixa de Base de dados, clique em "Decimal"
9. Na caixa de dados de valor, digite "10" e, em seguida, clique em Okey.
10. Saia do Editor do registro.
11. Tentativa de baixar o arquivo CSV somente erros novamente. 

Pretendemos alterar esse comportamento em uma versão posterior do Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Falha de substituição durante a migração entre redes

Ao migrar para um computador de destino em execução em uma rede diferente que a fonte, como uma instância de IaaS do Azure, substituição não for concluída quando a fonte estava usando um endereço IP estático. 

Esse comportamento ocorre por design, para evitar problemas de conectividade após a migração de usuários, aplicativos e scripts de conexão por meio do endereço IP. Quando o endereço IP é movido do computador de origem antigo para o novo destino de destino, ele não corresponder a novas informações de sub-rede de rede e talvez o DNS e WINS.

Para solucionar esse problema é executar uma migração para um computador na mesma rede. Em seguida, mover o computador para uma nova rede e reatribuir suas informações de IP. Por exemplo, se migrando para o Azure IaaS, primeiro migrar para uma VM local e usar as migrações do Azure para deslocar a VM no Azure.  

Podemos corrigir esse problema em uma versão posterior do Windows Admin Center. Vamos agora vai permitem que você especifique as migrações que não alteram as configurações de rede do servidor de destino. A extensão atualizada será listada aqui quando lançado. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avisos de validação de privilégios administrativos proxy e as credenciais de destino

Ao validar um trabalho de transferência, você verá os seguintes avisos:

 > **A credencial possui privilégios administrativos.**
 > Aviso: Ação não está disponível remotamente.
 > **O proxy de destino está registrado.**
 > Aviso: O proxy de destino não foi encontrado.

Se você não tiver instalado o serviço de Proxy de serviço de migração de armazenamento no computador de destino de 2019 do Windows Server ou o computador de destinaton for Windows Server 2016 ou Windows Server 2012 R2, esse comportamento ocorre por design. É recomendável migrar para um computador Windows Server 2019 com o proxy instalado para um desempenho de transferência significativamente aprimorado.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Alguns arquivos não de estoque ou transferir, 5 de erro "acesso negado"

Quando inventariando ou transferência de arquivos de origem para computadores de destino, os arquivos dos quais um usuário removeu permissões do grupo de administradores falharem migrar. Examinando a depuração de Proxy de serviço de migração de armazenamento mostra:

  Nome do Log:      Microsoft-Windows-StorageMigrationService-Proxy/Debug fonte:        Microsoft-Windows-StorageMigrationService-Proxy Date:          2/26/2019:04 9:00 a ID de evento:      Categoria da tarefa 10000: Nenhum nível:         Palavras-chave de erro:      
  Usuário:          Computador do serviço de rede: descrição srv1.contoso.com:

  Erro de transferência 02/26/2019-09:00:04.860 [Erro] para \\srv1.contoso.com\public\indy.png: (5) o acesso é negado.
Rastreamento de pilha: Em Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile (cadeia de caracteres de nome de arquivo, DesiredAccess desiredAccess, modo de compartilhamento do modo de compartilhamento, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) em Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (caminho de cadeia de caracteres) em Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (arquivo FileInfo) em Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo() em Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() em Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


Esse problema é causado por um defeito de código no serviço de migração de armazenamento em que o privilégio de backup não foi que está sendo invocado. 

Para resolver esse problema, instale [Windows Update 2 de abril de 2019 — KB4490481 (SO Build 17763.404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) no computador com o orchestrator e o computador de destino se o serviço de proxy está instalado lá. Certifique-se de que a conta de usuário de migração de código-fonte é um administrador local no computador de origem e o orquestrador de serviço de migração de armazenamento. Certifique-se de que a conta de usuário de migração de destino é um administrador local no computador de destino e o orquestrador de serviço de migração de armazenamento. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Incompatibilidade de hashes DFSR ao usar o serviço de migração de armazenamento para pré-propagar dados

Ao usar o serviço de migração de armazenamento para transferir arquivos para um novo destino, em seguida, configurar a DFSR (replicação DFS) para replicar dados com um servidor DFSR existente por meio de replicação presseded ou banco de dados DFSR clonagem, todos os arquivos experiemce um hash incompatibilidade e são novamente replicadas. Os fluxos de dados, fluxos de segurança, tamanhos e atributos são exibidos a serem correspondidos perfeitamente depois de usar o SMS para transferi-las. Verificação de arquivos com ICACLS ou o log de depuração de clonagem de banco de dados DFSR revela:

Arquivo de origem:

  icacls d:\test\Source:

  d:\test\thatcher.png icacls/salvar txt /t thatcher.png D:AI(A;; FA;; BA) (A; 0 x1200a9;; 1!d D) (A; 0 x1301bf;; 1!d U) (A; ID; FA;; BA) (A; ID; FA;; SY) (A; ID; 0X1200A9;; BU)

Arquivo de destino:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1301bf;;;DU)(A;;0x1200a9;;;DD)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)**S:PAINO_ACCESS_CONTROL**

Log de depuração DFSR:

  20190308 10:18:53.116 3948 DBCL 4045 [aviso] incompatibilidade de DBClone::IDTableImportUpdate registro foi encontrado. 

  Hash ACL local: 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Atributos: 32 

  Clonar o hash ACL:**DDC4FCE4-DDF329C4 977CED6D F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 Atributos: 32 

Esse problema é causado por um defeito de código em uma biblioteca usada pelo serviço de migração de armazenamento para definir a auditoria de segurança de ACLs (SACL). Uma SACL de não-nulo, inadvertidamente, é definida quando a SACL estava vazia, levando a DFSR para identificar corretamente uma incompatibilidade de hash. 

Para solucionar esse problema, continue a usar o Robocopy para [DFSR pré-propagação e operações de clonagem de banco de dados do DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) em vez do serviço de migração de armazenamento. Estamos investigando esse problema e pretende resolver esse problema em uma versão posterior do Windows Server e possivelmente uma retrocompatibilizada Windows Update. 

## <a name="error-404-when-downloading-csv-logs"></a>Logs de erro 404 ao baixar CSV

Ao tentar baixar os logs de transferência ou de erro no final de uma operação de transferência, você recebe o erro:

  $jobname: Log de transferência: erro 404 do ajax

Esse erro será esperado se você não tiver habilitado a regra de firewall "Compartilhamento de arquivos e impressora (SMB-entrada)" no servidor orchestrator. Downloads de arquivos do Windows Admin Center exigem a porta TCP/445 (SMB) em computadores conectados.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>Erro "não foi possível transferi-armazenamento em qualquer um dos pontos de extremidade" quando a transferência do Windows Server 2008 R2

Ao tentar transferir dados de um computador de origem do Windows Server 2008 R2, não há transferências de dados e você recebe o erro:  

  Não foi possível transferir o armazenamento em qualquer um dos pontos de extremidade.
0x9044

Esse erro será esperado se o computador do Windows Server 2008 R2 não é totalmente corrigido com as atualizações de todas as críticas e importantes do Windows Update. Independentemente do serviço de migração de armazenamento, é sempre recomendável aplicação de patch um computador Windows Server 2008 R2 para fins de segurança, como o sistema operacional não contém as melhorias de segurança de versões mais recentes do Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Erro "não foi possível transferi-armazenamento em qualquer um dos pontos de extremidade" e "Verificar se o dispositivo de origem está online - nós não foi possível acessá-lo."

Ao tentar transferir dados de um computador de origem, alguns ou todos os compartilhamentos não são transferidas, com o resumo de erro:

   Não foi possível transferir o armazenamento em qualquer um dos pontos de extremidade.
0x9044

Examinando os detalhes da transferência SMB mostra o erro:

   Verifique se o dispositivo de origem está online - nós não foi possível acessá-lo.

Examinar o log de eventos do administrador/StorageMigrationService mostra:

   Não foi possível transferir o armazenamento.

   Trabalho: ID do Job1:  
   Estado: Erro com falha: Mensagem de erro 36931: 

   Diretrizes: Verifique o erro detalhado e verifique se que os requisitos de transferência são atendidos. O trabalho de transferência não foi possível transferir nenhum computador de origem e de destino. Isso pode ocorrer porque o computador do orchestrator não foi possível acessar qualquer computador de origem ou destino, possivelmente devido a uma regra de firewall, ou falta de permissões.

Examinando a StorageMigrationService-Proxy/Debug log mostra:

   Falha na validação de transferência 07/02/2019-13:35:57.231 [Erro]. Código de erro: 40961, ponto de extremidade de origem não está acessível ou não existe, ou as credenciais da fonte são inválidas ou usuário autenticado não tem permissões suficientes para acessá-lo.
em Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate() em Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest (FileTransferRequest fileTransferRequest, operationId de Guid)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Esse erro será esperado se sua conta de migração não tem pelo menos permissões de acesso de leitura para os compartilhamentos SMB. Para solucionar esse erro, adicione um grupo de segurança que contém a conta de migração de código-fonte para os compartilhamentos do SMB no computador de origem e conceda a ela controle total, alterar ou leitura. Após a migração for concluída, você pode remover esse grupo. Uma versão futura do Windows Server pode alterar esse comportamento para não precisar de permissões explícitas para compartilhamentos de origem.

## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)
