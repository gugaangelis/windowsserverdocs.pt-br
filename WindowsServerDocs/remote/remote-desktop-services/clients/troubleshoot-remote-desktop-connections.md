---
title: Solucionando problemas de conexões de área de trabalho remota
description: Procedimentos de solução de problemas organizados por sintoma
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 43e40f8442600dfc66dafd6b8b210274908b4595
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446725"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Solucionando problemas de conexões de área de trabalho remota
Para obter explicações breves de vários dos problemas mais comuns dos serviços de área de trabalho remota (RDS), consulte [perguntas frequentes sobre os clientes de área de trabalho remota](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Este artigo descreve várias abordagens mais avançadas para solução de problemas de conexão. Muitos desses procedimentos se aplicam se você estiver solucionando problemas de uma configuração simple, como um computador físico para se conectar a outro computador físico ou uma configuração mais complicada. Alguns procedimentos abordam problemas que ocorrem apenas em cenários mais complicados de multiusuários. Para obter mais informações sobre os componentes da área de trabalho remota e como eles funcionam juntos, consulte [arquitetura dos serviços de área de trabalho remota](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muitos dos procedimentos descritos neste artigo exigem que você acessar vários computadores, que alguns dos quais você talvez precise acessar remotamente. Para obter mais informações sobre as ferramentas de administração remota e como configurá-las, consulte [remoto administração de servidor Tools (RSAT) para sistemas operacionais Windows de](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Na lista a seguir, identifique o tipo do sintoma que você (ou seus usuários) estão enfrentando.

- [O cliente de área de trabalho remota não pode se conectar à área de trabalho remota, mas não há sintomas específicos ou mensagens (etapas de solução de problemas gerais)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [O cliente de área de trabalho remota não pode se conectar à área de trabalho remota e recebe uma mensagem de "Classe não registrada"](#client-cannot-connect-class-not-registered)
- [O cliente de área de trabalho remota não pode se conectar à área de trabalho remota e recebe uma "não há licenças disponíveis" ou a mensagem "Erro de segurança"](#client-cannot-connect-no-licenses-available)
- [O usuário recebe uma mensagem de "Acesso negado" ou deve fornecer credenciais duas vezes](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sobre como se conectar, o recebe uma mensagem "O serviço de área de trabalho remoto está ocupado no momento"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [O cliente de área de trabalho remota se desconecta e não pode se reconectar à mesma sessão](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [O usuário se conecta a um laptop remoto em uma rede sem fio e, em seguida, o laptop se desconecta da rede](#remote-laptop-disconnects-from-wireless-network).
- [O usuário sentir um desempenho ruim ou problemas com aplicativos remotos](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obter uma lista atual dos códigos de desconexão RDP, consulte [ExtendedDisconnectReasonCode enumeração](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Nenhum sintomas específicos ou mensagens (etapas de solução de problemas gerais)

Use estas etapas quando um cliente de área de trabalho remota não pode se conectar a uma área de trabalho remota, mas não fornece mensagens ou outros sintomas que ajudam a identificam a causa. Para resolver muitas das causas mais comuns desse tipo de problema, use os seguintes métodos:

- [Verificar o status do protocolo RDP](#check-the-status-of-the-rdp-protocol)
- [Verificar o status dos serviços de RDP](#check-the-status-of-the-rdp-services)
- [Verifique se o ouvinte do RDP está funcionando](#check-that-the-rdp-listener-is-functioning)
- [Verifique a porta do ouvinte do RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Verificar o status do protocolo RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Verificar o status do protocolo RDP em um computador local

Para verificar e alterar o status do protocolo RDP em um computador local, consulte [como habilitar a área de trabalho remota](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se as opções de área de trabalho remotas não estão disponíveis, consulte [Verifique se um objeto de diretiva de grupo está bloqueando o RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Verificar o status do protocolo RDP em um computador remoto

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para verificar e alterar o status do protocolo RDP em um computador remoto, use uma conexão de registro de rede:

1. Selecione **inicie**, selecione **execute**e, em seguida, insira **regedt32**.
2. No Editor do registro, selecione **arquivo**e, em seguida, selecione **conectar registro da rede**.
3. No **Selecionar computador** diálogo caixa, digite o nome do computador remoto, selecione **verificar nomes**e, em seguida, selecione **Okey**.
4. Navegue até **HKEY\_LOCAL\_MACHINE\\sistema\\CurrentControlSet\\controle\\Terminal Server**.  
   ![Editor do registro, que mostra a entrada fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se o valor da **fDenyTSConnections** chave é **0**, em seguida, o RDP está habilitado
   - Se o valor da **fDenyTSConnections** chave é **1**, em seguida, o RDP está desabilitado
5. Para habilitar o RDP, altere o valor de **fDenyTSConnections** partir **1** para **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Verifique se um objeto de diretiva de grupo (GPO) está bloqueando o RDP em um computador local

Se você não pode ativar o RDP na interface do usuário ou se o valor de **fDenyTSConnections** reverte para **1** depois que você alterou-lo, um GPO pode substituir as configurações no nível do computador.

Para verificar a configuração de política de grupo em um computador local, abra uma janela de Prompt de comando como administrador e digite o seguinte comando:
```
gpresult /H c:\gpresult.html
```
Após a conclusão desse comando, abra gpresult.html. Na **configuração do computador\\modelos administrativos\\componentes do Windows\\dos serviços de área de trabalho remota\\Host de sessão da área de trabalho remota\\conexões**, localizar o **permitem que os usuários se conectem remotamente usando serviços de área de trabalho remota** política.

- Se a configuração para essa política está **Enabled**, diretiva de grupo não está bloqueando as conexões de RDP.
- Se a configuração para essa política está **desabilitados**, verifique **GPO vencedor**. Este é o GPO que está bloqueando as conexões de RDP.
  ![Um segmento de exemplo do gpresult.html, em que o GPO de nível de domínio * * bloco RDP * * está desabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Um segmento de exemplo do gpresult.html, em que * * Local diretiva de grupo * está desabilitando RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Verifique se um GPO está bloqueando o RDP em um computador remoto

Para verificar a configuração de diretiva de grupo em um computador remoto, o comando é quase o mesmo que para um computador local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
O arquivo que esse comando gera (**gpresult -\<nome do computador\>. HTML**) usa o mesmo formato de informações como a versão do computador local (**gpresult.html**) usa.

#### <a name="modifying-a-blocking-gpo"></a>Modificando um GPO de bloqueio

Você pode modificar essas configurações no Console de gerenciamento de diretiva de grupo (GPM) e Editor de objeto de diretiva de grupo (GPE). Para obter mais informações sobre como usar a diretiva de grupo, consulte [Advanced Group Policy Management](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Para modificar a política de bloqueio, use um dos seguintes métodos:

- No GPE, acesse o nível apropriado de GPO (por exemplo, local ou domínio) e navegue até **configuração do computador\\modelos administrativos\\componentes do Windows\\Remote Desktop Services\\ Host de sessão de área de trabalho remota\\conexões**\\**permitem que os usuários se conectem remotamente usando serviços de área de trabalho remota**.  
   1. Defina a política para **Enabled** ou **não configurado**.
   2. Nos computadores afetados, abra uma janela de Prompt de comando como administrador e execute o **gpupdate /force** comando.
- No GPM, navegue até a UO em que a política de bloqueio é aplicada aos computadores afetados e exclua a diretiva da UO.

### <a name="check-the-status-of-the-rdp-services"></a>Verificar o status dos serviços de RDP

No computador local (cliente) e o computador remoto (destino), devem estar executando os seguintes serviços:

- Serviços de área de trabalho remota (TermService)
- Redirecionador de porta de UserMode de serviços de área de trabalho remota (UmRdpService)

Você pode usar o snap-in MMC de serviços para gerenciar os serviços localmente ou remotamente. Você também pode usar o PowerShell localmente ou remotamente (se o computador remoto está configurado para aceitar comandos do PowerShell remotos).

![Serviços de área de trabalho remota no snap-in de MMC de serviços. Não modifique as configurações de serviço padrão.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Em qualquer computador, se um ou ambos os serviços não estão em execução, iniciá-los.

> [!NOTE]  
> Se você iniciar o serviço de serviços de área de trabalho remota, clique em **Sim** para reiniciar automaticamente o serviço de área de trabalho remota UserMode serviços redirecionador de portas.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Verifique se o ouvinte do RDP está funcionando

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

#### <a name="check-the-status-of-the-rdp-listener"></a>Verificar o status do ouvinte do RDP

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell como os mesmos comandos funcionam tanto localmente e remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **Enter-PSSession - ComputerName \<nome do computador\>** .
2. Enter **qwinsta**. 
    ![O comando qwinsta lista os processos escutando nas portas do computador.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Se a lista inclui **rdp-tcp** com um status de **escutar**, o ouvinte do RDP está funcionando. Vá para [verificar a porta do ouvinte RDP](#check-the-rdp-listener-port). Caso contrário, continue na etapa 4.
4. Exporte a configuração de ouvinte do RDP de um computador de trabalho.
    1. Entre em um computador que tem a mesma versão do sistema operacional que tenha o computador afetado e acessar o registro do computador (por exemplo, usando o Editor do registro).
    2. Navegue até a seguinte entrada do registro:  
        **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\controle\\servidor de Terminal\\WinStations\\RDP-Tcp**
    3. Exporte a entrada para um arquivo. reg. Por exemplo, no Editor do registro, clique com botão direito na entrada, selecione **exportar**e, em seguida, insira um nome de arquivo para as configurações exportadas.
    4. Copie o arquivo. reg exportado para o computador afetado.
5. Para importar a configuração de ouvinte do RDP, abra uma janela do PowerShell que tenha permissões administrativas no computador afetado (ou abra a janela do PowerShell e conectar-se remotamente ao computador afetado).
   1. Para fazer backup a entrada do registro existente, digite o seguinte comando:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para remover a entrada do registro existente, digite os seguintes comandos:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar a nova entrada de registro e, em seguida, reinicie o serviço, digite os seguintes comandos:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      em que \<filename\> é o nome do arquivo. reg exportado.
6. Teste a configuração de tentando novamente a conexão de área de trabalho remota. Se você ainda não conseguir conectar, reinicie o computador afetado.
7. Se você ainda não conseguir conectar, [verificar o status do certificado autoassinado RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Verificar o status do certificado autoassinado RDP

1. Se você ainda não conseguir conectar, abra o snap-in MMC de certificados. Quando solicitado para selecionar o repositório de certificados para gerenciar, selecione **conta de computador**e, em seguida, selecione o computador afetado.
2. No **certificados** pasta sob **área de trabalho remota**, exclua o certificado autoassinado do RDP. 
    ![Certificados da área de trabalho remotos no snap-in de certificados do MMC.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. No computador afetado, reinicie o serviço de serviços de área de trabalho remota.
4. Atualize o snap-in de certificados.
5. Se não tiver sido recriado, o certificado autoassinado do RDP [verificar as permissões da pasta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Verifique as permissões da pasta MachineKeys

1. No computador afetado, abra o Explorer e, em seguida, navegue até **c:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Clique com botão direito **MachineKeys**, selecione **Properties**, selecione **segurança**e, em seguida, selecione **avançado**.
3. Certifique-se de que as seguintes permissões são configuradas:
      - BUILTIN\\administradores: Controle total
      - Todas as pessoas: Leitura, gravação

### <a name="check-the-rdp-listener-port"></a>Verifique a porta do ouvinte do RDP

No computador local (cliente) e o computador remoto (destino), o ouvinte do RDP deve estar escutando na porta 3389. Nenhum outro aplicativo deve estar usando essa porta.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para verificar ou alterar a porta do RDP, use o Editor do registro:

1. Selecione Iniciar, selecione **executados**e, em seguida, insira **regedt32**.
      - Para se conectar a um computador remoto, selecione **arquivo**e, em seguida, selecione **conectar registro da rede**.
      - No **Selecionar computador** diálogo caixa, digite o nome do computador remoto, selecione **verificar nomes**e, em seguida, selecione **Okey**.
2. Abra o registro e navegue até **HKEY\_LOCAL\_MACHINE\\sistema\\CurrentControlSet\\controle\\Terminal Server\\WinStations\\ \<ouvinte\>** . 
    ![A subchave PortNumber para o protocolo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **PortNumber** tem um valor diferente de **3389**, altere-a para **3389**. 
   > [!IMPORTANT]  
    > Você pode operar os serviços de área de trabalho remota usando outra porta. No entanto, não recomendamos que você faça isso. Essa configuração de solução de problemas está além do escopo deste artigo.
4. Depois de alterar o número da porta, reinicie o serviço de serviços de área de trabalho remota.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Verifique se outro aplicativo não está tentando usar a mesma porta

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos comandos de trabalho localmente e remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **Enter-PSSession - ComputerName \<nome do computador\>** .
2. Digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![O comando netstat produz uma lista de portas e serviços de escuta a eles.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Procure uma entrada para a porta TCP 3389 (ou para a porta RDP atribuída) com o status **Escutando**. 
    > [!NOTE]  
   > O PID (identificador do processo) do processo ou serviço que está usando aquela porta aparece na coluna PID.
4. Para determinar qual aplicativo está usando a porta 3389 (ou a porta do RDP atribuída), digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![O comando tasklist relata detalhes de um processo específico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Procure uma entrada para o número do PID que está associado com a porta (da **netstat** saída). Os serviços ou processos que estão associados esse PID aparecerão à direita.
6. Se um aplicativo ou serviço que não seja o Remote Desktop Services (TermServ.exe) está usando a porta, você pode resolver o conflito usando um dos seguintes métodos:
      - Configure o outro aplicativo ou serviço para usar uma porta diferente (recomendada).
      - Desinstale o aplicativo ou serviço.
      - Configurar o RDP para usar uma porta diferente e, em seguida, reinicie o serviço de serviços de área de trabalho remota (não recomendado).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Verifique se um firewall está bloqueando a porta do RDP

Use o **psping** ferramenta para testar se você pode alcançar o computador afetado usando a porta 3389.

1. Em um computador diferente do computador afetado, baixe **psping** de <https://live.sysinternals.com/psping.exe>.
2. Abra uma janela de Prompt de comando como administrador, altere para o diretório no qual você instalou **psping**e, em seguida, digite o seguinte comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Verifique a saída do **psping** para resultados como o seguinte comando:  
      - **Conectar-se ao \<IP de computador\>** : O computador remoto está acessível.
      - **(0% de perda)** : Todas as tentativas de conexão foi bem-sucedida.
      - **O computador remoto recusou a conexão de rede**: O computador remoto não está acessível.
      - **(100% de perda)** : Todas as tentativas de conexão com falha.
1. Execute **psping** em vários computadores para testar sua capacidade de se conectar ao computador afetado.
1. Observe se o computador afetado bloqueia conexões de todos os outros computadores, alguns outros computadores ou apenas um outro computador.
1. Próximas etapas recomendadas:
      - Entre em contato com seus administradores de rede para verificar que a rede permite tráfego RDP para o computador afetado.
      - Investigar as configurações de qualquer firewall entre os computadores de origem e o computador afetado (incluindo o Firewall do Windows no computador afetado) para determinar se um firewall está bloqueando a porta do RDP.

## <a name="client-cannot-connect-class-not-registered"></a>Cliente não conseguir conectar, "Classe não registrada"

Quando você tenta se conectar a um computador remoto usando um cliente que está executando o Windows 10, versão 1709 ou posterior, o cliente não poderá se conectar enquanto o servidor de Host de sessão de área de trabalho remota informa uma mensagem que contém o código de erro "Classe não registrada (0x80040154)".

Esse problema ocorre se o usuário que está tentando se conectar tiver um perfil de usuário obrigatório. Para resolver esse problema, instale o [24 de julho de 2018 – KB4338817 (SO Build 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) atualização do Windows 10.

## <a name="client-cannot-connect-no-licenses-available"></a>Cliente não pode se conectar, não há licenças disponíveis

Essa situação se aplica a implantações que incluem um servidor RDSH e um servidor de licenciamento de área de trabalho remota.

Identificar qual tipo de comportamento que os usuários estão vendo:

- A sessão foi desconectada porque não há licenças disponíveis ou nenhum servidor de licença está disponível
- O acesso foi negado devido a um erro de segurança

Entrar para o Host de sessão de área de trabalho remota como um administrador de domínio e abra o diagnóstico de licença da área de trabalho remota. Procure as mensagens, como o seguinte:

  - O período de cortesia para o computador remoto que o servidor de Host de sessão da área de trabalho tiver expirado, mas o servidor de Host de sessão de área de trabalho remota não foi configurado com quaisquer servidores de licença. conexões com o servidor de Host de sessão de área de trabalho remota serão negadas, a menos que um servidor de licença está configurado para o servidor de Host de sessão de área de trabalho remota.
  - Servidor de licença \<nome do computador\> não está disponível. Isso pode ser causado por problemas de conectividade de rede, o serviço de licenciamento de área de trabalho remota é interrompido no servidor de licença ou licenciamento de área de trabalho remota não está mais instalado no computador.

Esses problemas tendem a ser associado com as seguintes mensagens de usuário:

  - A sessão remota foi desconectada porque não há licenças de acesso de cliente de área de trabalho remota estão disponíveis para este computador.
  - A sessão remota foi desconectada porque não há nenhum servidores de licença de área de trabalho remota disponíveis para fornecer uma licença.

Nesse caso, [configurar o serviço de licenciamento de área de trabalho remota](#configure-the-rd-licensing-service).

Se o diagnóstico de licença da área de trabalho remota para listar outros problemas, como "o componente de protocolo RDP 224 detectou um erro no fluxo do protocolo e desconectou o cliente," pode ser um problema que afeta os certificados de licença. Esses problemas tendem a ser associada a mensagens de usuário, como o seguinte:

Devido a um erro de segurança, o cliente não pôde se conectar ao servidor de Terminal. Após certificar-se de que você está conectado à rede, tente se conectar ao servidor novamente.

Nesse caso, [chaves de registro de certificado X509 a atualização](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurar o serviço de licenciamento de área de trabalho remota

O procedimento a seguir usa o Gerenciador do servidor para fazer as alterações de configuração. Para obter informações sobre como configurar e usar o Gerenciador do servidor, consulte [Gerenciador do servidor](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Abra o Gerenciador do servidor e, em seguida, navegue até **Remote Desktop Services**.
2. Na **visão geral da implantação**, selecione **tarefas**e, em seguida, selecione **editar propriedades de implantação**.
3. Selecione **licenciamento de área de trabalho remota**e, em seguida, selecione o modo de licenciamento apropriado para sua implantação (**por dispositivo** ou **por usuário**).
4. Insira o nome de domínio totalmente qualificado (FQDN) do seu servidor de licenças de área de trabalho remota e, em seguida, selecione **adicionar**.
5. Se você tiver mais de um servidor de licenças de área de trabalho remota, repita a etapa 4 para cada servidor. 
    ![Opções de configuração de servidor das licenças de área de trabalho remota no Gerenciador do servidor.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Chaves de registro de certificado X509 de atualização

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para resolver esse problema, faça backup e, em seguida, remover X509 chaves de registro de certificado, reinicie o computador e, em seguida, servidor de licenciamento de reativar a área de trabalho remota. Para fazer isso, siga estas etapas:

> [!NOTE]  
> Execute o seguinte procedimento em cada um dos servidores do RDSH.

1. Abra o Editor do registro e navegue até **HKEY\_LOCAL\_MACHINE\\sistema\\CurrentControlSet\\controle\\Terminal Server\\RCM**.
2. No menu de registro, selecione **Exportar arquivo de registro**.
3. Tipo de **certificado exportado** na **nome do arquivo** caixa e, em seguida, selecione **salvar**.
4. Com cada um dos seguintes valores, selecionados o botão direito **exclua**e, em seguida, selecione **Sim** para verificar a exclusão:  
      - **Certificado**
      - **Certificado X509**
      - **ID do Certificado X509**
      - **X509 Certificate2**
5. Saia do Editor do registro e, em seguida, reinicie o servidor RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>Usuário não pode autenticar ou deve se autenticar duas vezes

Há vários problemas que podem causar problemas que afetam a autenticação do usuário. Esta seção aborda os seguintes casos:

  - [Um usuário for negado acesso com uma mensagem "tipo de logon de restricted"](#access-denied-restricted-type-of-logon)
  - [Um usuário ou aplicativo for negado acesso com o evento 16965, "uma chamada remota para o banco de dados SAM foi negada"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Um usuário não pode entrar usando um cartão inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se o computador remoto estiver bloqueado, um usuário precisa inserir uma senha duas vezes](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Um usuário não pode entrar e receber mensagens de "Correção de oracle de criptografia CredSSP" e "Erro de autenticação"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Depois de atualizar computadores cliente, alguns usuários precisam entrar duas vezes](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Os usuários são o acesso negados em uma implantação que usa RemoteGuard com vários agentes de Conexão de área de trabalho remota](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Acesso negado, restritos de tipo de logon

Nessa situação, um usuário tentar se conectar a computadores Windows 10 ou Windows Server 2016 do Windows 10 tiver acesso negado com a seguinte mensagem:

> Conexão de área de trabalho remota:  
> O administrador do sistema restringiu o tipo de logon (rede ou interativo) que você pode usar. Para obter assistência, entre em contato com o administrador do sistema ou o suporte técnico.

Esse problema ocorre quando a autenticação de nível de rede (NLA) é necessária para conexões de RDP e o usuário não é um membro do **Remote Desktop Users** grupo. Também pode ocorrer se o **Remote Desktop Users** grupo não foi atribuído para o **acesso a este computador pela rede** direito de usuário.

Para corrigir esse problema, siga uma destas etapas:

  - [Modificar associação de grupo do usuário ou a atribuição de direitos de usuário](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desative a NLA (não recomendado).
  - Use os clientes de desktop remotos que não sejam Windows 10. Por exemplo, os clientes do Windows 7 não apresenta esse problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificar associação de grupo do usuário ou a atribuição de direitos de usuário

Se esse problema afeta um único usuário, a solução mais simples para esse problema é adicionar o usuário para o **Remote Desktop Users** grupo.

Se o usuário já é um membro desse grupo (ou se vários membros do grupo têm o mesmo problema), verifique a configuração de direitos de usuário no computador remoto do Windows 10 ou Windows Server 2016.

1. Abra o Editor de objeto de diretiva de grupo (GPE) e conecte-se para a política local do computador remoto.
2. Navegue até **configuração do computador\\configurações do Windows\\configurações de segurança\\políticas locais\\atribuição de direitos de usuário**, clique com botão direito **acessá-la o computador da rede**e, em seguida, selecione **propriedades**.
3. Verifique a lista de usuários e grupos para **Remote Desktop Users** (ou um grupo pai).
4. Se a lista não inclui **Remote Desktop Users** (ou um pai grupo, como **todas as pessoas**) você precisa adicioná-lo à lista (se você tiver mais de um ou dois computadores em sua implantação, use um objeto de diretiva de grupo) .  
    Por exemplo, a associação padrão para **acesso a este computador pela rede** inclui **todas as pessoas**. Se sua implantação usa um objeto de diretiva de grupo para remover **Everyone**, talvez você precise restaurar o acesso ao atualizar o objeto de diretiva de grupo para adicionar **Remote Desktop Users**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Acesso negado, uma chamada remota para o banco de dados SAM foi negada

Esse comportamento é mais provável de ocorrer se os controladores de domínio estão executando o Windows Server 2016 ou posterior, e os usuários tentam se conectar por meio de um aplicativo de conexão personalizada. Em particular, aplicativos que acessam informações de perfil do usuário no Active Directory terá o acesso negados.

Esse comportamento resulta de uma mudança para o Windows. No Windows Server 2012 R2 e versões anteriores versões, quando um usuário faz logon em uma área de trabalho, contatos de Gerenciador de Conexão remota (RCM) remoto o controlador de domínio (DC) para consultar as configurações que são específicas para a área de trabalho remota no objeto de usuário no domínio do Active Directory Serviços (AD DS). Essas informações são exibidas na guia perfil de serviços de área de trabalho remota das propriedades do objeto do usuário no Active Directory Users e Computers MMC snap-in.

A partir do Windows Server 2016, RCM não consulta o objeto de usuários no AD DS. Se você precisar RCM consultar o AD DS porque você está usando os atributos de serviços de área de trabalho remota, você deve habilitar manualmente o comportamento anterior do RCM.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [fazer backup do registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para habilitar o comportamento herdado do RCM em um servidor de Host de sessão de área de trabalho remota, configurar as seguintes entradas de registro e, em seguida, reinicie o **Remote Desktop Services** serviço:  
  - **HKEY\_LOCAL\_máquina\\SOFTWARE\\políticas\\Microsoft\\Windows NT\\dos serviços de Terminal**
  - **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\controle\\Terminal Server\\WinStations\\\<Winstation nome\>\\**  
      - Name: **fQueryUserConfigFromDC**
      - Digite: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar o comportamento herdado do RCM em um servidor que não seja um servidor de Host de sessão de área de trabalho remota, configurar essas entradas do registro e a seguinte entrada de registro adicionais (e, em seguida, reinicie o serviço):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para obter mais informações sobre esse comportamento, consulte o KB 3200967 [altera remoto o Gerenciador de Conexão no Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Um usuário não pode entrar usando um cartão inteligente

Este artigo aborda três circunstâncias comuns em que um usuário não pode entrar uma área de trabalho remota usando um cartão inteligente:  
  - [O usuário não pode entrar uma filial que usa um somente leitura domínio RODC (controlador)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [O usuário não pode entrar um computador Windows Server 2008 SP2 que foi atualizado recentemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [O usuário não é possível permanecer conectado a um computador Windows que foi atualizado recentemente, e o serviço dos serviços de área de trabalho remota se torna sem resposta (trava)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Não é possível entrar com um cartão inteligente em uma filial com um RODC

Esse problema ocorre em implantações que incluem um servidor RDSH em um site de branch que usa um RODC. O servidor de RDSH é hospedado no domínio raiz. Os usuários da filial pertencem a um domínio filho e usam cartões inteligentes para autenticação. O RODC está configurado para as senhas de usuário do cache (RODC pertence a **permitido grupo de replicação de Senha RODC**). Quando os usuários tentarem entrar para sessões no servidor de RDSH, recebem mensagens como "a tentativa de logon é inválida. Isso é devido a uma informações de nome de usuário ou autenticação ruins".

Esse é um problema conhecido da maneira que a raiz do controlador de domínio e RODC gerenciam a criptografia das credenciais do usuário. A raiz do controlador de domínio usa uma chave de criptografia para criptografa as credenciais e o RODC fornece uma chave de descriptografia ao cliente. No entanto, as duas chaves não correspondem.

Para contornar esse problema, considere alterar sua topologia de controlador de domínio ao se desativar o cache de senhas no RODC ou implantando um controlador de domínio gravável para o site do branch. É um método alternativo mover o servidor RDSH ao mesmo domínio filho que os usuários. Outra alternativa é permitir que usuários se conectem sem cartões inteligentes. Qualquer uma dessas soluções pode exigir comprometimento no desempenho ou nível de segurança.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Não é possível entrar com um cartão inteligente para um computador Windows Server 2008 SP2

Esse problema ocorre quando os usuários entram um computador Windows Server 2008 SP2 que foi atualizado com KB4093227 (2018.4B). Quando os usuários tentam entrar usando um cartão inteligente, lhes for negado acesso com mensagens, como "nenhum certificado válido encontrado. Verifique se o cartão está inserido corretamente e se encaixa perfeitamente." Ao mesmo tempo, o computador com Windows Server registra o evento de aplicativo "Ocorreu um erro ao recuperar um certificado digital do cartão inteligente inserido. Assinatura inválida."

Para resolver esse problema, atualize o computador com Windows Server com o 2018.06 B relançamento 4093227 KB, [descrição da atualização de segurança para a protocolo de área de trabalho remota (RDP) Windows vulnerabilidade de negação de serviço no Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Não é possível permanecer conectado com um cartão inteligente e travamentos de serviço dos serviços de área de trabalho remota

Esse problema ocorre quando os usuários entram um computador Windows ou Windows Server que foi atualizado com 4056446 KB. Primeiro, o usuário poderá entrar no sistema usando um cartão inteligente, mas, em seguida, recebe um "SCARTAR\_eletrônico\_não\_SERVICE" mensagem de erro. O computador remoto poderá parar de responder.

Para contornar esse problema, reinicie o computador remoto.

Para resolver esse problema, atualize o computador remoto com a correção apropriada:

  - Windows Server 2008 SP2: KB 4090928, [Windows vazamentos de identificadores do processo lsm.exe e aplicativos de cartão inteligente podem exibir "SCARTAR\_eletrônico\_não\_SERVICE" erros](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versão 1607: KB 4103720, [17 de maio de 2018 – KB4103720 (Build do sistema operacional 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Se o computador remoto estiver bloqueado, um usuário precisa inserir uma senha duas vezes

Esse problema pode ocorrer quando um usuário tenta se conectar a uma área de trabalho remota que esteja executando o Windows 10 versão 1709 em uma implantação em que as conexões de RDP não exigem a NLA. Sob essas condições, se a área de trabalho remota tiver sido bloqueada, o usuário precisa inserir suas credenciais duas vezes ao se conectar.

Para resolver esse problema, atualize o computador do Windows 10 versão 1709 com 4343893 de KB [30 de agosto de 2018 – KB4343893 (SO Build 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Usuário não é possível entrar e receber mensagens de "Correção de oracle de criptografia CredSSP" e "Erro de autenticação"

Quando os usuários tentarem entrar usando qualquer versão do Windows do Windows Vista SP2 e versões posteriores ou Windows Server 2008 SP2 e versões posteriores, eles têm acesso negados e recebem mensagens, como o seguinte:

  - Ocorreu um erro de autenticação. Não há suporte para a função solicitada.
  - Isso pode ser devido a correção do CredSSP criptografia oracle

"CredSSP criptografia oracle-remediation" se refere a um conjunto de atualizações de segurança lançado em março, abril e maio de 2018. CredSSP é um provedor de autenticação que processa as solicitações de autenticação para outros aplicativos. A 13 de março de 2018, "3B" e as atualizações subsequentes resolvidos uma exploração em que um invasor pode retransmitir credenciais do usuário para executar o código no sistema de destino.

As atualizações inicias adicionou suporte para um novo objeto de diretiva de grupo **criptografia Oracle correção**, que tem as seguintes configurações possíveis:

  - **Vulnerável**. Aplicativos cliente que usam o CredSSP podem voltar a versões inseguras, mas esse comportamento expõe as áreas de trabalho remotas a ataques. Serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Atenuado**. Aplicativos cliente que usam o CredSSP não podem voltar a versões inseguras, mas os serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Forçar atualizados clientes**. Aplicativos cliente que usam o CredSSP não podem voltar a versões inseguras e serviços que usam o CredSSP não aceitará os clientes sem patch. 
    Observação: Essa configuração não deve ser implantada até que todos os hosts remotos dão suporte a versão mais recente.

A atualização de 8 de maio de 2018, alterado o padrão **criptografia Oracle remediação** definindo a partir **vulnerável** para **mitigada**. Com essa alteração em vigor, os clientes da área de trabalho remotos que têm as atualizações não podem se conectar aos servidores que eles não estão (ou atualizado os servidores que não foram reiniciados). Para obter mais informações sobre os efeitos de atualizações e os tipos de comunicação que elas bloqueiam, consulte 4093492 de KB [atualizações do CredSSP para CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver esse problema, certifique-se de que todos os sistemas estão atualizados e reiniciados. Para obter uma lista completa de atualizações e obter mais informações sobre as vulnerabilidades, consulte [CVE-2018-0886 | Vulnerabilidade de execução de código remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Para contornar esse problema até que as atualizações forem concluídas, verifique 4093492 KB tipos permitidos de conexões. Se não houver nenhum alternativas viáveis, você pode considerar um dos seguintes métodos:

- Para computadores clientes afetados, defina as **criptografia Oracle remediação** política de volta para **vulnerável**.
- Modificar as políticas a seguir na **configuração do computador\\modelos administrativos\\componentes do Windows\\Remote Desktop Services\\Host da sessão da área de trabalho remota\\ Segurança** pasta de diretiva de grupo:  
  - **Exigir o uso de camada de segurança específica para conexões remotas (RDP)** : definido como **Enabled** e selecione **RDP**.
  - **Exigir autenticação do usuário para conexões remotas usando a autenticação de nível de rede**: definido como **desabilitado**.
    > [!IMPORTANT]  
    > Essas modificações reduzem a segurança da sua implantação. Eles só devem ser temporários, se você usá-los em todos os.

Para obter mais informações sobre como trabalhar com a diretiva de grupo, consulte [modificando um GPO de bloqueio](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Depois de atualizar computadores cliente, alguns usuários precisam entrar duas vezes

Os usuários em alguma versão do Windows 7 ou Windows 10 1709 entrar em uma sessão de área de trabalho remota e ver imediatamente uma segunda tela entrar. Esse problema ocorre se os computadores cliente tem recebido as seguintes atualizações:

  - Windows 7: KB 4103718, [8 de maio de 2018 – KB4103718 (acúmulo mensal)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de maio de 2018 – KB4103727 (Build do sistema operacional 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver esse problema, certifique-se de que os computadores que desejam que os usuários para se conectar ao (além dos servidores RDSH ou RDVI) estejam totalmente atualizados por meio de junho de 2018. Isso inclui as seguintes atualizações:

  - Windows Server 2016: KB 4284880, [12 de junho de 2018 – KB4284880 (Build do sistema operacional 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junho de 2018 – KB4284815 (acúmulo mensal)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junho de 2018 – KB4284855 (acúmulo mensal)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junho de 2018 – KB4284826 (acúmulo mensal)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [descrição da atualização de segurança para a vulnerabilidade de execução remota de código CredSSP no Windows Server 2008, Windows Embedded POSReady 2009 e 2009 padrão incorporado do Windows: 13 de março de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Os usuários são o acesso negados em uma implantação que usa do Credential Guard remoto com vários agentes de Conexão de área de trabalho remota

Esse problema ocorre em implantações de alta disponibilidade que usam dois ou mais área de trabalho Conexão agentes remotos, se o Windows Defender Remote Credential Guard está em uso. Os usuários não podem entrar em áreas de trabalho remotas.

Esse problema ocorre porque do Credential Guard remoto usa Kerberos para autenticação e restringe o NTLM. No entanto, em uma configuração de alta disponibilidade com balanceamento de carga, os agentes de Conexão de área de trabalho remota não pode dar suporte a operações Kerberos.

Se você precisar usar uma configuração de alta disponibilidade com agentes de Conexão de área de trabalho remota com balanceamento de carga, você pode contornar esse problema desabilitando do Credential Guard remoto. Para obter mais informações sobre como gerenciar o Windows Defender Remote Credential Guard, consulte [credenciais de área de trabalho remota proteger com o Windows Defender Remote Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Sobre como se conectar, o usuário recebe a mensagem "O serviço de área de trabalho remoto está ocupado no momento"

Para determinar uma resposta apropriada para esse problema, use as seguintes etapas:

1. O serviço dos serviços de área de trabalho remota se torna sem resposta (por exemplo, o cliente de área de trabalho remota parece "congelar" na tela de boas-vinda).  
      - Se o serviço ficar sem resposta, consulte [problema de memória do servidor RDSH](#rdsh-server-memory-issue).
      - Se o cliente parece estar interagindo com o serviço normalmente, vá para a próxima etapa.
2. Se um ou mais usuários desconectarem as sessões de área de trabalho remotas, os usuários podem conectar novamente?  
   - Se o serviço continua a negar conexões, independentemente de quantos usuários desconectar as sessões, consulte [problema de ouvinte de área de trabalho remota](#rd-listener-issue).
   - Se o serviço começa a aceitar conexões novamente após um número de usuários têm suas sessões desconectadas, [verificar a política de limite de conexão](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema de memória do servidor RDSH

Foi encontrado um vazamento de memória em alguns servidores do Windows Server 2012 R2 RDSH. Ao longo do tempo, esses servidores começam se recusar a conexões de área de trabalho remota e entradas do console local com as mensagens, como o seguinte:

> A tarefa que você está tentando não pode ser concluída porque o serviço de área de trabalho remota está ocupado no momento. Tente novamente em alguns minutos. Outros usuários ainda devem ser capazes de entrar.

Clientes de desktop remotos tentando se conectar também param de responder.

Para contornar esse problema, reinicie o servidor RDSH.

Para resolver esse problema, aplique 4093114 KB, [10 de abril de 2018 – KB4093114 (Rollup)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)) mensal, para o Servidores RDSH.

### <a name="rd-listener-issue"></a>Problema de ouvinte de área de trabalho remota

Um problema foi observado em alguns servidores RDSH que tenham sido atualizados diretamente do Windows Server 2008 R2 para Windows Server 2012 R2 ou Windows Server 2016. Quando um cliente de área de trabalho remoto se conecta ao servidor de RDSH, o servidor de RDSH cria um ouvinte de área de trabalho remota para a sessão do usuário. Os servidores afetados mantém uma contagem dos ouvintes de área de trabalho remota que aumenta à medida que os usuários se conectar, mas nunca diminui.

Para contornar esse problema, você pode usar os seguintes métodos:

  - Reinicie o servidor RDSH para redefinir a contagem de ouvintes de área de trabalho remota
  - Modificar a política de limite de conexão, definindo-o como um valor muito grande. Para obter mais informações sobre como gerenciar a política de limite de conexão, consulte [verificar a política de limite de conexão](#check-the-connection-limit-policy).

Para resolver esse problema, aplique as seguintes atualizações para os servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Verifique a política de limite de conexão

Você pode definir o limite no número de conexões simultâneas de área de trabalho remotas no nível do computador individual ou por meio da configuração de um objeto de diretiva de grupo (GPO). Por padrão, o limite não é definido.

Para verificar as configurações atuais e identificar quaisquer GPOs existentes no servidor de RDSH, abra uma janela de prompt de comando como administrador e digite o seguinte comando:
  
```
gpresult /H c:\gpresult.html
```
   
Após esse comando terminar, abra gpresult.html e na **configuração do computador\\modelos administrativos\\componentes do Windows\\Remote Desktop Services\\Host da sessão da área de trabalho remota \\Conexões**, localize o **limitar o número de conexões** política.

  - Se a configuração para essa política está **desabilitado**, em seguida, diretiva de grupo não está limitando a conexões RDP.
  - Se a configuração para essa política está **Enabled**, em seguida, verifique **GPO vencedor**. Se você precisar remover ou alterar o limite de conexão, edite esse GPO.

Para aplicar alterações de política, abra uma janela de prompt de comando no computador afetado e digite o seguinte comando:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Desconecta de cliente de área de trabalho remota e não pode se reconectar à mesma sessão

Depois que o cliente de área de trabalho remota perde sua conexão de área de trabalho remota, o cliente não é possível reconectar imediatamente. O usuário recebe mensagens de erro como o seguinte:

  - Devido a um erro de segurança, o cliente não pôde se conectar ao servidor de Terminal. Após certificar-se de que você está conectado à rede, tente se conectar ao servidor novamente.
  - Área de trabalho remota desconectada. Devido a um erro de segurança, o cliente não pôde se conectar ao computador remoto. Verifique se você estiver conectado à rede e, em seguida, tente se conectar novamente.

Em um momento posterior, o cliente de área de trabalho remota se reconecta a área de trabalho remota. No entanto, em vez de conexão do cliente para a sessão original, o servidor de RDSH conecta o cliente a uma nova sessão. Verificar o servidor de RDSH revela que a sessão original não inseriu um estado desconectado, mas em vez disso, permanece ativa.

Para contornar esse problema, você pode permitir que o **configurar intervalo de conexão keep-alive** diretiva na **configuração do computador\\modelos administrativos\\componentes do Windows\\ Serviços de área de trabalho remota\\Host de sessão de área de trabalho remota\\conexões** pasta de diretiva de grupo. Se você habilitar essa política, você deve inserir um intervalo de keep-alive. O intervalo keep-alive determina a frequência, em minutos, o servidor verifica o estado da sessão.

Esse problema pode ser causado por uma configuração incorreta de suas configurações de autenticação e a configuração. Você pode configurar essas configurações no nível do servidor, ou usando GPOs. As configurações de política de grupo estão disponíveis na **configuração do computador\\modelos administrativos\\componentes do Windows\\Remote Desktop Services\\Host da sessão da área de trabalho remota\\ Segurança** pasta de diretiva de grupo.

1. No servidor de Host da Sessão RD, abra a Configuração de Host da Sessão da Área de Trabalho Remota.
2. Sob **conexões**, o nome da conexão com o botão direito e, em seguida, clique em **propriedades**.
3. No **propriedades** caixa de diálogo de conexão, na **gerais** guia **segurança** de camada, selecione um método de segurança.
4. Na **nível de criptografia**, clique no nível desejado. Você pode selecionar **baixa**, **compatível com cliente**, **alta**, ou **compatível com FIPS**.

> [!NOTE]  
>  - Quando a comunicação entre clientes e servidores de Host de sessão de área de trabalho remota exige o nível mais alto de criptografia, use a criptografia compatível com FIPS.
>  - As configurações de nível de criptografia que você configurar na política de grupo substituem a configuração que podem ser definidas usando a ferramenta de configuração de serviços de área de trabalho remota. Além disso, se você habilitar o [criptografia do sistema: Use algoritmos compatíveis com FIPS para criptografia, hash e assinatura](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) essa configuração de política, substitui o **definir o nível de criptografia de conexão de cliente** política. A política de criptografia do sistema está no **configuração do computador\\configurações do Windows\\configurações de segurança\\políticas locais\\opções de segurança** pasta.
>  - Quando você alterar o nível de criptografia, o novo nível de criptografia terá efeito da próxima vez que um usuário fizer o logon. Se você precisar de vários níveis de criptografia em um servidor, instale vários adaptadores de rede e configure cada adaptador separadamente.
>  - Para verificar se o certificado tem uma chave privada correspondente na configuração de serviços de área de trabalho remota, clique com botão direito a conexão para o qual você deseja exibir o certificado, selecione **gerais**, selecione **editar**, selecione o certificado que você deseja exibir e, em seguida, selecione **Exibir certificado**. Na parte inferior a **geral** guia, a instrução, "você tem uma chave privada que corresponde a este certificado" deve aparecer. Você também pode exibir essas informações usando o snap-in de certificados.
>  - Criptografia compatível com FIPS (o **criptografia do sistema: Os algoritmos em conformidade Use FIPS para criptografia, hash e assinatura** diretiva ou o **compatível com FIPS** na configuração de servidor de área de trabalho remota) criptografa e descriptografa os dados enviados do cliente para o servidor e do o servidor para o cliente, com os algoritmos de criptografia 1 140 FIPS Federal Information Processing Standard (), usando módulos criptográficos da Microsoft. Para obter mais informações, consulte [validação do FIPS 140](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - O **alta** configuração criptografa os dados enviados do cliente para o servidor e do servidor para o cliente usando criptografia forte de 128 bits.
>  - O **compatível com cliente** configuração criptografa os dados enviados entre o cliente e servidor com a restrição de chave máximo com suporte no cliente.
>  - O **baixa** configuração criptografa os dados enviados do cliente ao servidor usando a criptografia de 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Laptop remotos se desconecta da rede sem fio

Esse problema pode ocorrer quando um cliente de área de trabalho remoto se conecta a um computador laptop usando uma rede sem fio do 802.1 x. Intermitentemente, o laptop se desconecta da rede sem fio e não se reconectar automaticamente.

Isso é um problema conhecido que ocorre quando é a configuração de autenticação de rede para a conexão de rede sem fio **autenticação de usuário**.

Para contornar esse problema, defina a configuração de autenticação de rede como **autenticação do usuário ou computador** ou **autenticação do computador**.

 > [!NOTE]  
> Para alterar as configurações de autenticação de rede em um único computador, talvez você precise usar o painel de controle do Centro de compartilhamento e de rede para criar uma nova conexão sem fio com as novas configurações.

Para obter uma descrição completa de como definir as configurações de rede sem fio usando GPOs, consulte [políticas de configurar a rede sem fio (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>Experiências de usuário, um desempenho ruim ou problemas de aplicativo

Esta seção aborda vários problemas comuns que os usuários podem enfrentar ao usarem a funcionalidade da área de trabalho remota:

  - [Problemas de usuários que se conectam a novas máquinas virtuais Azure Microsoft intermitentemente](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Os usuários que se conectam a áreas de trabalho remotas do Windows 10 versão 1709 podem ter problemas de reprodução de vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Os usuários com perfis de usuário somente leitura que se conectam a áreas de trabalho remotas do Windows 10 não é possível compartilhar suas áreas de trabalho.](#desktop-sharing-issues-on-windows-10)
  - [Usuários que se conectam a remotas do Windows 10 usando clientes que executam versões mais antigas do Windows 10 enfrentar um desempenho ruim quando NLA está desabilitado](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando os usuários executar vários aplicativos em sessões de área de trabalho remotas e desconectarem e reconectar-se com frequência, que podem ocorrer uma tela preta na reconexão.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes com novas máquinas de virtuais do Microsoft Azure

Esse problema afeta as máquinas virtuais que foram provisionadas recentemente. Depois que o usuário se conectar à máquina virtual, a sessão de área de trabalho remota pode não carregar todas as configurações do usuário corretamente.

Para contornar esse problema, desconecte a máquina virtual, aguarde pelo menos 20 minutos e, em seguida, conectar-se novamente.

Para resolver esse problema, aplique as seguintes atualizações para as máquinas virtuais, conforme apropriado:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reprodução de vídeo no Windows 10 versão 1709

Esse problema ocorre quando os usuários se conectar a computadores remotos que executam o Windows 10, versão 1709. Quando esses usuários reproduzir o vídeos usando o VMR9 codec (9 de vídeo do renderizador de misturar), o player mostra apenas uma janela preta.

Esse é um problema conhecido no Windows 10, versão 1709. O problema não ocorre no Windows 10 versão 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemas no Windows 10 de compartilhamento de área de trabalho

Esse problema ocorre quando o usuário tem um perfil de usuário somente leitura (e o hive do Registro associados), como em um cenário de quiosque. Quando esse usuário se conecta a um computador remoto que esteja executando o Windows 10, versão 1803, eles não podem compartilhar sua área de trabalho.

Para corrigir esse problema, aplique a atualização do Windows 10 4340917, [24 de julho de 2018 – KB4340917 (SO Build 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de desempenho ao misturar versões do Windows 10 se NLA está desabilitado

Esse problema ocorre quando o NLA está desabilitado e os computadores cliente da área de trabalho remota que executam o Windows 10 se conectem a áreas de trabalho remotas que executam versões diferentes do Windows 10. Usuários de clientes da área de trabalho remotos em computadores que executam o Windows 10, versão 1709 ou versões anteriores enfrentar um desempenho ruim quando se conectarem a computadores remotos que executam o Windows 10, versão 1803 ou posterior.

Isso ocorre porque, quando NLA está desabilitado, os computadores cliente mais antigos usam um protocolo mais lento ao se conectar ao Windows 10, versão 1803 ou posterior.

Para resolver esse problema, aplique 4340917 de KB [24 de julho de 2018 – KB4340917 (SO Build 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de tela preta

Esse problema ocorre no Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Um usuário inicia os vários aplicativos em uma área de trabalho remota e, em seguida, desconecta da sessão. Periodicamente, o usuário reconecta-se para a área de trabalho remota para interagir com os aplicativos e, em seguida, desconectar-se novamente. Em algum momento, quando o usuário reconecta, a sessão de área de trabalho remota mostra apenas uma tela preta. O usuário deve terminar a sessão de área de trabalho remota do console do computador remoto (ou o console do servidor RDSH). Essa ação interrompe os aplicativos.

Para resolver esse problema, aplique as seguintes atualizações conforme apropriado:

  - Windows 8 e Windows Server 2012: KB4103719, [17 de maio de 2018 – KB4103719 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) e KB 4284863, [21 de junho de 2018 – KB4284863 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Corrigido no KB4284860, [12 de junho de 2018 – KB4284860 (Build do sistema operacional 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
