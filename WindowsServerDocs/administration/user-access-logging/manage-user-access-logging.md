---
title: Gerenciar o Log de Acesso do Usuário
description: Descreve como gerenciar o log de acesso do usuário
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03bad9864f81cf75be13b4ca391fdcbc5f9dcb5c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435350"
---
# <a name="manage-user-access-logging"></a>Gerenciar o Log de Acesso do Usuário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento descreve como gerenciar o UAL (Log de Acesso do Usuário).  
  
O UAL é um recurso que pode ajudar os administradores de servidor a quantificar o número de solicitações de cliente exclusivas de funções e serviços em um servidor local.  
  
O UAL é instalado e habilitado por padrão e coleta dados quase em tempo real. Existem apenas algumas opções de configuração para o UAL. Este documento descreve essas opções e sua finalidade.  
  
Para saber mais sobre as vantagens do UAL, consulte o [Introdução ao log de acesso do usuário](get-started-with-user-access-logging.md).  
  
**Neste documento**  
  
As opções de configuração abordadas neste documento incluem:  
  
-   Desabilitando e habilitando o serviço UAL  
  
-   Coletando e removendo dados  
  
-   Excluindo dados registrados pelo UAL  
  
-   Gerenciando o UAL em ambientes de volumes elevados  
  
-   Recuperando de um estado corrompido  
  
-   Habilitar rastreamento de licenças de uso de pastas de trabalho   
  
## <a name="BKMK_Step1"></a>Desabilitando e habilitando o serviço UAL  
UAL é habilitado e executado por padrão quando um computador executando o Windows Server 2012, ou posterior, é instalado e iniciado pela primeira vez. Os administradores podem achar recomendável desativar e desabilitar o UAL para cumprir requisitos de privacidade ou outras necessidades operacionais. Você pode desativar o UAL usando o console de serviços, a partir da linha de comando ou usando cmdlets do PowerShell. No entanto, para garantir que o UAL não é executado novamente na próxima vez em que o computador for iniciado, você também precisa desabilitar o serviço. Os procedimentos a seguir descreve como desativar e desabilitar o UAL.  
  
> [!NOTE]  
> Você pode usar o cmdlet `Get-Service UALSVC` do PowerShell para recuperar informações sobre o serviço UAL, inclusive para saber se ele está em execução ou não e se está habilitado ou desabilitado.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Para interromper e desabilitar o serviço UAL pelo console de serviços  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  No Gerenciador do Servidor, aponte para **Ferramentas**e clique em **Serviços**.  
  
3.  Role a tela para baixo e selecione **Serviço de Log para Acesso de Usuário**. Clique em **Finalizar o serviço**.  
  
4.  À direita\-clique no nome do serviço e selecione **propriedades**. Na guia **Geral** , mude o **Tipo de inicialização** para **Desabilitado**e clique em **OK**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Para interromper e desabilitar o UAL pela linha de comando  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  Pressione o logotipo do Windows + R e digite **cmd** para abrir uma janela Prompt de Comando.  
  
    > [!IMPORTANT]  
    > Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
3.  Digite **net stop ualsvc** e pressione ENTER.  
  
4.  Digite **netsh ualsvc set opmode mode=disable** e pressione ENTER.  
   
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  

Você também pode interromper e desabilitar o UAL usando os comandos Stop-service e Disable-Ual do Windows PowerShell.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Se posteriormente você deseja reiniciar e reabilitar o UAL pode fazer isso com os procedimentos a seguir.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Para iniciar e habilitar o serviço UAL pelo console de serviços  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  No Gerenciador do Servidor, aponte para **Ferramentas**e clique em **Serviços**.  
  
3.  Role a tela para baixo e selecione **Serviço de Log para Acesso de Usuário**. Clique em **Iniciar o serviço**.  
  
4.  Clique com o botão direito do mouse no nome do serviço e selecione **Propriedades**. Na guia **Geral** , mude o **Tipo de inicialização** para **Automático**e clique em **OK**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Para iniciar e habilitar o UAL pela linha de comando  
  
