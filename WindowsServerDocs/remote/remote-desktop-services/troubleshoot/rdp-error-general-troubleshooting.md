---
title: Solução de problemas gerais de conexão de Área de Trabalho Remota
description: Solução do erro “Classe não registrada” com a conexão de Área de Trabalho Remota.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 39b11dac044c38f1ae80d4401fbb66af0317ab56
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870698"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>Solução de problemas gerais de conexão de Área de Trabalho Remota

Use estas etapas quando um cliente de Área de Trabalho Remota não pode se conectar a uma área de trabalho remota, mas não fornece mensagens ou outros sintomas que ajudem a identificar a causa.

## <a name="check-the-status-of-the-rdp-protocol"></a>Verificar o status do protocolo RDP

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Verificar o status do protocolo RDP em um computador local

Para verificar e alterar o status do protocolo RDP em um computador local, confira [Como habilitar a Área de Trabalho Remota](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se as opções de Área de Trabalho Remota não estiverem disponíveis, confira [Verifique se um Objeto de Política de Grupo está bloqueando o RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Verificar o status do RDP em um computador remoto

> [!IMPORTANT]  
> Siga as instruções desta seção com cuidado. Poderão ocorrer problemas sérios se o Registro for modificado incorretamente. Antes de começar a modificar o Registro, [faça backup dele](https://support.microsoft.com/help/322756) para que você possa restaurá-lo caso algo dê errado.

Para verificar e alterar o status do RDP em um computador remoto, use uma conexão de Registro da rede:

1. Primeiro, acesse o menu **Iniciar** e, em seguida, selecione **Executar**. Na caixa de texto exibida, insira **regedt32**.
2. No Editor do Registro, selecione **Arquivo** e, em seguida, **Conectar Registro da Rede**.
3. Na caixa de diálogo **Selecionar Computador**, digite o nome do computador remoto, selecione **Verificar Nomes** e, em seguida, selecione **OK**.
4. Navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor do Registro, mostrando a entrada fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se o valor da chave **fDenyTSConnections** for **0**, o RDP estará habilitado.
   - Se o valor da chave **fDenyTSConnections** for **1**, o RDP estará desabilitado.
5. Para habilitar o RDP, altere o valor de **fDenyTSConnections** de **1** para **0**.

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Verifique se um GPO (Objeto de Política de Grupo) está bloqueando o RDP em um computador local

Se você não puder ativar o RDP na interface do usuário ou se o valor de **fDenyTSConnections** for revertido para **1** depois que você alterá-lo, pode ser que um GPO esteja substituindo as configurações no nível do computador.

Para verificar a configuração da Política de Grupo em um computador local, abra uma janela do prompt de comando como administrador e digite o seguinte comando:

```cmd
gpresult /H c:\gpresult.html
```

Após a conclusão desse comando, abra gpresult.html. Em **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Conexões**, localize a política **Permitir que os usuários se conectem remotamente usando Serviços de Área de Trabalho Remota**.

- Se a configuração para essa política estiver **Habilitada**, a Política de Grupo não está bloqueando as conexões RDP.
- Se a configuração para essa política estiver **Desabilitada**, verifique o **GPO vencedor**. Esse é o GPO que está bloqueando as conexões RDP.
  ![Um segmento de exemplo de gpresult.html, em que o GPO de nível de domínio **Bloquear Protocolo RDP** está desabilitando o RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Um segmento de exemplo de gpresult.html, em **Política de Grupo Local** está desabilitando o RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Verificar se um GPO está bloqueando o RDP em um computador remoto

Para verificar a configuração da Política de Grupo em um computador remoto, o comando é quase o mesmo que para um computador local:

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

O arquivo que esse comando gera (**gpresult-\<nome do computador\>.html**) usa o mesmo formato de informações que a versão do computador local (**gpresult.html**).

### <a name="modifying-a-blocking-gpo"></a>Modificar um GPO de bloqueio

Você pode modificar essas configurações no GPE (Editor de Objeto de Política de Grupo) e no GPM (Console de Gerenciamento de Política de Grupo). Para obter mais informações sobre como usar a Política de Grupo, confira [Gerenciamento Avançado de Política de Grupo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Para modificar a política responsável pelo bloqueio, use um dos seguintes métodos:

- No GPE, acesse o nível adequado do GPO (como local ou domínio) e navegue até **Configuração do Computador** > **Modelos Administrativos** > **Componentes do Windows** > **Serviços de Área de Trabalho Remota** > **Host da Sessão da Área de Trabalho Remota** > **Conexões** > **Permitir que usuários se conectem remotamente usando os Serviços de Área de Trabalho Remota**.  
   1. Defina a política como **Habilitada** ou **Não configurada**.
   2. Nos computadores afetados, abra uma janela de prompt de comando como administrador e execute o comando **gpupdate /force**.
- No GPM, navegue até a UO (unidade organizacional) em que a política de bloqueio é aplicada aos computadores afetados e exclua a política da UO.

## <a name="check-the-status-of-the-rdp-services"></a>Verificar o status dos serviços de RDP

Os serviços a seguir devem estar em execução tanto no computador local (cliente) quanto no computador remoto (destino):

- Serviços de Área de Trabalho Remota (TermService)
- Redirecionador de porta de UserMode de Serviços de Área de Trabalho Remota (UmRdpService)

Você pode usar o snap-in do MMC dos Serviços para gerenciá-los localmente ou remotamente. Você também pode usar o PowerShell para gerenciar os serviços local ou remotamente (se o computador remoto estiver configurado para aceitar cmdlets remotos do PowerShell).

![Serviços de Área de Trabalho Remota no snap-in do MMC de serviços. Não modifique as configurações de serviço padrão.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Em qualquer um dos dois computadores, se um ou ambos os serviços não estiverem em execução, inicie-os.

> [!NOTE]  
> Se você iniciar o serviço de Serviços de Área de Trabalho Remota, clique em **Sim** para reiniciar automaticamente o serviço Redirecionador de Portas UserMode dos Serviços de Área de Trabalho Remota.

## <a name="check-that-the-rdp-listener-is-functioning"></a>Verificar se o ouvinte de RDP está funcionando

> [!IMPORTANT]  
> Siga as instruções desta seção com cuidado. Poderão ocorrer problemas sérios se o Registro for modificado incorretamente. Antes de começar a modificar o Registro, [faça backup dele](https://support.microsoft.com/help/322756) para que você possa restaurá-lo caso algo dê errado.

### <a name="check-the-status-of-the-rdp-listener"></a>Verificar o status do ouvinte de RDP

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos cmdlets funcionam tanto local quanto remotamente.

1. Para conectar-se a um computador remoto, execute o seguinte cmdlet:

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

2. Digite **qwinsta**. 
    ![O comando qwinsta lista os processos que estão escutando nas portas do computador.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Se a lista inclui **rdp-tcp** com um status de **Escutar**, o ouvinte de RDP está funcionando. Prossiga para [Verificar a porta do ouvinte de RDP](#check-the-rdp-listener-port). Caso contrário, prossiga para a etapa 4.
4. Exporte a configuração do ouvinte de RDP de um computador que funcione corretamente.
    1. Entre em um computador que tenha a mesma versão do sistema operacional que o computador afetado e acesse o Registro do computador (por exemplo, usando o Editor do Registro).
    2. Navegue até a seguinte entrada do Registro:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporte a entrada para um arquivo .reg. Por exemplo, no Editor do Registro, clique com o botão direito do mouse na entrada, selecione **Exportar** e, em seguida, insira um nome de arquivo para as configurações exportadas.
    4. Copie o arquivo .reg exportado para o computador afetado.
5. Para importar a configuração do ouvinte de RDP, abra uma janela do PowerShell que tenha permissões administrativas no computador afetado (ou abra a janela do PowerShell e conecte-se remotamente ao computador afetado).
   1. Para fazer backup da entrada do Registro existente, digite o seguinte cmdlet:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para remover a entrada do Registro existente, digite os seguintes cmdlets:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar a nova entrada de Registro e depois reiniciar o serviço, digite os seguintes cmdlets:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      Substitua \<filename\> pelo nome do arquivo. reg exportado.

6. Teste a configuração experimentando novamente a Conexão de Área de Trabalho Remota. Se você ainda não conseguir se conectar, reinicie o computador afetado.
7. Se você ainda não conseguir se conectar, [verifique o status do certificado autoassinado do RDP](#check-the-status-of-the-rdp-self-signed-certificate).

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Verificar o status do certificado autoassinado do RDP

1. Se você ainda não conseguir se conectar, abra o snap-in do MMC dos certificados. Quando solicitado para selecionar o repositório de certificados para gerenciar, selecione **Conta de computador** e, em seguida, selecione o computador afetado.
2. Na pasta **Certificados**, em **Área de Trabalho Remota**, exclua o certificado autoassinado do RDP. 
    ![Certificados da Área de Trabalho Remota no snap-in de certificados MMC.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. No computador afetado, reinicie o serviço Serviços de Área de Trabalho Remota.
4. Atualize o snap-in de certificados.
5. Se o certificado autoassinado de RDP não tiver sido recriado, [verifique as permissões da pasta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Verifique as permissões da pasta MachineKeys

1. No computador afetado, abra o Explorer e, em seguida, navegue até **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Clique com o botão direito do mouse em **MachineKeys**, selecione **Propriedades**, selecione **Segurança** e, em seguida, selecione **Avançado**.
3. Verifique se as seguintes permissões estão configuradas:
      - Builtin\\Administradores: Controle total
      - Todos: Leitura, Gravação

## <a name="check-the-rdp-listener-port"></a>Verificar a porta do ouvinte de RDP

O ouvinte de RDP deve estar escutando na porta 3389 tanto no computador local (cliente) quanto no computador remoto (destino). Nenhum outro aplicativo deve estar usando essa porta.

> [!IMPORTANT]  
> Siga as instruções desta seção com cuidado. Poderão ocorrer problemas sérios se o Registro for modificado incorretamente. Antes de começar a modificar o Registro, [faça backup dele](https://support.microsoft.com/help/322756) para que você possa restaurá-lo caso algo dê errado.

Para verificar ou alterar a porta do RDP, use o Editor do Registro:

1. Acesse o menu Iniciar, selecione **Executar** e, em seguida, digite **REGEDT32** na caixa de texto exibida.
      - Para se conectar a um computador remoto, selecione **Arquivo** e, em seguida, selecione **Conectar Registro da Rede**.
      - Na caixa de diálogo **Selecionar Computador**, digite o nome do computador remoto, selecione **Verificar Nomes** e, em seguida, selecione **OK**.
2. Abra o Registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![A subchave PortNumber para o protocolo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **PortNumber** tiver um valor diferente de **3389**, altere-a para **3389**. 
   > [!IMPORTANT]  
    > Você pode operar os Serviços de Área de Trabalho Remota usando outra porta. No entanto, não recomendamos que você faça isso. Este artigo não aborda como solucionar esse tipo de configuração.
4. Depois de alterar o número da porta, reinicie o serviço de Serviços de Área de Trabalho Remota.

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>Verifique se outro aplicativo não está tentando usar a mesma porta

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos cmdlets funcionam local e remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **Enter-PSSession -ComputerName \<nome do computador\>** .
2. Digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![O comando netstat produz uma lista de portas e de serviços que escutam nessas portas.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Procure uma entrada para a porta TCP 3389 (ou para a porta RDP atribuída) com o status **Escutando**. 
    > [!NOTE]  
   > O PID (identificador do processo) do processo ou do serviço que está usando aquela porta aparece na coluna PID.
4. Para determinar qual aplicativo está usando a porta 3389 (ou a porta RDP atribuída), digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![O comando tasklist relata detalhes de um processo específico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Procure uma entrada para o número do PID que esteja associado à porta (da saída da **netstat**). Os serviços ou processos associados a esse PID aparecem na coluna direita.
6. Se um aplicativo ou serviço que não seja os Serviços de Área de Trabalho Remota (TermServ.exe) está usando a porta, você pode resolver o conflito usando um dos seguintes métodos:
      - Configure o outro aplicativo ou serviço para usar uma porta diferente (recomendado).
      - Desinstale o outro aplicativo ou serviço.
      - Configure o RDP para usar uma porta diferente e, em seguida, reinicie o serviço de Serviços de Área de Trabalho Remota (não recomendado).

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Verificar se um firewall está bloqueando a porta RDP

Use a ferramenta **psping** para testar se você pode alcançar o computador afetado usando a porta 3389.

1. Acesse um computador diferente que não é afetado e baixe **psping** de <https://live.sysinternals.com/psping.exe>.
2. Abra uma janela de prompt de comando como administrador, altere para o diretório no qual você instalou **psping** e, em seguida, digite o seguinte comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Verifique a saída do comando **psping** para resultados como o seguinte:  
      - **Conectar-se ao \<IP do computador\>** : O computador remoto está acessível.
      - **(0% de perda)** : Todas as tentativas de conexão foram bem-sucedidas.
      - **O computador remoto recusou a conexão de rede**: O computador remoto não está acessível.
      - **(100% de perda)** : Todas as tentativas de conexão falharam.
1. Execute **psping** em vários computadores para testar sua capacidade de se conectar ao computador afetado.
1. Observe se o computador afetado bloqueia conexões de todos os outros computadores, alguns outros computadores ou apenas um outro computador.
1. Próximas etapas recomendadas:
      - Entre em contato com seus administradores de rede para verificar se a rede permite tráfego de RDP para o computador afetado.
      - Investigue as configurações de todos os firewalls entre os computadores de origem e o computador afetado (incluindo o Firewall do Windows no computador afetado) para determinar se um firewall está bloqueando a porta do RDP.
