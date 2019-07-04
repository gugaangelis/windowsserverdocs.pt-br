---
title: Como solucionar problemas de conexões de Área de Trabalho Remota
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
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284143"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Como solucionar problemas de conexões de Área de Trabalho Remota
Para obter explicações breves de vários dos problemas mais comuns dos RDS (Serviços de Área de Trabalho Remota), confira [Perguntas frequentes sobre os clientes de Área de Trabalho Remota](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Este artigo descreve várias abordagens mais avançadas para solução de problemas de conexão. Muitos desses procedimentos se aplicam se você está solucionando problemas de uma configuração simples, como um computador físico se conectando a outro computador físico, ou então uma configuração mais complicada. Alguns procedimentos resolvem problemas que ocorrem apenas em cenários multiusuários mais complicados. Para obter mais informações sobre os componentes da área de trabalho remota e como eles funcionam juntos, confira [Arquitetura de Serviços de Área de Trabalho Remota](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muitos dos procedimentos descritos neste artigo exigem que você acesse vários computadores, que alguns dos quais você talvez precise acessar remotamente. Para obter mais informações sobre as ferramentas de administração remota e como configurá-las, confira [RSAT (Ferramentas de Administração de Servidor Remoto) para sistemas operacionais Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Na lista a seguir, identifique o tipo do sintoma que você ou seus usuários estão enfrentando.

- [O cliente de área de trabalho remota não pode se conectar à Área de Trabalho Remota, mas não há sintomas específicos nem mensagens (etapas de solução de problemas gerais)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [O cliente de área de trabalho remota não pode se conectar à Área de Trabalho Remota e recebe uma mensagem de "Classe não registrada"](#client-cannot-connect-class-not-registered)
- [O cliente de Área de Trabalho Remota não pode se conectar à Área de Trabalho Remota e recebe uma mensagem de "Nenhuma licença disponível" ou "Erro de segurança"](#client-cannot-connect-no-licenses-available)
- [O usuário recebe uma mensagem de "Acesso negado" ou precisa fornecer credenciais duas vezes](#user-cannot-authenticate-or-must-authenticate-twice)
- [Ao se conectar, o usuário recebe uma mensagem "O serviço de Área de Trabalho Remota está ocupado no momento"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [O cliente de Área de Trabalho Remota se desconecta e não pode se reconectar à mesma sessão](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [O usuário se conecta a um laptop remoto em uma rede sem fio e, em seguida, o laptop se desconecta da rede](#remote-laptop-disconnects-from-wireless-network).
- [O usuário experimenta um desempenho ruim ou problemas com aplicativos remotos](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obter uma lista atual dos códigos de desconexão de RDP, confira [Enumeração ExtendedDisconnectReasonCode](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Nenhum sintoma ou mensagem específico (etapas de solução de problemas gerais)

Use estas etapas quando um cliente de Área de Trabalho Remota não pode se conectar a uma Área de Trabalho Remota, mas não fornece mensagens ou outros sintomas que ajudem a identificar a causa. Para resolver muitas das causas mais comuns desse tipo de problema, use os seguintes métodos:

- [Verificar o status do protocolo RDP](#check-the-status-of-the-rdp-protocol)
- [Verificar o status dos serviços de RDP](#check-the-status-of-the-rdp-services)
- [Verificar se o ouvinte de RDP está funcionando](#check-that-the-rdp-listener-is-functioning)
- [Verificar a porta do ouvinte de RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Verificar o status do protocolo RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Verificar o status do protocolo RDP em um computador local

Para verificar e alterar o status do protocolo RDP em um computador local, confira [Como habilitar a Área de Trabalho Remota](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se as opções de Área de Trabalho Remota não estiverem disponíveis, confira [Verifique se um Objeto de Política de Grupo está bloqueando o RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Verificar o status do RDP em um computador remoto

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) em caso de problemas.

Para verificar e alterar o status do RDP em um computador remoto, use uma conexão de Registro da rede:

1. Selecione **Iniciar**, depois **Executar** e, em seguida, digite **regedt32**.
2. No Editor do Registro, selecione **Arquivo** e, em seguida, **Conectar Registro da Rede**.
3. Na caixa de diálogo **Selecionar Computador**, digite o nome do computador remoto, selecione **Verificar Nomes** e, em seguida, selecione **OK**.
4. Navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor do Registro, mostrando a entrada fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se o valor da chave **fDenyTSConnections** é **0**, o RDP está habilitado
   - Se o valor da chave **fDenyTSConnections** é **1**, o RDP está desabilitado
5. Para habilitar o RDP, altere o valor de **fDenyTSConnections** de **1** para **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Verifique se um GPO (Objeto de Política de Grupo) está bloqueando o RDP em um computador local

Se você não pode ativar o RDP na interface do usuário ou se o valor de **fDenyTSConnections** for revertido para **1** depois que você alterá-lo, um GPO poder estar substituindo as configurações no nível do computador.

Para verificar a configuração da Política de Grupo em um computador local, abra uma janela do prompt de comando como administrador e digite o seguinte comando:
```
gpresult /H c:\gpresult.html
```
Após a conclusão desse comando, abra gpresult.html. Em **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Conexões**, localize a política **Permitir que os usuários se conectem remotamente usando Serviços de Área de Trabalho Remota**.

- Se a configuração para essa política está **Habilitada**, a Política de Grupo não está bloqueando as conexões RDP.
- Se a configuração para essa política estiver **Desabilitada**, verifique o **GPO vencedor**. Esse é o GPO que está bloqueando as conexões RDP.
  ![Um segmento de exemplo de gpresult.html, em que o GPO de nível de domínio **Bloquear Protocolo RDP** está desabilitando o RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Um segmento de exemplo de gpresult.html, em **Política de Grupo Local** está desabilitando o RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Verificar se um GPO está bloqueando o RDP em um computador remoto

Para verificar a configuração da Política de Grupo em um computador remoto, o comando é quase o mesmo que para um computador local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
O arquivo que esse comando gera (**gpresult-\<nome do computador\>.html**) usa o mesmo formato de informações que a versão do computador local (**gpresult.html**).

#### <a name="modifying-a-blocking-gpo"></a>Modificar um GPO de bloqueio

Você pode modificar essas configurações no GPE (Editor de Objeto de Política de Grupo) e no GPM (Console de Gerenciamento de Política de Grupo). Para obter mais informações sobre como usar a Política de Grupo, confira [Gerenciamento Avançado de Política de Grupo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Para modificar a política responsável pelo bloqueio, use um dos seguintes métodos:

- No GPE, acesse o nível adequado do GPO (tal como local ou domínio) e navegue até **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Conexões**\\**Permitir que os usuários se conectem remotamente usando Serviços de Área de Trabalho Remota**.  
   1. Defina a política para **Habilitado** ou **Não configurado**.
   2. Nos computadores afetados, abra uma janela de prompt de comando como administrador e execute o comando **gpupdate /force**.
- No GPM, navegue até a UO em que a política de bloqueio é aplicada aos computadores afetados e exclua a política da UO.

### <a name="check-the-status-of-the-rdp-services"></a>Verificar o status dos serviços de RDP

Os serviços a seguir devem estar em execução tanto no computador local (cliente) quanto no computador remoto (destino):

- Serviços de Área de Trabalho Remota (TermService)
- Redirecionador de porta de UserMode de Serviços de Área de Trabalho Remota (UmRdpService)

Você pode usar o snap-in do MMC dos Serviços para gerenciá-los localmente ou remotamente. Você também pode usar o PowerShell localmente ou remotamente (se o computador remoto está configurado para aceitar comandos remotos do PowerShell).

![Serviços de Área de Trabalho Remota no snap-in do MMC de serviços. Não modifique as configurações de serviço padrão.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Em qualquer um dos dois computadores, se um ou ambos os serviços não estiverem em execução, inicie-os.

> [!NOTE]  
> Se você iniciar o serviço de Serviços de Área de Trabalho Remota, clique em **Sim** para reiniciar automaticamente o serviço Redirecionador de Portas UserMode dos Serviços de Área de Trabalho Remota.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Verificar se o ouvinte de RDP está funcionando

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) em caso de problemas.

#### <a name="check-the-status-of-the-rdp-listener"></a>Verificar o status do ouvinte de RDP

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos comandos funcionam tanto localmente quanto remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **Enter-PSSession -ComputerName \<nome do computador\>** .
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
   1. Para fazer backup da entrada do Registro existente, digite o seguinte comando:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para remover a entrada do Registro existente, digite os seguintes comandos:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar a nova entrada de Registro e depois reiniciar o serviço, digite os seguintes comandos:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      em que \<filename\> é o nome do arquivo .reg exportado.
6. Teste a configuração experimentando novamente a Conexão de Área de Trabalho Remota. Se você ainda não conseguir se conectar, reinicie o computador afetado.
7. Se você ainda não conseguir se conectar, [verifique o status do certificado autoassinado do RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Verificar o status do certificado autoassinado do RDP

1. Se você ainda não conseguir se conectar, abra o snap-in do MMC dos certificados. Quando solicitado para selecionar o repositório de certificados para gerenciar, selecione **Conta de computador** e, em seguida, selecione o computador afetado.
2. Na pasta **Certificados**, em **Área de Trabalho Remota**, exclua o certificado autoassinado do RDP. 
    ![Certificados da Área de Trabalho Remota no snap-in de certificados MMC.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. No computador afetado, reinicie o serviço Serviços de Área de Trabalho Remota.
4. Atualize o snap-in de certificados.
5. Se o certificado autoassinado de RDP não tiver sido recriado, [verifique as permissões da pasta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Verifique as permissões da pasta MachineKeys

1. No computador afetado, abra o Explorer e, em seguida, navegue até **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Clique com o botão direito do mouse em **MachineKeys**, selecione **Propriedades**, selecione **Segurança** e, em seguida, selecione **Avançado**.
3. Verifique se as seguintes permissões estão configuradas:
      - Builtin\\Administradores: Controle total
      - Todos: Leitura, Gravação

### <a name="check-the-rdp-listener-port"></a>Verificar a porta do ouvinte de RDP

O ouvinte de RDP deve estar escutando na porta 3389 tanto no computador local (cliente) quanto no computador remoto (destino). Nenhum outro aplicativo deve estar usando essa porta.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) em caso de problemas.

Para verificar ou alterar a porta do RDP, use o Editor do Registro:

1. Selecione Iniciar, depois **Executar** e, em seguida, digite **regedt32**.
      - Para se conectar a um computador remoto, selecione **Arquivo** e, em seguida, selecione **Conectar Registro da Rede**.
      - Na caixa de diálogo **Selecionar Computador**, digite o nome do computador remoto, selecione **Verificar Nomes** e, em seguida, selecione **OK**.
2. Abra o Registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![A subchave PortNumber para o protocolo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **PortNumber** tiver um valor diferente de **3389**, altere-a para **3389**. 
   > [!IMPORTANT]  
    > Você pode operar os Serviços de Área de Trabalho Remota usando outra porta. No entanto, não recomendamos que você faça isso. Solucionar problemas de uma configuração como essa está além do escopo deste artigo.
4. Depois de alterar o número da porta, reinicie o serviço de Serviços de Área de Trabalho Remota.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Verificar se outro aplicativo não está tentando usar a mesma porta

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos comandos funcionam tanto localmente quanto remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **Enter-PSSession -ComputerName \<nome do computador\>** .
2. Digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![O comando netstat produz uma lista de portas e de serviços que escutam nessas portas.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Procure uma entrada para a porta TCP 3389 (ou para a porta RDP atribuída) com o status **Escutando**. 
    > [!NOTE]  
   > O PID (identificador do processo) do processo ou serviço que está usando aquela porta aparece na coluna PID.
4. Para determinar qual aplicativo está usando a porta 3389 (ou a porta RDP atribuída), digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![O comando tasklist relata detalhes de um processo específico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Procure uma entrada para o número do PID que esteja associado à porta (da saída da **netstat**). Os serviços ou processos associados àquele PID aparecem à direita.
6. Se um aplicativo ou serviço que não seja os Serviços de Área de Trabalho Remota (TermServ.exe) está usando a porta, você pode resolver o conflito usando um dos seguintes métodos:
      - Configure o outro aplicativo ou serviço para usar uma porta diferente (recomendado).
      - Desinstale o outro aplicativo ou serviço.
      - Configure o RDP para usar uma porta diferente e, em seguida, reinicie o serviço de Serviços de Área de Trabalho Remota (não recomendado).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Verificar se um firewall está bloqueando a porta RDP

Use a ferramenta **psping** para testar se você pode alcançar o computador afetado usando a porta 3389.

1. Em um computador diferente do computador afetado, baixe **psping** de <https://live.sysinternals.com/psping.exe>.
2. Abra uma janela de Prompt de comando como administrador, altere para o diretório no qual você instalou **psping** e, em seguida, digite o seguinte comando:  
   
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

## <a name="client-cannot-connect-class-not-registered"></a>O cliente não consegue conectar, "Classe não registrada"

Quando você tenta se conectar a um computador remoto usando um cliente que está executando o Windows 10 versão 1709 ou posterior, o cliente não pode se conectar enquanto o servidor Host da Sessão da Área de Trabalho Remota relata uma mensagem que contém o código de erro "Classe não registrada (0x80040154)".

Esse problema ocorre se o usuário que está tentando se conectar tem um perfil do usuário obrigatório. Para resolver esse problema, instale a [atualização do Windows 10 de 24 de julho de 2018 – KB4338817 (Build do SO 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817).

## <a name="client-cannot-connect-no-licenses-available"></a>O cliente não consegue se conectar, não há licenças disponíveis

Essa situação se aplica a implantações que incluem um servidor RDSH e um servidor de Licenciamento de Área de Trabalho Remota.

Identifique qual tipo de comportamento os usuários estão vendo:

- A sessão foi desconectada porque não há licenças disponíveis ou nenhum servidor de licença está disponível
- O acesso foi negado devido a um erro de segurança

Entre no Host da Sessão RD como um administrador de domínio e abra o Diagnosticador de Licença da Área de Trabalho Remota. Procure mensagens como a seguinte:

  - O período de cortesia para o computador remoto que o servidor de Host da Sessão RD expirou, mas o servidor Host da Sessão da Área de Trabalho Remota não foi configurado com nenhum servidor de licença. Conexões com o servidor Host da Sessão da Área de Trabalho Remota serão negadas, a menos que um servidor de licença esteja configurado para o servidor Host da Sessão da Área de Trabalho Remota.
  - O servidor de licença \<nome do computador\> não está disponível. Isso pode ser causado por problemas de conectividade de rede, pelo serviço de Licenciamento da Área de Trabalho Remota ter sido interrompido no servidor de licença ou pelo Licenciamento de Área de Trabalho Remota não estar mais instalado no computador.

Esses problemas tendem a ser associados com as seguintes mensagens de usuário:

  - A sessão remota foi desconectada porque não há licenças de acesso para cliente de Área de Trabalho Remota disponíveis para este computador.
  - A sessão remota foi desconectada porque não há servidores de licença Área de Trabalho Remota disponíveis para fornecer uma licença.

Nesse caso, [configure o serviço de Licenciamento de Área de Trabalho Remota](#configure-the-rd-licensing-service).

Se o Diagnosticador de Licença da Área de Trabalho Remota lista outros problemas, tais como "O componente de protocolo RDP X.224 detectou um erro no fluxo do protocolo e desconectou o cliente," pode haver um problema que afeta os certificados de licença. Esses problemas tendem a ser associados com mensagens de usuário, tais como as seguintes:

Devido a um erro de segurança, o cliente não pôde se conectar ao servidor Host da Sessão da Área de Trabalho Remota. Após verificar se você está conectado à rede, tente se conectar ao servidor novamente.

Nesse caso, [atualize as chaves do Registro de Certificado X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurar o serviço de Licenciamento de Área de Trabalho Remota

O procedimento a seguir usa o Gerenciador do Servidor para fazer as alterações de configuração. Para obter informações sobre como configurar e usar o Gerenciador do Servidor, confira [Gerenciador do Servidor](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager).

1. Abra o Gerenciador do Servidor e, em seguida, navegue até **Serviços de Área de Trabalho Remota**.
2. Em **Visão Geral da Implantação**, selecione **Tarefas** e, em seguida, selecione **Editar Propriedades de Implantação**.
3. Selecione **Licenciamento de Área de Trabalho Remota** e, em seguida, selecione o modo de licenciamento apropriado para sua implantação (**Por Dispositivo** ou **Por Usuário**).
4. Insira o FQDN (nome de domínio totalmente qualificado) do seu servidor de Licença de Área de Trabalho Remota e, em seguida, selecione **Adicionar**.
5. Se você tiver mais de um servidor de Licença de Área de Trabalho Remota, repita a etapa 4 para cada servidor. 
    ![Opções de configuração de servidor de Licença de Área de Trabalho Remota no Gerenciador do Servidor.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Atualizar as chaves do Registro de Certificado X509

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) em caso de problemas.

Para resolver esse problema, faça backup das chaves do Registro de Certificado X509 e então remova-as, reinicie o computador e, em seguida, reative o servidor de Licenciamento de Área de Trabalho Remota. Para fazer isso, siga estas etapas:

> [!NOTE]  
> Execute o seguinte procedimento em cada um dos servidores do RDSH.

1. Abra o Editor do Registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. No menu Registro, selecione **Exportar Arquivo do Registro**.
3. Digite **exportado – Certificado** na caixa **Nome do arquivo** e, em seguida, selecione **Salvar**.
4. Clique com o botão direito do mouse em cada um dos valores a seguir, selecione **Excluir** e, em seguida, selecione **Sim** para verificar a exclusão:  
      - **Certificado**
      - **Certificado X509**
      - **ID do Certificado X509**
      - **Certificado X509 2**
5. Saia do Editor de Registro e reinicie o servidor RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>O usuário não pode se autenticar ou deve se autenticar duas vezes

Há várias questões que podem causar problemas que afetam a autenticação do usuário. Esta seção aborda os seguintes casos:

  - [O acesso é negado a um usuário com uma mensagem "tipo de logon restrito"](#access-denied-restricted-type-of-logon)
  - [O acesso é negado a um usuário ou aplicativo com o evento 16965, "Uma chamada remota para o banco de dados SAM foi negada"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Um usuário não consegue entrar usando um cartão inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se o computador remoto está bloqueado, um usuário precisa inserir uma senha duas vezes](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Um usuário não consegue entrar e recebe as mensagens de "Erro de autenticação" e "Correção de Oráculo de Criptografia CredSSP"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Depois de você atualizar os computadores cliente, alguns usuários precisam entrar duas vezes](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [O acesso é negado aos usuários em uma implantação que usa RemoteGuard com vários agentes de Conexão de Área de Trabalho Remota](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Acesso negado, tipo de logon restrito

Nessa situação, o acesso é negado a um usuário do Windows 10 tentando se conectar a computadores Windows 10 ou Windows Server 2016, com a seguinte mensagem:

> Conexão de Área de Trabalho Remota:  
> O administrador do sistema restringiu o tipo de logon (de rede ou interativo) que você pode usar. Para obter assistência, entre em contato com o administrador do sistema ou o suporte técnico.

Esse problema ocorre quando a NLA (Autenticação no Nível da Rede) é necessária para conexões RDP e o usuário não é um membro do grupo **Usuários de Área de Trabalho Remota**. Ele também pode ocorrer se o grupo **Usuários de Área de Trabalho Remota** não foi atribuído ao direito de usuário **Acessar este computador pela rede**.

Para corrigir esse problema, siga uma destas etapas:

  - [Modifique a associação do grupo do usuário ou a atribuição de direitos de usuário](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desative a NLA (não recomendado).
  - Use clientes de Área de Trabalho Remota que não executem o Windows 10. Por exemplo, clientes executando Windows 7 não apresentam esse problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modifique a associação do grupo do usuário ou a atribuição de direitos de usuário

Se esse problema afeta um único usuário, a solução mais simples para esse problema é adicionar o usuário ao grupo **Usuários de Área de Trabalho Remota**.

Se o usuário já for um membro desse grupo (ou se vários membros do grupo tiverem o mesmo problema), verifique a configuração de direitos de usuário no computador remoto executando Windows 10 ou Windows Server 2016.

1. Abra o GPE (Editor de Objeto de Política de Grupo) e conecte-se à política local do computador remoto.
2. Navegue até **Configuração do Computador\\Configurações do Windows\\Configurações de Segurança\\Políticas Locais\\Atribuição de Direitos do Usuário**, clique com o botão direito do mouse em **Acessar este computador da rede** e, em seguida, selecione **Propriedades**.
3. Verifique a lista de usuários e grupos para **Usuários da Área de Trabalho Remota** (ou um grupo pai).
4. Se a lista não inclui **Usuários da Área de Trabalho Remota** (ou um grupo pai, assim como **Todas as pessoas**), você precisa adicioná-lo à lista (se você tem mais de um ou dois computadores em sua implantação, use um objeto de política de grupo).  
    Por exemplo, a associação padrão para **Acessar este computador da rede** inclui **Todas as pessoas**. Se a implantação usa um objeto de política de grupo para remover **Todas as pessoas**, talvez você precise restaurar o acesso ao atualizar o objeto de política de grupo para adicionar **Usuários da Área de Trabalho Remota**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Acesso negado, uma chamada remota para o banco de dados SAM foi negada

Esse comportamento tem mais chance de ocorrer se os controladores de domínio estão executando o Windows Server 2016 ou posterior e os usuários tentam se conectar por meio de um aplicativo de conexão personalizada. Em particular, aplicativos que acessam informações de perfil do usuário no Active Directory terão o acesso negado.

Esse comportamento resulta de uma alteração no Windows. No Windows Server 2012 R2 e versões anteriores, quando um usuário faz logon em uma Área de Trabalho Remota, o RCM (Gerenciador de Conexão Remota) contata o CD (controlador de domínio) para consultar as configurações que são específicas à Área de Trabalho Remota no objeto de usuário nos AD DS (Active Directory Domain Services). Essas informações são exibidas na guia Perfil de Serviços de Área de Trabalho Remota das propriedades do objeto de um usuário no snap-in do MMC de Usuários e Computadores do Active Directory.

No Windows Server 2016 e versões posteriores, o RCM não consulta mais o objeto de usuários no AD DS. Se precisar que o RCM consulte o AD DS porque você está usando os atributos de Serviços de Área de Trabalho Remota, você deverá habilitar manualmente o comportamento anterior do RCM.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) em caso de problemas.

Para habilitar o comportamento herdado do RCM em um servidor Host da Sessão da Área de Trabalho Remota, configure as entradas do Registro a seguir e, em seguida, reinicie o serviço **Serviços de Área de Trabalho Remota**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Nome da Winstation\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar o comportamento herdado do RCM em um servidor que não seja o Host da Sessão da Área de Trabalho Remota, configure essas entradas do Registro e a entrada do Registro adicional a seguir (e depois reinicie o serviço):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para obter mais informações sobre esse comportamento, confira o KB 3200967 [Alterações ao Gerenciador de Conexões Remotas no Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Um usuário não consegue entrar usando um cartão inteligente

Este artigo aborda três circunstâncias comuns em que um usuário não consegue entrar em uma Área de Trabalho Remota usando um cartão inteligente:  
  - [O usuário não consegue entrar em uma filial que usa um RODC (controlador de domínio somente leitura)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [O usuário não consegue entrar em um computador executando Windows Server 2008 SP2 que foi atualizado recentemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [O usuário não consegue permanecer conectado a um computador Windows que foi atualizado recentemente e o serviço dos Serviços de Área de Trabalho Remota deixam de responder (travam)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Não é possível entrar com um cartão inteligente em uma filial com um RODC

Esse problema ocorre em implantações que incluem um servidor RDSH em um site de filial que usa um RODC. O servidor de RDSH é hospedado no domínio raiz. Os usuários do site de filial pertencem a um domínio filho e usam cartões inteligentes para autenticação. O RODC está configurado para armazenar senhas de usuário no cache (o RODC pertence ao **Grupo de Replicação de Senha RODC Permitido**). Quando os usuários tentarem entrar em sessões no servidor RDSH, eles receberão mensagens como "A tentativa de logon é inválida. Isso é devido a uma informação de autenticação ou nome de usuário inválida".

Esse é um problema conhecido no modo em que o controlador de domínio raiz e o RODC gerenciam a criptografia das credenciais do usuário. O controlador de domínio raiz usa uma chave de criptografia para criptografar as credenciais e o RODC fornece uma chave de descriptografia ao cliente. No entanto, as duas chaves não correspondem.

Para contornar esse problema, considere alterar sua topologia de controlador de domínio, seja desativando o cache de senhas no RODC ou implantando um controlador de domínio gravável no site de filial. Um método alternativo é mover o servidor RDSH para o mesmo domínio filho que os usuários. Outra alternativa é permitir que usuários entrem sem cartões inteligentes. Qualquer uma dessas soluções pode exigir prejuízos ao desempenho ou ao nível de segurança.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Não é possível entrar com um cartão inteligente em um computador executando Windows Server 2008 SP2

Esse problema ocorre quando os usuários entram em um computador executando Windows Server 2008 SP2 que foi atualizado com KB4093227 (2018.4B). Quando os usuários tentam entrar usando um cartão inteligente, o acesso é negado a eles com mensagens como "Nenhum certificado válido encontrado. Verifique se o cartão está inserido corretamente e se encaixa perfeitamente." Ao mesmo tempo, o computador com Windows Server registra o evento de aplicativo "Ocorreu um erro ao recuperar um certificado digital do cartão inteligente inserido. Assinatura inválida."

Para resolver esse problema, atualize o computador com Windows Server com o relançamento 2018.06 B do KB 4093227, [Descrição da atualização de segurança para a vulnerabilidade de negação de serviço do protocolo RDP no Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Não é possível permanecer conectado com um cartão inteligente e o serviço de Serviços de Área de Trabalho Remota trava

Esse problema ocorre quando os usuários entram em um computador executando Windows ou Windows Server que foi atualizado com KB 4056446. Primeiro, o usuário poderá conseguir entrar no sistema usando um cartão inteligente, mas, em seguida, receberá uma mensagem de erro "SCARD\_E\_NO\_SERVICE". O computador remoto poderá parar de responder.

Para contornar esse problema, reinicie o computador remoto.

Para resolver esse problema, atualize o computador remoto com a correção apropriada:

  - Windows Server 2008 SP2: KB 4090928, [O Windows vaza identificadores no processo lsm.exe e aplicativos de cartão inteligente podem exibir erros "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versão 1607: KB 4103720, [17 de maio de 2018 – KB4103720 (Build do sistema operacional 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Se o computador remoto está bloqueado, um usuário precisa inserir uma senha duas vezes

Esse problema pode ocorrer quando um usuário tenta se conectar a uma Área de Trabalho Remota que está executando o Windows 10 versão 1709 em uma implantação em que as conexões RDP não exigem NLA. Sob essas condições, se a Área de Trabalho Remota tiver sido bloqueada, o usuário precisará inserir suas credenciais duas vezes ao se conectar.

Para resolver esse problema, atualize o computador que executa o Windows 10 versão 1709 com o KB 4343893 de [30 de agosto de 2018 – KB4343893 (SO Build 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Um usuário não consegue entrar e recebe as mensagens de "Erro de autenticação" e "Correção de Oráculo de Criptografia CredSSP"

Quando os usuários tentam entrar usando qualquer versão do Windows, do Windows Vista SP2 e versões posteriores ou do Windows Server 2008 SP2 e versões posteriores, o acesso é negado a eles, que recebem mensagens como a seguinte:

  - Ocorreu um erro de autenticação. A função solicitada não é compatível.
  - Isso pode ser devido a Correção de Oráculo de Criptografia CredSSP

"Correção de Oráculo de Criptografia CredSSP" refere-se a um conjunto de atualizações de segurança lançado em março, abril e maio de 2018. O CredSSP é um provedor de autenticação que processa solicitações de autenticação para outros aplicativos. A atualização "3B" de 13 de março de 2018 e as atualizações subsequentes resolveram uma exploração em que um invasor podia retransmitir credenciais do usuário para executar o código no sistema de destino.

As atualizações inicias adicionaram suporte para um novo objeto de política de grupo, **Correção do Oráculo de Criptografia**, que tem as seguintes configurações possíveis:

  - **Vulnerável**. Aplicativos cliente que usam o CredSSP podem reverter para versões não seguras, mas esse comportamento expõe as Áreas de Trabalho Remotas a ataques. Serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Atenuado**. Aplicativos cliente que usam o CredSSP não podem reverter para versões inseguras, mas os serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Forçar Clientes Atualizados**. Aplicativos cliente que usam o CredSSP não podem reverter para versões inseguras e os serviços que usam o CredSSP não aceitam clientes sem patch. 
    Observação: Essa configuração não deve ser implantada até que todos os hosts remotos sejam compatíveis com a versão mais recente.

A atualização de 8 de maio de 2018 alterou a configuração **Correção do Oráculo de Criptografia** padrão de **Vulnerável** para **Atenuado**. Com essa alteração em vigor, os clientes da Área de Trabalho Remota que têm as atualizações não conseguem se conectar a servidores que não as têm (nem a servidores atualizados que não foram reiniciados). Para obter mais informações sobre os efeitos de atualizações e os tipos de comunicação que elas bloqueiam, confira KB 4093492, [Atualizações de CredSSP para CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver esse problema, verifique se todos os sistemas estão atualizados e foram reiniciados. Para obter uma lista completa de atualizações e obter mais informações sobre vulnerabilidades, confira [CVE-2018-0886 | Vulnerabilidade de execução de código remoto de CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Para contornar esse problema até as atualizações serem concluídas, confira o KB 4093492 para os tipos permitidos de conexões. Se não há nenhuma alternativa viável, você pode considerar um dos seguintes métodos:

- Para computadores cliente afetados, defina a política de **Correção do Oráculo de Criptografia** de volta como **Vulnerável**.
- Modifique as políticas a seguir na pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Segurança**:  
  - **Exigir o uso de camada de segurança específica para conexões remotas (RDP)** : defina para **Habilitado** e selecione **RDP**.
  - **Exigir autenticação de usuário para conexões remotas usando a Autenticação no Nível da Rede**: defina para **Desabilitado**.
    > [!IMPORTANT]  
    > Essas modificações reduzem a segurança da sua implantação. Elas deverão ser apenas temporárias se você chegar a usá-las.

Para obter mais informações sobre como trabalhar com a política de grupo, confira [Modificar um GPO de bloqueio](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Depois de você atualizar os computadores cliente, alguns usuários precisam entrar duas vezes

Os usuários em alguma versão do Windows 7 ou Windows 10 1709 entram em uma sessão de Área de Trabalho Remota e veem imediatamente uma segunda tela de entrada. Esse problema ocorre se os computadores cliente receberam as seguintes atualizações:

  - Windows 7: KB 4103718, [8 de maio de 2018 – KB4103718 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de maio de 2018 – KB4103727 (Build do sistema operacional 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver esse problema, verifique se os computadores com os quais os usuários desejam se conectar (bem como os servidores RDSH ou RDVI) estão totalmente atualizados até junho de 2018. Isso inclui as seguintes atualizações:

  - Windows Server 2016: KB 4284880, [12 de junho de 2018 – KB4284880 (Build do sistema operacional 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junho de 2018 – KB4284815 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junho de 2018 – KB4284855 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junho de 2018 – KB4284826 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descrição da atualização de segurança para a vulnerabilidade de execução de código remoto do CredSSP no Windows Server 2008, Windows Embedded POSReady 2009 e Windows Embedded Standard 2009: 13 de março de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>O acesso é negado aos usuários em uma implantação que usa o Credential Guard remoto com vários agentes de Conexão de Área de Trabalho Remota

Esse problema ocorre em implantações de alta disponibilidade que usam dois ou mais agentes de Conexão de Área de Trabalho Remota se o Credential Guard remoto do Windows Defender está em uso. Os usuários não podem entrar em Áreas de Trabalho Remotas.

Esse problema ocorre porque o Credential Guard remoto usa o Kerberos para autenticação e restringe o NTLM. No entanto, em uma configuração de alta disponibilidade com balanceamento de carga, os agentes de Conexão de Área de Trabalho Remota não podem dar suporte a operações do Kerberos.

Se você precisar usar uma configuração de alta disponibilidade com agentes de Conexão de Área de Trabalho Remota com balanceamento de carga, poderá contornar esse problema desabilitando o Credential Guard remoto. Para obter mais informações sobre como gerenciar o Credential Guard remoto do Windows Defender, confira [Proteger credenciais da Área de Trabalho Remota com o Credential Guard remoto do Windows Defender](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Ao se conectar, o usuário recebe uma mensagem "O serviço de Área de Trabalho Remota está ocupado no momento"

Para determinar uma resposta apropriada para esse problema, use as seguintes etapas:

1. O serviço dos Serviços de Área de Trabalho Remota para de responder (por exemplo, o cliente de Área de Trabalho Remota parece "congelar" na tela de boas-vindas).  
      - Se o serviço deixar de responder, confira [Problema de memória do servidor RDSH](#rdsh-server-memory-issue).
      - Se o cliente parece estar interagindo normalmente com o serviço, prossiga para a próxima etapa.
2. Se um ou mais usuários desconectarem as respectivas sessões de Área de Trabalho Remota, eles poderão se conectar novamente?  
   - Se o serviço continuar a negar conexões independentemente de quantos usuários desconectam suas sessões, confira [Problema de ouvinte de Área de Trabalho Remota](#rd-listener-issue).
   - Se o serviço começar a aceitar conexões novamente após um número de usuários terem suas sessões desconectadas, [verifique a política de limite de conexões](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema de memória do servidor RDSH

Foi encontrado um vazamento de memória em alguns servidores do Windows Server 2012 R2 RDSH. Ao longo do tempo, esses servidores começam se recusar conexões de Área de Trabalho Remota e entradas do console local com mensagens como a seguinte:

> A tarefa que você está tentando realizar não pode ser concluída porque o Serviço de Área de Trabalho Remota está ocupado no momento. Tente novamente em alguns minutos. Outros usuários ainda devem ser capazes de entrar.

Clientes de Área de Trabalho Remota tentando se conectar também param de responder.

Para contornar esse problema, reinicie o servidor RDSH.

Para resolver esse problema, aplique o KB 4093114, [10 de abril de 2018 – KB4093114 (pacote cumulativo de atualizações mensal)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)) aos servidores RDSH.

### <a name="rd-listener-issue"></a>Problema de ouvinte de Área de Trabalho Remota

Um problema foi observado em alguns servidores RDSH que foram atualizados diretamente do Windows Server 2008 R2 para o Windows Server 2012 R2 ou o Windows Server 2016. Quando um cliente de Área de Trabalho Remota se conecta ao servidor RDSH, o servidor RDSH cria um ouvinte de área de trabalho remota para a sessão do usuário. Os servidores afetados mantêm uma contagem dos ouvintes de Área de Trabalho Remota que aumenta à medida que os usuários se conectam, mas nunca diminui.

Para solucionar esse problema, é possível usar os métodos a seguir:

  - Reiniciar o servidor RDSH para redefinir a contagem de ouvintes de Área de Trabalho Remota
  - Modificar a política de limite de conexões, definindo-a como um valor muito grande. Para obter mais informações sobre como gerenciar a política de limite de conexões, confira [Verificar a política de limite de conexões](#check-the-connection-limit-policy).

Para resolver esse problema, aplique as seguintes atualizações aos servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Verifique a política de limite de conexões

Você pode definir o limite para o número de conexões simultâneas de Área de Trabalho Remota no nível de computador individual ou por meio da configuração de um GPO (objeto de política de grupo). Por padrão, o limite não é definido.

Para verificar as configurações atuais e identificar quaisquer GPOs existentes no servidor de RDSH, abra uma janela de prompt de comando como administrador e digite o seguinte comando:
  
```
gpresult /H c:\gpresult.html
```
   
Após a conclusão desse comando, abra gpresult.html e, em **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Conexões**, localize a política **Limitar o número de conexões**.

  - Se a configuração para essa política está **Desabilitada**, a Política de Grupo não está limitando as conexões RDP.
  - Se a configuração para essa política estiver **Habilitada**, verifique o **GPO vencedor**. Se você precisar remover ou alterar o limite de conexões, edite esse GPO.

Para aplicar alterações de política, abra uma janela de prompt de comando no computador afetado e digite o seguinte comando:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>O cliente de Área de Trabalho Remota se desconecta e não pode se reconectar à mesma sessão

Depois que o cliente de Área de Trabalho Remota perde sua conexão à Área de Trabalho Remota, o cliente não é capaz de se reconectar imediatamente. O usuário recebe mensagens de erro como as seguintes:

  - Devido a um erro de segurança, o cliente não pôde se conectar ao servidor Host da Sessão da Área de Trabalho Remota. Após verificar se você está conectado à rede, tente se conectar ao servidor novamente.
  - A Área de Trabalho Remota foi desconectada. Devido a um erro de segurança, o cliente não pôde se conectar ao computador remoto. Verifique se você está conectado à rede e, em seguida, tente se conectar novamente.

Em um momento posterior, o cliente de Área de Trabalho Remota se reconecta à Área de Trabalho Remota. No entanto, em vez de conectar o cliente à sessão original, o servidor de RDSH conecta o cliente a uma nova sessão. Uma verificação do servidor RDSH revela que a sessão original não entrou em um estado desconectado, mas em vez disso, permanece ativa.

Para contornar esse problema, você pode habilitar a política **Configurar intervalo de conexão keep alive** na pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host de Sessão de Área de Trabalho Remota\\Conexões**. Se habilitar essa política, você deverá inserir um intervalo de keep alive. O intervalo de keep alive determina a frequência, em minutos, com que o servidor verifica o estado de sessão.

Esse problema pode ser causado por definições de autenticação e de configuração incorretas. Você pode definir essas configurações no nível do servidor ou usando GPOs. As configurações de política de grupo estão disponíveis na pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Segurança**.

1. No servidor de Host da Sessão RD, abra a Configuração de Host da Sessão da Área de Trabalho Remota.
2. Em **Conexões**, clique com o botão direito do mouse no nome da conexão e clique em **Propriedades**.
3. Na caixa de diálogo **Propriedades** da conexão, na guia **Geral**, na camada **Segurança**, selecione um método de segurança.
4. Em **Nível de criptografia**, clique no nível desejado. Você pode selecionar **Baixa**, **Compatível com o Cliente**, **Alta** ou **Em Conformidade com FIPS**.

> [!NOTE]  
>  - Quando a comunicação entre clientes e servidores de Host da Sessão RD exige o nível mais alto de criptografia, use a criptografia Em Conformidade com FIPS.
>  - As configurações de nível de criptografia que você define na Política de Grupo substituem a configuração que você define usando a ferramenta de Configuração de Serviços de Área de Trabalho Remota. Além disso, se você habilitar a política [Criptografia do sistema: Usar algoritmos em conformidade com FIPS para criptografia, hash e assinatura](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)), essa configuração substituirá a política **Definir o nível de criptografia da conexão do cliente**. A política de criptografia do sistema está na pasta **Configuração do computador\\Configurações do Windows\\Configurações de Segurança\\Políticas Locais\\Opções de Segurança**.
>  - Quando você alterar o nível de criptografia, o novo nível de criptografia terá efeito da próxima vez que um usuário fizer o logon. Se você necessitar de vários níveis de criptografia em um servidor, instale vários adaptadores de rede e configure cada adaptador separadamente.
>  - Para verificar se o certificado tem uma chave privada correspondente, na configuração de Serviços de Área de Trabalho Remota, clique com o botão direito do mouse na conexão para a qual você deseja exibir o certificado, selecione **Geral**, selecione **Editar**, selecione o certificado que você deseja exibir e, em seguida, selecione **Exibir Certificado**. Na parte inferior da guia **Geral**, a instrução "Você tem uma chave privada que corresponde a este certificado" deve aparecer. Você também pode exibir essas informações usando o snap-in de certificados.
>  - A criptografia em conformidade com FIPS (a política **Criptografia do sistema: Usar algoritmos em conformidade com FIPS para criptografia, hash e assinatura** ou a configuração **Em Conformidade com FIPS** na Configuração de Servidor de Área de Trabalho Remota) criptografa e descriptografa os dados enviados do cliente para o servidor e do servidor para o cliente, com os algoritmos de criptografia 140-1 padrão FIPS, usando módulos criptográficos da Microsoft. Para obter mais informações, confira [Validação do FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - A configuração **Alta** criptografa os dados enviados do cliente para o servidor e do servidor para o cliente usando criptografia forte de 128 bits.
>  - A configuração **Compatível com o Cliente** criptografa os dados enviados entre o cliente e servidor com a força de chave máxima compatível com o cliente.
>  - A configuração **Baixa** criptografa os dados enviados do cliente ao servidor usando criptografia de 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Um laptop remoto se desconecta da rede sem fio

Esse problema pode ocorrer quando um cliente da Área de Trabalho Remota se conecta a um computador laptop usando uma rede sem fio 802.1x. Intermitentemente, o laptop se desconecta da rede sem fio e não se reconecta automaticamente.

Esse é um problema conhecido que ocorre quando a configuração de autenticação de rede para a conexão de rede sem fio é **Autenticação de usuário**.

Para contornar esse problema, defina a configuração de autenticação de rede como **Autenticação do usuário ou do computador** ou **Autenticação do computador**.

 > [!NOTE]  
> Para alterar as configurações de autenticação de rede em um único computador, talvez você precise usar o painel de controle da Central de Rede e Compartilhamento para criar uma conexão sem fio com as novas configurações.

Para obter uma descrição completa de como definir as configurações de rede sem fio usando GPOs, confira [Configurar Políticas de Rede Sem Fio (IEEE 802.11)](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>O usuário experimenta um desempenho ruim ou problemas com aplicativos

Esta seção aborda vários problemas comuns que os usuários podem enfrentar ao usarem a funcionalidade da Área de Trabalho Remota:

  - [Os usuários que se conectam a novas máquinas virtuais do Microsoft Azure intermitentemente experimentam problemas](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Os usuários que se conectam a Áreas de Trabalho Remotas do Windows 10 versão 1709 podem ter problemas para reproduzir vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Os usuários com perfis de usuário somente leitura que se conectam a Áreas de Trabalho Remotas do Windows 10 não conseguem compartilhar suas áreas de trabalho.](#desktop-sharing-issues-on-windows-10)
  - [Os usuários que se conectam a Áreas de Trabalho Remotas do Windows 10 usando clientes que executam versões mais antigas do Windows 10 enfrentam um desempenho ruim quando o NLA está desabilitado](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando os usuários executam vários aplicativos em sessões de Área de Trabalho Remota e desconectam-se e reconectam-se com frequência, eles podem se deparar com uma tela preta ao se reconectarem.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes com novas máquinas de virtuais do Microsoft Azure

Esse problema afeta as máquinas virtuais que foram provisionadas recentemente. Depois que o usuário se conecta à máquina virtual, a sessão de Área de Trabalho Remota pode não carregar todas as configurações do usuário corretamente.

Para contornar esse problema, desconecte a máquina virtual, aguarde pelo menos 20 minutos e, em seguida, conecte-se novamente.

Para resolver esse problema, aplique as atualizações a seguir às máquinas virtuais, conforme apropriado:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reprodução de vídeo no Windows 10 versão 1709

Esse problema ocorre quando os usuários se conectam a computadores remotos que executam o Windows 10, versão 1709. Quando esses usuários reproduzem vídeo usando o codec VMR9 (Video Mixing Renderer 9), o player mostra apenas uma janela preta.

Esse é um problema conhecido no Windows 10, versão 1709. O problema não ocorre no Windows 10 versão 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemas de compartilhamento de área de trabalho no Windows 10

Esse problema ocorre quando o usuário tem um perfil do usuário somente leitura (e o hive do Registro associado), assim como em um cenário de quiosque. Quando esse usuário se conecta a um computador remoto que está executando o Windows 10 versão 1803, ele não pode compartilhar sua área de trabalho.

Para corrigir esse problema, aplique a atualização 4340917 do Windows 10, de [24 de julho de 2018 – KB4340917 (build do sistema operacional 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de desempenho ao misturar versões do Windows 10 se o NLA está desabilitado

Esse problema ocorre quando o NLA está desabilitado e os computadores cliente da Área de Trabalho Remota que executam o Windows 10 se conectam a Áreas de Trabalho Remotas que executam versões diferentes do Windows 10. Usuários de clientes da Área de Trabalho Remota em computadores que executam o Windows 10 versão 1709 ou versões anteriores enfrentam um desempenho ruim quando se conectam a Áreas de Trabalho Remotas que executam o Windows 10 versão 1803 ou posterior.

Isso ocorre porque quando o NLA está desabilitado, os computadores cliente mais antigos usam um protocolo mais lento ao se conectarem ao Windows 10 versão 1803 ou posterior.

Para resolver esse problema, aplique o KB 4340917 de [24 de julho de 2018 – KB4340917 (Build do sistema operacional 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de tela preta

Esse problema ocorre no Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Um usuário inicia vários aplicativos em uma Área de Trabalho Remota e, em seguida, desconecta-se da sessão. Periodicamente, o usuário reconecta-se à Área de Trabalho Remota para interagir com os aplicativos e, em seguida, desconecta-se novamente. Em algum momento, quando o usuário reconecta, a sessão de Área de Trabalho Remota mostra apenas uma tela preta. O usuário precisa encerrar a sessão de Área de Trabalho Remota do console do computador remoto (ou o console do servidor RDSH). Essa ação interrompe os aplicativos.

Para resolver esse problema, aplique as seguintes atualizações conforme apropriado:

  - Windows 8 e Windows Server 2012: KB4103719, [17 de maio de 2018 – KB4103719 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) e KB 4284863, [21 de junho de 2018 – KB4284863 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Corrigido no KB4284860, [12 de junho de 2018 – KB4284860 (Build do sistema operacional 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