1.  Entre no servidor com credenciais de administrador local.  
  
2.  Pressione o logotipo do Windows + R e digite **cmd** para abrir uma janela Prompt de Comando.  
  
    > [!IMPORTANT]  
    > Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
3.  Digite **net start ualsvc** e pressione ENTER.  
  
4.  Digite **netsh ualsvc set opmode mode=enable**e pressione ENTER.  

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.
  
Você também pode iniciar e reabilitar o UAL usando os comandos Start-service e Enable-Ual do Windows PowerShell.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="BKMK_Step2"></a>Coletando dados do UAL  
Além dos cmdlets do PowerShell descritos na seção anterior, 12 cmdlets adicionais pode ser usados para coletar dados do UAL:  
  
-   **Get-UalOverview**: fornece detalhes relacionados ao UAL e o histórico das funções e dos produtos instalados.  
  
-   **Get-UalServerUser**: fornece dados de acesso de usuário cliente para o servidor local ou de destino.  
  
-   **Get-UalServerDevice**: fornece dados de acesso de dispositivo cliente para o servidor local ou de destino.  
  
-   **Get-UalUserAccess**: fornece dados de acesso de usuário cliente para cada função ou produto instalado no servidor local ou de destino.  
  
-   **Get-UalDeviceAccess**: fornece dados de acesso de dispositivo cliente para cada função ou produto instalado no servidor local ou de destino.  
  
-   **Get-UalDailyUserAccess**: fornece dados de acesso do usuário cliente para cada dia do ano.  
  
-   **Get-UalDailyDeviceAccess**: fornece dados de acesso do dispositivo cliente para cada dia do ano.  
  
-   **Get-UalDailyAccess**: fornece dados de acesso de usuário e de dispositivo cliente para cada dia do ano.  
  
-   **Get-UalHyperV**: fornece dados de máquina virtual relevantes para o servidor local ou de destino.  
  
-   **Get-UalDns**: fornece dados do servidor DNS local ou de destino específicos ao cliente DNS.  
  
-   **Get-UalSystemId**: fornece dados específicos ao sistema para identificar o servidor local ou de destino de forma exclusiva.  
  
`Get-UalSystemId` visa fornecer um perfil exclusivo de um servidor para correlação com todos os outros dados desse servidor.  Se um servidor ocorra alguma alteração em um dos parâmetros do `Get-UalSystemId` é criado um novo perfil.  `Get-UalOverview` visa fornecer ao administrador uma lista das funções instaladas e em uso no servidor.  
  
> [!NOTE]  
> Recursos básicos de serviços de impressão e documento de serviços e arquivos são instalados por padrão. Assim, os administradores podem esperar que as informações sobre eles sejam sempre exibidas como se as funções completas estivessem instaladas. Cmdlets do UAL separados são incluídos para o Hyper-V e o DNS devido aos dados exclusivos que o UAL coleta para essas funções de servidor.  
  
Um cenário típico de uso dos cmdlets do UAL seria quando um administrador consulta o UAL sobre acessos de cliente exclusivos durante um intervalo de datas. Isso pode ser feito de várias maneiras. O método a seguir é recomendado para consultar acessos de dispositivo exclusivos durante um intervalo de datas.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Isso retorna uma lista detalhada de todos os dispositivos cliente exclusivos, por endereço IP, que fizeram solicitações ao servidor nesse intervalo de datas.  
  
Para cada cliente exclusivo, ‘ActivityCount’ está limitado a 65.535 por dia. Além disso, só é necessário fazer uma chamada do PowerShell para a WMI quando você consulta por data.  Todos os outros parâmetros de cmdlets do UAL podem ser usados em consultas do PS conforme esperado, como mostra este exemplo:  
  
```  
PS C:\Windows\system32> Get-UalDeviceAccess -IPAddress "10.36.206.112"  
  
ActivityCount    : 1  
FirstSeen        : 6/23/2012 5:06:50 AM  
IPAddress        : 10.36.206.112  
LastSeen         : 6/23/2012 5:06:50 AM  
ProductName      : Windows Server 2012 Datacenter  
RoleGuid         : 10a9226f-50ee-49d8-a393-9a501d47ce04  
RoleName         : File Server  
TenantIdentifier : 00000000-0000-0000-0000-000000000000  
PSComputerName  
  
```  
  
O UAL guarda um histórico de até dois anos. Para permitir que um administrador recupere dados do UAL quando o serviço estiver em execução, o UAL faz uma cópia do arquivo de banco de dados ativo, current.mdb, para um arquivo chamado *GUID.mdb* a cada 24 horas, para ser usado pelo provedor WMI.  
  
No primeiro dia do ano, o UAL cria um novo *GUID.mdb*. O *GUID.mdb* antigo é guardado como arquivo morto para uso pelo provedor.  Passados dois anos, o *GUID.mdb* original é substituído.  
  
> [!IMPORTANT]  
> O procedimento a seguir só deve ser realizado por um usuário avançado. Normalmente, ele seria utilizado por um desenvolvedor durante o teste de sua própria instrumentação das interfaces de programação de aplicativo do UAL.  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Para ajustar o intervalo padrão de 24 horas para tornar os dados visíveis ao provedor WMI  
  
1.  Entre no servidor com uma conta que tenha privilégios de administrador local.  
  
2.  Pressione o logotipo do Windows + R e digite **cmd** para abrir uma janela Prompt de Comando.  
  
3.  Adicione o valor do Registro:  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)** .  
  
    > [!WARNING]  
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes no computador.  
  
    O exemplo a seguir mostra como adicionar um intervalo de dois minutos (não recomendado como um estado de execução a longo prazo): **REG ADD HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum /v PollingInterval /t REG\_DWORD /d 120000 /F**  
  
    Valores de hora estão em milissegundos. O valor mínimo é 60 segundos, o máximo é sete dias e o padrão é 24 horas.  
  
4.  Use o console de serviços para interromper e reiniciar o Serviço de Log para Acesso de Usuário.  
  
## <a name="deleting-data-logged-by-ual"></a>Excluindo dados registrados pelo UAL  
O UAL não tem a finalidade de ser um componente crítico. Seu design destina-se a afetar as operações do sistema local o mínimo possível, mantendo um alto nível de confiabilidade. Isso também permite que o administrador exclua manualmente o banco de dados do UAL e arquivos (todos os arquivos no diretório \Windows\System32\LogFilesSUM\) de suporte para atender às necessidades operacionais.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Para excluir dados registrados pelo UAL  
  
1. Interrompa o Serviço de Log para Acesso de Usuário.  
  
2. Abra o Windows Explorer.  
  
3. Vá para **\Windows\System32\Logfiles\SUM\\** .  
  
4. Exclua todos os arquivos da pasta.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Gerenciando o UAL em ambientes de volumes elevados  
Esta seção descreve o que um administrador pode esperar quando o UAL é usado em um servidor com elevado volume de clientes.  
  
O número máximo de acessos que podem ser registrados com o UAL é de 65.535 por dia.  Não se recomenda o uso do UAL em servidores conectados diretamente à Internet, como os servidores Web conectados diretamente à Internet, ou nos cenários em que um desempenho extremamente alto é a principal função do servidor (como nos ambientes de carga de trabalho HPC). O UAL é destina principalmente a pequeno, médio e cenários de intranet corporativo em que o volume elevado é esperado, mas não tão altas quanto muitas deployment que servem o volume de tráfego da Internet regularmente.  
  
**UAL na memória**: uma vez que o UAL utiliza o ESE (Mecanismo de Armazenamento Extensível), seus requisitos de memória aumentam ao longo do tempo (ou conforme a quantidade de solicitações de cliente). Mas a memória será liberada à medida que o sistema precisar dela para minimizar o impacto no desempenho.  
  
**UAL no disco**: os requisitos de disco rígido do UAL são aproximadamente estes:  
  
-   0 registro de cliente exclusivo: 22 MB  
  
-   50.000 registros de cliente exclusivos: 80 MB  
  
-   500.000 registros de cliente exclusivos: 384 MB  
  
-   1.000.000 registros de cliente exclusivos: 729M  
  
## <a name="recovering-from-a-corrupt-state"></a>Recuperando de um estado corrompido  
Esta seção discute o uso que o UAL faz do ESE (Mecanismo de Armazenamento Extensível) em alto nível e o que um administrador pode fazer se os dados do UAL ficarem corrompidos ou não puderem ser recuperados.  
  
O UAL emprega o ESE para otimizar o uso dos recursos do sistema e por sua resistência a danos.  Para saber mais sobre as vantagens do ESE, consulte o tópico sobre [Mecanismo de Armazenamento Extensível](https://msdn.microsoft.com/library/windows/desktop/gg269259(v=exchg.10).aspx) no MSDN.  
  
Cada vez que o serviço UAL é iniciado, o ESE realiza uma recuperação simples. Para saber mais, consulte o tópico sobre [os arquivos do Mecanismo de Armazenamento Extensível](https://msdn.microsoft.com/library/windows/desktop/gg294069(v=exchg.10).aspx) no MSDN.  
  
Se houver um problema com a recuperação simples, o ESE realizará uma recuperação de pane. Para saber mais, consulte o tópico sobre a [função JetInit](https://msdn.microsoft.com/library/windows/desktop/gg294068(v=exchg.10).aspx) no MSDN.  
  
Caso o UAL ainda não consiga ser iniciado com o conjunto existente de arquivos do ESE, ele excluirá todos os arquivos do diretório \Windows\System32\LogFiles\SUM\. Após a exclusão dos arquivos, o Serviço de Log para Acesso de Usuário será reiniciado e novos arquivos serão criados. O serviço UAL será retomado como se estivesse em um computador recém-instalado.  
  
> [!IMPORTANT]  
> Os arquivos do diretório do banco de dados do UAL nunca devem ser movidos ou modificados, pois com isso as etapas de recuperação serão iniciadas, incluindo a rotina de limpeza descrita nesta seção. Caso sejam necessários backups do diretório \Windows\System32\LogFiles\SUM\, será preciso fazer backup de todos os arquivos do diretório juntos para que uma operação de restauração funcione conforme esperado.  
  
## <a name="enable-work-folders-usage-license-tracking"></a>Habilitar rastreamento de licenças de uso de pastas de trabalho  
O servidor das pastas de trabalho podem usar UAL para relatar o uso do cliente. Ao contrário do UAL, o log de pasta de trabalho não é ativado por padrão. Você pode habilitá-lo com a seguinte alteração de regkey:  
  
```  
Reg add HKLM\Software\Microsoft\Windows\CurrentVersion\SyncShareSrv /v EnableWorkFoldersUAL /t REG_DWORD /d 1  
```  
  
Depois de adicionar a regkey, reinicie o serviço SyncShareSvc no servidor para habilitar o log.  
  
Depois que o log estiver habilitado, dois eventos informativos são registrados no canal de Log\Aplicativos do Windows sempre que um cliente se conecta ao servidor. Para pastas de trabalho, cada usuário pode ter um ou mais dispositivos de cliente que se conectam ao servidor e verificar se há atualizações de dados a cada 10 minutos. Se o servidor estiver enfrentando 1.000 usuários, cada um com dois dispositivos, os logs de aplicativo farão a substituição a cada 70 minutos, tornando difícil a solução de problemas não relacionados. Para evitar isso, você pode desabilitar temporariamente o serviço de log de acesso do usuário ou aumentar o tamanho do canal de log\aplicativos do Windows do servidor.  
  
## <a name="BKMK_Links"></a>Consulte também  

- [Introdução ao usuário acessar o registro em log](get-started-with-user-access-logging.md)
  

