---
title: Solucionando problemas de conexões de área de trabalho remota
description: Procedimentos de solução de problemas organizados por sintoma
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297395"
---
# Solucionando problemas de conexões de área de trabalho remota
Para breves explicações de vários os problemas mais comuns de serviços de área de trabalho remota (RDS), consulte [Perguntas frequentes sobre os clientes de área de trabalho remota](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Este artigo descreve várias abordagens mais avançadas para solucionar problemas de conexão. Muitos desses procedimentos se aplicam se você está solucionando problemas de uma configuração simples, como um computador físico para se conectar a outro computador físico ou uma configuração mais complicada. Alguns procedimentos resolva os problemas que ocorrem apenas em cenários de vários usuários mais complicados. Para obter mais informações sobre os componentes da área de trabalho remota e como eles funcionam juntos, consulte [arquitetura de serviços de área de trabalho remota](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muitos dos procedimentos descritos neste artigo exigem que você acessar vários computadores, que algumas das quais você pode ter para acessar remotamente. Para obter mais informações sobre as ferramentas de administração remota e como configurá-las, consulte [Server Administration ferramentas remoto (RSAT) para sistemas operacionais Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Na lista a seguir, identificam o tipo de sintoma que apresentarem você (ou os usuários).

- [O cliente da área de trabalho remoto não pode se conectar à área de trabalho remota, mas não existem sintomas específicos ou mensagens (etapas de solução de problemas gerais)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [O cliente da área de trabalho remoto não puder se conectar à área de trabalho remota e recebe uma mensagem de "Classe não registrada"](#client-cannot-connect-class-not-registered)
- [O cliente da área de trabalho remoto não puder se conectar à área de trabalho remota e recebe uma "sem licenças disponíveis" ou "Erro de segurança" mensagem](#client-cannot-connect-no-licenses-available)
- [O usuário recebe uma mensagem de "Acesso negado" ou deve fornecer as credenciais duas vezes](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sobre como conectar, o recebe uma mensagem "O serviço de área de trabalho remoto está ocupado"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [O cliente da área de trabalho remoto desconecta e não poderá reconectar à mesma sessão](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [O usuário se conecta a um laptop remoto em uma rede sem fio, e, em seguida, o notebook se desconectar da rede](#remote-laptop-disconnects-from-wireless-network).
- [O usuário experiencia desempenho ruim ou problemas com aplicativos remotos](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obter uma lista atual de códigos de desconectar RDP, consulte a [enumeração ExtendedDisconnectReasonCode](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## Não há sintomas específicos ou mensagens (etapas de solução de problemas gerais)

Use estas etapas quando um cliente de área de trabalho remota não pode se conectar a uma área de trabalho remota, mas não fornece mensagens ou outros sintomas que ajudam a identificam a causa. Para resolver muitos das causas mais comuns desse tipo de problema, use os seguintes métodos:

- [Verificar o status do protocolo RDP](#check-the-status-of-the-rdp-protocol)
- [Verificar o status dos serviços RDP](#check-the-status-of-the-rdp-services)
- [Verifique se o ouvinte RDP está funcionando](#check-that-the-rdp-listener-is-functioning)
- [Verifique a porta de escuta RDP](#check-the-rdp-listener-port)

### Verificar o status do protocolo RDP

#### Verificar o status do protocolo RDP em um computador local

Para verificar e alterar o status do protocolo RDP em um computador local, veja [como habilitar a área de trabalho remota](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se as opções da área de trabalho remotas não estiverem disponíveis, consulte [Verificar se um objeto de política de grupo está bloqueando o RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### Verificar o status do protocolo RDP em um computador remoto

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes você modificá-lo, [faça backup do registro para restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para verificar e alterar o status do protocolo RDP em um computador remoto, use uma conexão de registro de rede:

1. Selecione **Iniciar**, selecione **Executar**e, em seguida, digite **regedt32**.
2. No Editor do registro, selecione o **arquivo**e, em seguida, selecione **Conectar registro da rede**.
3. Na caixa de diálogo **Selecionar computador** , insira o nome do computador remoto, selecione **Verificar nomes**e, em seguida, selecione **Okey**.
4. Navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor do registro, mostrando a entrada fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Se o valor da chave **fDenyTSConnections** for **0**, então RDP está habilitada
   - Se o valor da chave **fDenyTSConnections** for **1**, RDP está desabilitado
5. Para habilitar o RDP, altere o valor de **fDenyTSConnections** de **1** para **0**.

#### Verifique se um objeto de política de grupo (GPO) está bloqueando RDP em um computador local

Se você não pode ativar o RDP na interface do usuário ou se o valor de **fDenyTSConnections** é revertido para **1** depois que você alterou, um GPO pode ser substituam as configurações de nível do computador.

Para verificar a configuração de política de grupo em um computador local, abra uma janela de Prompt de comando como administrador e digite o seguinte comando:
```
gpresult /H c:\gpresult.html
```
Depois que esse comando for concluída, abra gpresult.html. No **Computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Connections**, localize a política **Permitir que usuários se conectem remotamente usando serviços de área de trabalho remota** .

- Se a configuração dessa política estiver **habilitada**, a política de grupo não está bloqueando conexões RDP.
- Se a configuração para essa política está **desabilitado**, verifique o **GPO vencedor**. Este é o GPO que está bloqueando conexões RDP.
![Um segmento de exemplo de gpresult.html, em que o GPO de nível do domínio * * bloco RDP * * é desabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Um segmento de exemplo de gpresult.html, em que * * Local política de grupo * é desabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### Verifique se um GPO está bloqueando RDP em um computador remoto

Para verificar a configuração de política de grupo em um computador remoto, o comando é praticamente o mesmo para um computador local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
O arquivo que esse comando produz (**gpresult-\<computer name\>.html**) usa o mesmo formato de informações que use a versão do computador local (**gpresult.html**).

#### Modificar um GPO de bloqueio

Você pode modificar essas configurações no Editor de objeto de política de grupo (GPE) e Console de gerenciamento de política de grupo (GPM). Para obter mais informações sobre como usar a política de grupo, consulte o [Gerenciamento avançado de política de grupo](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Para modificar a política de bloqueio, use um dos seguintes métodos:

- No GPE, acesse o nível de GPO apropriado (por exemplo, local ou de domínio) e navegue até **Computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Connections**\\**Permitir os usuários se conectem remotamente usando serviços de área de trabalho remota**.  
   1. Defina a política para **habilitado** ou **Não configurado**.
   2. Nos computadores afetados, abra uma janela de Prompt de comando como administrador e execute o comando **gpupdate /force** .
- No GPM, navegue até a UO em que a política de bloqueio é aplicada para os computadores afetados e excluir a política da UO.

### Verificar o status dos serviços RDP

No computador local (cliente) e o computador remoto (destino), os seguintes serviços devem estar em execução:

- Serviços de área de trabalho remota (serviço de terminal)
- Redirecionador de porta de modo do usuário de serviços de área de trabalho remota (UmRdpService)

Você pode usar o snap-in do MMC de serviços para gerenciar os serviços, local ou remotamente. Você também pode usar o PowerShell, local ou remotamente (se o computador remoto estiver configurado para aceitar comandos do PowerShell remotos).

![Serviços da área de trabalho remota no snap-in do MMC de serviços. Não modifique as configurações padrão do serviço.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

Em qualquer computador, se não estiver executando um ou ambos os serviços, iniciá-los.

> [!NOTE]  
> Se você iniciar o serviço de serviços de área de trabalho remota, clique em **Sim** para reiniciar o serviço remoto modo do usuário de serviços de área de trabalho redirecionador porta automaticamente.

### Verifique se o ouvinte RDP está funcionando

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes você modificá-lo, [faça backup do registro para restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

#### Verificar o status do ouvinte RDP

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos comandos funcionam tanto localmente e remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **\<computer name\> Enter-PSSession-ComputerName**.
2. Insira **qwinsta**. 
    ![O comando qwinsta lista os processos que escuta as portas do computador.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Se a lista inclui **rdp-tcp** com um status de **ouvir**, o ouvinte RDP está funcionando. Prossiga para [Verificar a porta de escuta RDP](#check-the-rdp-listener-port). Caso contrário, continue na etapa 4.
4. Exporte a configuração de ouvinte RDP em um computador de trabalho.
    1. Entre um computador que tenha a mesma versão do sistema operacional que tenha o computador afetado e acessar o registro do computador (por exemplo, usando o Editor do registro).
    2. Navegue até a seguinte entrada do registro:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporte a entrada para um arquivo. reg. Por exemplo, no Editor do registro, clique com botão direito a entrada, selecione **Exportar**e, em seguida, insira um nome de arquivo para as configurações exportadas.
    4. Copie o arquivo. reg exportados para o computador afetado.
5. Para importar a configuração de ouvinte RDP, abra uma janela do PowerShell que tenha permissões administrativas no computador afetado (ou abrir a janela do PowerShell e se conectar remotamente ao computador afetado).
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
      onde \<filename\> é o nome do arquivo. reg exportados.
6. Teste a configuração por tentar novamente a conexão de área de trabalho remota. Se você ainda não pode se conectar, reinicie o computador afetado.
7. Se você ainda não se conectar, [Verifique o status do certificado autoassinado RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### Verificar o status do certificado autoassinado RDP

1. Se você ainda não pode se conectar, abra o snap-in do MMC de certificados. Quando você for solicitado a selecionar o repositório de certificados para gerenciar, selecione a **conta de computador**e, em seguida, selecione o computador afetado.
2. Na pasta **certificados** na **Área de trabalho remota**, exclua o certificado autoassinado RDP. 
    ![Certificados da área de trabalho remotos no snap-in do MMC certificados.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. No computador afetado, reinicie o serviço de serviços de área de trabalho remota.
4. Atualize o snap-in de certificados.
5. Se o certificado autoassinado RDP não foi criado novamente, [Verifique as permissões da pasta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### Verifique as permissões da pasta MachineKeys

1. No computador afetado, abra o Explorador e, em seguida, navegue até **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**.
2. Clique com botão direito **MachineKeys**, selecione **Propriedades**, selecione a **segurança**e, em seguida, selecione **Avançado**.
3. Certifique-se de que as permissões a seguir são configuradas:
      - Builtin\\Administrators: Controle total
      - Todos: Leitura, gravação

### Verifique a porta de escuta RDP

No computador local (cliente) e o computador remoto (destino), o ouvinte RDP deve escutar na porta 3389. Nenhum outro aplicativo deve usar essa porta.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes você modificá-lo, [faça backup do registro para restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para verificar ou alterar a porta RDP, use o Editor do registro:

1. Selecione Iniciar, selecione **Executar**e, em seguida, digite **regedt32**.
      - Para se conectar a um computador remoto, selecione o **arquivo**e, em seguida, selecione **Conectar registro da rede**.
      - Na caixa de diálogo **Selecionar computador** , insira o nome do computador remoto, selecione **Verificar nomes**e, em seguida, selecione **Okey**.
2. Abra o registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**. 
    ![A subchave PortNumber para o protocolo RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Se **PortNumber** tem um valor que não seja **3389**, alterá-la para **3389**. 
   > [!IMPORTANT]  
    > Você pode operar usando outra porta de serviços de área de trabalho remota. No entanto, não recomendamos que você faça isso. Essa configuração de solução de problemas está além do escopo deste artigo.
4. Depois de alterar o número da porta, reinicie o serviço de serviços de área de trabalho remota.

#### Verifique se outro aplicativo não está tentando usar a mesma porta

Para este procedimento, use uma instância do PowerShell que tenha permissões administrativas. Para um computador local, você também pode usar um prompt de comando que tenha permissões administrativas. No entanto, esse procedimento usa o PowerShell porque os mesmos comandos de trabalho localmente e remotamente.

1. Abra uma janela do PowerShell. Para se conectar a um computador remoto, digite **\<computer name\> Enter-PSSession-ComputerName**.
2. Digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![O comando netstat produz uma lista de portas e os serviços de escuta para eles.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Procure uma entrada para a porta TCP 3389 (ou a porta RDP atribuída) com um status de **ouvindo**. 
    > [!NOTE]  
   > O processo (PID) do processo ou serviço usando essa porta aparece na coluna PID.
1. Para determinar qual aplicativo está usando a porta 3389 (ou a porta RDP atribuída), digite o seguinte comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![O comando tasklist relata detalhes de um processo específico.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Procure uma entrada para o número de PID que está associada com a porta (da saída **netstat** ). Os serviços ou processos que estão associados esse PID aparecerem à direita.
1. Se um aplicativo ou serviço que não sejam de serviços de área de trabalho remota (TermServ.exe) está usando a porta, você pode resolver o conflito usando um dos seguintes métodos:
      - Configure o outro aplicativo ou serviço para usar uma porta diferente (recomendada).
      - Desinstale o aplicativo ou serviço.
      - Configurar o RDP para usar uma porta diferente e, em seguida, reinicie o serviço de serviços de área de trabalho remota (não recomendado).

#### Verifique se um firewall está bloqueando a porta RDP

Use a ferramenta **psping** para testar se você pode acessar o computador afetado por meio da porta 3389.

1. Em um computador diferente do computador afetado, baixe **psping** de <https://live.sysinternals.com/psping.exe>.
2. Abra uma janela de Prompt de comando como administrador, altere o diretório no qual você instalou **psping**e, em seguida, digite o seguinte comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Verifique a saída do comando **psping** para obter resultados, como o seguinte:  
      - **Conectando-se ao \<computer IP\>**: O computador remoto está acessível.
      - **(0% de perda)**: todas as tentativas de se conectar com êxito.
      - **O computador remoto recusou a conexão de rede**: O computador remoto não está acessível.
      - **(100% de perda)**: falharam de todas as tentativas de se conectar.
1. Execute **psping** em vários computadores para testar sua capacidade de se conectar ao computador afetado.
1. Observe se o computador afetado bloqueia conexões de todos os outros computadores, alguns outros computadores ou apenas um outro computador.
1. Próximas etapas recomendadas:
      - Envolva os administradores de rede para verificar se a rede permite o tráfego RDP para o computador afetado.
      - Investigar as configurações de qualquer firewalls entre os computadores de origem e o computador afetado (incluindo o Firewall do Windows no computador afetado) para determinar se um firewall está bloqueando a porta RDP.

## Cliente não pode se conectar, "Classe não registrada"

Quando você tenta se conectar a um computador remoto usando um cliente que está executando o Windows 10, versão 1709 ou posterior, o cliente não pode se conectar enquanto o servidor de Host de sessão de área de trabalho remota relata uma mensagem que contém o código de erro "Classe não registrada (0x80040154)".

Esse problema ocorre se o usuário que está tentando se conectar tem um perfil de usuário obrigatórios. Para resolver esse problema, instale o [24 de julho de 2018 — KB4338817 (16299.579 de compilação do sistema operacional)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) atualização do Windows 10.

## Cliente não pode se conectar, sem licenças disponíveis

Essa situação se aplica a implantações que incluem um servidor RDSH e um servidor de licenciamento de área de trabalho remota.

Identificar o tipo de comportamento de usuários estão vendo:

- A sessão foi desconectada porque não há licenças estão disponíveis ou nenhum servidor de licenças está disponível
- O acesso foi negado devido a um erro de segurança

Entrar para o Host de sessão de área de trabalho remota como um administrador de domínio e abra o diagnóstico de licença de área de trabalho remota. Procure mensagens como o seguinte:

  - O período de carência para o controle remoto servidor de Host de sessão da área de trabalho tiver expirado, mas o servidor de Host de sessão de área de trabalho remota não tiver sido configurado com todos os servidores licença. conexões com o servidor de Host de sessão de RD serão negadas, a menos que um servidor de licenças está configurado para o servidor de Host de sessão de área de trabalho remota.
  - Licença server \<computer name\> não está disponível. Isso pode ser causado por problemas de conectividade de rede, o serviço de licenciamento de área de trabalho remota está parado no servidor de licenças ou licenciamento de área de trabalho remota não está instalado no computador.

Esses problemas tendem a ser associados com as seguintes mensagens de usuário:

  - A sessão remota foi desconectada porque não há nenhuma licenças de acesso do cliente de área de trabalho remota disponíveis para esse computador.
  - A sessão remota foi desconectada porque não há nenhuma servidores de licença de área de trabalho remota disponíveis para fornecer uma licença.

Nesse caso, [configure o serviço de licenciamento de área de trabalho remota](#configure-the-rd-licensing-service).

Se o diagnóstico de licença de área de trabalho remota lista outros problemas, como "o componente de protocolo RDP 224 detectou um erro no fluxo do protocolo e foi desconectado do cliente," pode ser um problema que afeta os certificados de licença. Esses problemas tendem a ser associado a mensagens de usuário, como o seguinte:

Por causa de um erro de segurança, o cliente não pôde se conectar ao servidor de Terminal. Após certificar-se de que você está conectado à rede, tente conectar ao servidor novamente.

Nesse caso, a [atualização do X509 chaves do registro de certificado](#refresh-the-x509-certificate-registry-keys).

### Configurar o serviço de licenciamento de área de trabalho remota

O procedimento a seguir usa o Gerenciador do servidor para fazer as alterações de configuração. Para obter informações sobre como configurar e usar o Gerenciador do servidor, consulte o [Gerenciador do servidor](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Abra o Gerenciador do servidor e, em seguida, navegue até **Serviços de área de trabalho remota**.
2. Na **Visão geral da implantação**, selecione as **tarefas**e, em seguida, selecione **Editar propriedades de implantação**.
3. Selecione o **Licenciamento de área de trabalho remota**e, em seguida, selecione o modo de licenciamento apropriado para a sua implantação (**Por dispositivo** ou **Por usuário**).
4. Insira o nome de domínio totalmente qualificado (FQDN) do seu servidor de licença de área de trabalho remota e, em seguida, selecione **Add**.
5. Se você tiver mais de um servidor de licença de área de trabalho remota, repita a etapa 4 para cada servidor. 
    ![Opções de configuração servidor licença de área de trabalho remota no Gerenciador do servidor.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### Atualizar o X509 chaves do registro de certificado

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes você modificá-lo, [faça backup do registro para restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para resolver esse problema, faça backup e, em seguida, remove o X509 chaves de registro de certificado, reinicie o computador e, em seguida, servidor de licenciamento reativar a área de trabalho remota. Para fazer isso, siga estas etapas.

> [!NOTE]  
> Execute o seguinte procedimento em cada um dos servidores RDSH.

1. Abra o Editor do registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. No menu do registro, selecione **Exportar arquivo do registro**.
3. Digite o **Certificado exportado** na caixa de **nome de arquivo** e, em seguida, selecione **Salvar**.
4. Clique com botão direito cada um dos seguintes valores, selecione **Excluir**e, em seguida, selecione **Sim** para verificar a exclusão:  
      - **Certificado**
      - **Certificado X509**
      - **ID do certificado X509**
      - **X509 Certificate2**
5. Saia do Editor do registro e, em seguida, reinicie o servidor de RDSH.

## Usuário não consegue autenticar ou deve autenticar duas vezes

Há vários problemas que podem causar problemas que afetam a autenticação do usuário. Esta seção aborda os seguintes casos:

  - [Um usuário terá o acesso negado com uma mensagem "tipo de logon restrito"](#access-denied-restricted-type-of-logon)
  - [Um usuário ou aplicativo é negado acesso com o evento 16965, "uma chamada remota ao banco de dados SAM foi negada"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Um usuário não pode se conectar usando um cartão inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se o computador remoto estiver bloqueado, um usuário precisa inserir uma senha duas vezes](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Um usuário não pode entrar e receber mensagens de "Correção de oracle de criptografia CredSSP" e "Erro de autenticação"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Depois de atualizar computadores cliente, alguns usuários precisem entrar duas vezes](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Os usuários são acesso negados em uma implantação que usa RemoteGuard com vários agentes de Conexão de área de trabalho remota](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### Acesso negado, tipo restrito de logon

Nessa situação, um usuário do Windows 10 estão tentando se conectar a computadores Windows 10 ou Windows Server 2016 é negado acesso com a seguinte mensagem:

> Conexão de área de trabalho remota:  
> O administrador do sistema tiver restringido o tipo de logon (rede ou interativo) que você pode usar. Para obter ajuda, contate o administrador do sistema ou o suporte técnico.

Esse problema ocorre quando a autenticação de nível de rede (NLA) é necessária para conexões RDP, e o usuário não é um membro do grupo de **Usuários da área de trabalho remota** . Ele também pode ocorrer se o grupo de **Usuários da área de trabalho remota** não tiver sido atribuído ao usuário **acesso a este computador da rede** à direita.

Para corrigir esse problema, siga uma destas etapas:

  - [Modificar a associação de grupo do usuário ou a atribuição de direitos de usuário](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desative NLA (não recomendado).
  - Use os clientes da área de trabalho remotos diferente do Windows 10. Por exemplo, os clientes do Windows 7 não têm esse problema.

#### Modificar a associação de grupo do usuário ou a atribuição de direitos de usuário

Se esse problema afeta um único usuário, a solução mais simples para esse problema é adicionar o usuário ao grupo de **Usuários da área de trabalho remota** .

Se o usuário já for um membro desse grupo (ou se vários membros do grupo têm o mesmo problema), verifique a configuração de direitos de usuário no computador remoto do Windows 10 ou Windows Server 2016.

1. Abra o Editor de objeto de política de grupo (GPE) e conecte-se à política local do computador remoto.
2. Navegue até a **Atribuição de direitos do computador configuração configurações Settings\\Local Policies\\User**, clique com botão direito **acesso a este computador da rede**e, em seguida, selecione **Propriedades**.
3. Verificar a lista de usuários e grupos de **Usuários da área de trabalho remota** (ou um grupo pai).
4. Se a lista não inclui **Os usuários de área de trabalho remota** (ou um grupo de pai, como **todos**) você precisará adicioná-lo à lista (se você tiver mais de um ou dois computadores em sua implantação, use um objeto de política de grupo).  
    Por exemplo, a associação padrão para **acesso a este computador da rede** inclui **todos**. Se sua implantação usa um objeto de política de grupo para remover **todos**, você precisa restaurar o acesso, atualizando o objeto de política de grupo para adicionar **Usuários de área de trabalho remota**.

### Acesso negado, uma chamada remota ao banco de dados SAM foi negada

Esse comportamento é mais provável de ocorrer se os controladores de domínio estão executando o Windows Server 2016 ou posterior, e os usuários tentam se conectar usando um aplicativo de conexão personalizado. Em particular, os aplicativos que acessam informações de perfil do usuário no Active Directory terá acesso negados.

Esse comportamento resulta de uma mudança para o Windows. No Windows Server 2012 R2 e versões anteriores versões, quando um usuário faz logon em um remoto contatos do Gerenciador de Conexão remota (RCM) da área de trabalho, o controlador de domínio (DC) para consultar as configurações que são específicas para a área de trabalho remota no objeto de usuário no domínio do Active Directory Services (AD DS). Essas informações são exibidas na guia do perfil de serviços de área de trabalho remota de propriedades do objeto do usuário no Active Directory Users e snap-in do MMC de computadores.

A partir do Windows Server 2016, RCM não consulta o objeto de usuários no AD DS. Se você precisar RCM consultar o AD DS porque você está usando os atributos de serviços de área de trabalho remota, você deve habilitar manualmente o comportamento RCM anterior.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes você modificá-lo, [faça backup do registro para restauração](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) no caso de problemas.

Para habilitar o comportamento RCM herdado em um servidor de Host de sessão de área de trabalho remota, configure as seguintes entradas do registro e, em seguida, reinicie o serviço de **Serviços de área de trabalho remota** :  
  - **Serviços de NT\\Terminal HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar o comportamento RCM herdado em um servidor que não seja um servidor de Host de sessão de área de trabalho remota, configurar essas entradas do registro e a seguinte entrada do registro adicionais (e, em seguida, reinicie o serviço):
  - **Servidor HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal**

Para obter mais informações sobre esse comportamento, consulte KB 3200967 [alterações remoto o Gerenciador de Conexão no Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### Um usuário não pode se conectar usando um cartão inteligente

Este artigo aborda três circunstâncias comuns em que um usuário não pode entrar uma área de trabalho remota usando um cartão inteligente:  
  - [O usuário não pode entrar em uma filial que usa um somente leitura domínio RODC (controlador)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [O usuário não pode entrar em um computador Windows Server 2008 SP2 que foi atualizado recentemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [O usuário não pode ficar conectado a um computador Windows que foi atualizado recentemente, e não responde (trava) se torna o serviço de serviços de área de trabalho remota](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### Não é possível entrar com um cartão inteligente em uma filial com um RODC

Esse problema ocorre em implantações que incluem um servidor RDSH de uma filial que usa um RODC. O servidor de RDSH está hospedado no domínio raiz. Os usuários da filial pertencem a um domínio filho e usam cartões inteligentes para autenticação. O RODC está configurado para senhas de usuário de cache (RODC pertence ao **Grupo de replicação de senha de RODC permitido**). Quando os usuários tentarem entrar para sessões no servidor de RDSH, eles recebem mensagens, como "a tentativa de logon é inválida. Isso é devido a um nome de usuário ou autenticação informações ruins."

Isso é um problema conhecido da maneira que a raiz do controlador de domínio e o RODC gerenciam a criptografia das credenciais do usuário. A controlador de domínio raiz usa uma chave de criptografia para criptografa as credenciais e o RODC fornece uma chave de descriptografia para o cliente. No entanto, as duas chaves não coincidem.

Para contornar esse problema, considere alterar sua topologia DC desativando o cache de senhas no RODC ou implantando um controlador de domínio gravável para a filial. É um método alternativo mover o servidor RDSH ao mesmo domínio filho como os usuários. Outra alternativa é permitir que os usuários entrem sem cartões inteligentes. Qualquer uma dessas soluções pode exigir comprometimentos em desempenho ou nível de segurança.

#### Não é possível entrar com um cartão inteligente para um computador Windows Server 2008 SP2

Esse problema ocorre quando os usuários se conectam a um computador Windows Server 2008 SP2 que foi atualizado com KB4093227 (2018.4B). Quando os usuários tentarem entrar usando um cartão inteligente, eles têm o acesso negados com mensagens como "nenhum certificado válido localizado. Verifique se o cartão for inserido corretamente e se encaixa perfeitamente." Ao mesmo tempo, o computador Windows Server registra o evento de aplicativo "Ocorreu um erro ao recuperar um certificado digital do cartão inteligente inserido. Assinatura inválida."

Para resolver esse problema, atualize o computador Windows Server com o 2018.06Bre-versão do KB 4093227, [Descrição da atualização de segurança para a protocolo de área de trabalho remota (RDP) vulnerabilidade de negação de serviço no Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### Não é possível permanecer assinado com um cartão inteligente e travamentos de serviço de serviços de área de trabalho remota

Esse problema ocorre quando os usuários se conectam a um computador Windows ou Windows Server que foi atualizado com 4056446 KB. Primeiro, o usuário poderá entrar no sistema usando um cartão inteligente, mas, em seguida, recebe uma mensagem de erro "SCARD\_E\_NO\_SERVICE". O computador remoto pode parar de responder.

Para contornar esse problema, reinicie o computador remoto.

Para resolver esse problema, atualize o computador remoto com a correção apropriada:

  - Windows Server 2008 SP2: KB 4090928, [manipula no processo lsm.exe vazamentos de Windows e aplicativos de cartão inteligente podem exibir erros de "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 — KB4103724 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versão 1607: 4103720 KB, [17 de maio de 2018 — KB4103720 (14393.2273 de compilação do sistema operacional)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### Se o computador remoto estiver bloqueado, um usuário precisa inserir uma senha duas vezes

Esse problema pode ocorrer quando um usuário tenta se conectar a uma área de trabalho remota que está executando o Windows 10 versão 1709 em uma implantação em que as conexões RDP não exigem NLA. Sob essas condições, se a área de trabalho remota foi bloqueada, o usuário precisa inserir suas credenciais duas vezes ao se conectar.

Para resolver esse problema, atualize o computador do Windows 10 versão 1709 com 4343893 KB, [30 de agosto de 2018 — KB4343893 (16299.637 de compilação do sistema operacional)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### Usuário não é possível entrar e receber mensagens de "Correção de oracle de criptografia CredSSP" e "Erro de autenticação"

Quando os usuários tentarem entrar usando qualquer versão do Windows do Windows Vista SP2 e versões posteriores ou Windows Server 2008 SP2 e versões posteriores, eles têm acesso negados e recebem mensagens, como o seguinte:

  - Ocorreu um erro de autenticação. Não há suporte para a função solicitada.
  - Isso pode ser devido a correção da oracle CredSSP criptografia

"Correção de oracle de criptografia CredSSP" refere-se a um conjunto de atualizações de segurança, lançada em março e abril de maio de 2018. CredSSP é um provedor de autenticação que processa as solicitações de autenticação para outros aplicativos. O 13 de março de 2018, "3B" e as atualizações posteriores resolvido uma exploração em que um invasor poderia retransmitir credenciais do usuário para executar código no sistema de destino.

As atualizações iniciais adicionado o suporte para um novo objeto de política de grupo, **Criptografia Oracle correção**, que tem as seguintes configurações possíveis:

  - **Vulneráveis**. Aplicativos de cliente que usar CredSSP poderá retornar para versões inseguras, mas esse comportamento expõe as áreas de trabalho remotas para ataques. Serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Atenuados**. Aplicativos de cliente que usar CredSSP não podem retornar para versões inseguras, mas serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - **Força atualizado clientes**. Aplicativos de cliente que usar CredSSP não podem retornar para versões inseguras e serviços que usam o CredSSP não aceitará clientes sem patch. 
    Observação: Esta configuração não deve ser implantada até que todos os hosts remotos compatível com a versão mais recente.

A atualização de 8 de maio de 2018, alterado a configuração de **Criptografia Oracle correção** padrão de **vulnerável** a **Mitigated**. Com essa alteração no lugar, clientes da área de trabalho remotos que as atualizações não podem se conectar a servidores que não têm-los (ou atualizado servidores que não foi reiniciados). Para obter mais informações sobre os efeitos das atualizações e os tipos de comunicação que eles bloqueiam, consulte 4093492 KB, [CredSSP atualizações para CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver esse problema, certifique-se de que todos os sistemas são totalmente atualizados e reiniciados. Para obter uma lista completa de atualizações e para obter mais informações sobre as vulnerabilidades, consulte [CVE-2018-0886 | Vulnerabilidade de execução de código remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Para contornar esse problema até que as atualizações sejam concluídas, verifique 4093492 KB para permitidos tipos de conexões. Se não houver nenhum viáveis alternativas que você pode considerar um dos seguintes métodos:

  - Para os computadores cliente afetado, defina a política de **Criptografia Oracle correção** para **vulnerável**.
  - Modifique as políticas a seguir na pasta de política de grupo do **Computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Security** :  
      - **Exigir o uso de camada de segurança específica para conexões remotas (RDP)**: definido como **habilitado** e selecione **RDP**.
      - **Exigir autenticação do usuário para conexões remotas usando autenticação no nível da rede**: definido como **desabilitado**.
      > [!IMPORTANT]  
      > Essas modificações reduzem a segurança da sua implantação. Eles só devem ser temporários, se você usá-los em todos os.

Para obter mais informações sobre como trabalhar com a política de grupo, consulte [modificar um GPO de bloqueio](#modifying-a-blocking-gpo).

### Depois de atualizar computadores cliente, alguns usuários precisem entrar duas vezes

Os usuários em alguma versão do Windows 7 ou Windows 10 1709 entrar em uma sessão de área de trabalho remota e veja imediatamente uma segunda tela de entrada. Esse problema ocorre se os computadores cliente tem recebido as atualizações a seguir:

  - Windows 7: KB 4103718, [8 de maio de 2018 — KB4103718 (mensal Rollup)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de maio de 2018 — KB4103727 (compilação do sistema operacional 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver esse problema, certifique-se de que os computadores que os usuários desejam se conectar (bem como RDSH ou RDVI servidores) são totalmente atualizados por meio de junho de 2018. Isso inclui as atualizações a seguir:

  - Windows Server 2016: KB 4284880, [12 de junho de 2018 — KB4284880 (compilação do sistema operacional 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junho de 2018 — KB4284815 (mensal Rollup)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junho de 2018 — KB4284855 (mensal Rollup)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junho de 2018 — KB4284826 (mensal Rollup)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descrição da atualização de segurança para a vulnerabilidade de execução remota de código CredSSP no Windows Server 2008, Windows Embedded POSReady 2009 e Windows Embedded padrão 2009: 13 de março de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### Os usuários são acesso negados em uma implantação que usa o Remote Credential Guard com vários agentes de Conexão de área de trabalho remota

Esse problema ocorre em implantações de alta disponibilidade que usam dois ou mais remoto da área de trabalho Conexão agentes, se o Windows Defender Remote Credential Guard está em uso. Os usuários não podem entrar áreas de trabalho remotas.

Esse problema ocorre porque o Remote Credential Guard usa Kerberos para autenticação e restringe o NTLM. No entanto, em uma configuração de alta disponibilidade com balanceamento de carga, os agentes de Conexão de área de trabalho remota não podem suportar operações da Kerberos.

Se você precisar usar uma configuração de alta disponibilidade com agentes de Conexão de área de trabalho remota com balanceamento de carga, você pode contornar esse problema, desativando o Remote Credential Guard. Para obter mais informações sobre como gerenciar o Windows Defender Remote Credential Guard, consulte as [credenciais de área de trabalho remota proteger com o Windows Defender Remote Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## Sobre como conectar, o usuário recebe a mensagem "O serviço de área de trabalho remoto está ocupado"

Para determinar uma resposta apropriada para esse problema, use as seguintes etapas:

1. O serviço de serviços de área de trabalho remota não responde (por exemplo, o cliente da área de trabalho remoto parece "travar" na tela de boas-vindas).  
      - Se o serviço ficar sem resposta, consulte o [problema de memória do servidor RDSH](#rdsh-server-memory-issue).
      - Se o cliente parece ser interagindo com o serviço normalmente, continue para a próxima etapa.
2. Se um ou mais usuários desconectarem sessões remotas da área de trabalho, poderá os usuários se conectar novamente?  
   - Se o serviço continua a negar conexões não importa quantos usuários desconectarem suas sessões, consulte o [problema do ouvinte de área de trabalho remota](#rd-listener-issue).
   - Se o serviço começa a aceitar conexões novamente após um número de usuários tem desconectado suas sessões, [Verifique a política de limite de conexão](#check-the-connection-limit-policy).

### Problema de memória do servidor RDSH

Um vazamento de memória foi encontrado em alguns servidores do Windows Server 2012 R2 RDSH. Ao longo do tempo, esses servidores começam recuse conexões de área de trabalho remotas e logons console local com mensagens, como o seguinte:

> A tarefa que você está tentando não pode ser concluída porque o serviço de área de trabalho remota está ocupado. Tente novamente em alguns minutos. Outros usuários ainda devem ser capazes de entrar.

Clientes da área de trabalho remotos estão tentando se conectar também param de responder.

Para contornar esse problema, reinicie o servidor RDSH.

Para resolver esse problema, aplicar 4093114 KB, [10 de abril de 2018 — KB4093114 (mensal Rollup)] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly% 20Rollup\)), para os servidores RDSH.

### Problema de ouvinte de área de trabalho remota

Um problema foi observado em alguns servidores RDSH que foram atualizados diretamente do Windows Server 2008 R2 para o Windows Server 2012 R2 ou Windows Server 2016. Quando um cliente da área de trabalho remoto se conecta ao servidor de RDSH, o servidor de RDSH cria um ouvinte de área de trabalho remota para a sessão do usuário. Os servidores afetados mantêm uma contagem dos ouvintes de área de trabalho remota que aumenta à medida que os usuários se conectar, mas nunca diminui.

Para contornar esse problema, você pode usar os métodos a seguir:

  - Reinicie o servidor RDSH para redefinir a contagem de ouvintes de área de trabalho remota
  - Modificar a política de limite de conexão, defini-lo como um valor muito grande. Para obter mais informações sobre como gerenciar a política de limite de conexão, consulte [Verificar a política de limite de conexão](#check-the-connection-limit-policy).

Para resolver esse problema, aplica as atualizações a seguir para os servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 — KB4343891 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018 — KB4343884 (compilação do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### Verifique a política de limite de conexão

Você pode definir o limite no número de conexões de área de trabalho remotas simultâneas no nível do computador individual ou configurando um objeto de política de grupo (GPO). Por padrão, o limite não está definido.

Para verificar as configurações atuais e identificar qualquer GPOs existentes no servidor de RDSH, abra uma janela de prompt de comando como administrador e digite o seguinte comando:
  
```
gpresult /H c:\gpresult.html
```
   
Após a conclusão desse comando, abra gpresult.html e no **Computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Connections**, localize o **número limite de conexões** política.

  - Se a configuração dessa política estiver **desabilitado**, a política de grupo não está limitando conexões RDP.
  - Se a configuração para essa política é **habilitado**, verifique **GPO vencedor**. Se você precisar remover ou alterar o limite de conexão, edite esse GPO.

Para aplicar alterações de políticas, abra uma janela de prompt de comando no computador afetado e digite o seguinte comando:
  
```
gpupdate /force
```
  
## Cliente de área de trabalho remota desconecta e não poderá reconectar à mesma sessão

Depois que o cliente da área de trabalho remota perde sua conexão de área de trabalho remota, o cliente não pode reconectar imediatamente. O usuário recebe mensagens de erro, como o seguinte:

  - Por causa de um erro de segurança, o cliente não pôde se conectar ao servidor de Terminal. Após certificar-se de que você está conectado à rede, tente conectar ao servidor novamente.
  - Área de trabalho remota desconectada. Por causa de um erro de segurança, o cliente não pôde se conectar ao computador remoto. Verifique se você estiver conectado à rede e tente se conectar novamente.

Em um momento posterior, o cliente da área de trabalho remoto reconectado a área de trabalho remota. No entanto, em vez de conexão do cliente para a sessão original, o servidor de RDSH conecta o cliente para uma nova sessão. O servidor de RDSH a verificação revela que a sessão original não inseriu um estado desconectado, mas em vez disso, permanece ativo.

Para contornar esse problema, você pode habilitar a política de **intervalo de conexão keep alive de configurar** do computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Connections ** **pasta de política de grupo. Se você habilitar esta configuração de política, você deve inserir um intervalo de keep alive. O intervalo keep alive determina a frequência, em minutos, o servidor verifica o estado da sessão.

Esse problema pode ser causado pela configuração incorreta de suas configurações de autenticação e configuração. Você pode definir essas configurações no nível do servidor ou usando GPOs. As configurações de política de grupo estão disponíveis na pasta de política de grupo do **Computador\\modelos administrativos\\componentes Components\\Remote de área de trabalho Services\\Remote da sessão da área de trabalho Host\\Security** .

1. No servidor de Host de sessão de área de trabalho remota, abra a configuração do Host da sessão da área de trabalho remota.
2. Em **conexões**, clique com botão direito no nome da conexão e, em seguida, clique em **Propriedades**.
3. Na caixa de diálogo **Propriedades** para a conexão, na guia **Geral** , na camada de **segurança** , selecione um método de segurança.
4. No **nível de criptografia**, clique no nível que você deseja. Você pode selecionar **baixa**, **Compatível com cliente**, **alto**ou **Compatível com FIPS**.

> [!NOTE]  
>  - Quando a comunicação entre clientes e servidores de Host de sessão de RD exige o nível mais alto de criptografia, use criptografia compatível com FIPS.
>  - As configurações de nível de criptografia que você definir na política de grupo substituem a configuração que você definir usando a ferramenta de configuração de serviços de área de trabalho remota. Além disso, se você habilitar o [criptografia do sistema: usar compatível com algoritmos FIPS para criptografia, hash e assinatura](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) diretiva, essa configuração substitui a política de **definir o nível de criptografia de conexão do cliente** . A política de criptografia do sistema está na pasta de **Configuração do computador configurações Settings\\Local Policies\\Security opções** .
>  - Quando você alterar o nível de criptografia, o novo nível de criptografia entra em vigor na próxima vez em que um usuário faz logon. Se você precisar de vários níveis de criptografia em um servidor, instale vários adaptadores de rede e configure cada adaptador separadamente.
>  - Para verificar que se o certificado tem uma chave privada correspondente, na configuração de serviços de área de trabalho remota, clique com botão direito a conexão para o qual você deseja exibir o certificado, selecione **Geral**, selecione **Editar**, selecione o certificado que você deseja Exibir e, em seguida, selecione **Exibir certificado**. Na parte inferior da guia **Geral** , a instrução "você tem uma chave privada que corresponde a esse certificado" deve aparecer. Você também pode exibir essas informações usando o snap-in de certificados.
>  - Criptografia compatível com FIPS (o **criptografia do sistema: usar compatível com algoritmos FIPS para criptografia, hash e assinatura** política ou a configuração **Compatível com FIPS** na configuração do servidor de área de trabalho remota) criptografa e descriptografa dados enviados da cliente para o servidor e do servidor para o cliente, com os algoritmos de criptografia de 140-1 Federal informações Processing Standard (FIPS), usando módulos criptográficos da Microsoft. Para obter mais informações, consulte [FIPS 140 validação](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - A configuração de **alta** criptografa os dados enviados do cliente para o servidor e do servidor para o cliente usando criptografia forte de 128 bits.
>  - A configuração **Compatível com cliente** criptografa os dados enviados entre o cliente e servidor em nível máximo suportado pelo cliente.
>  - A configuração **baixo** criptografa dados enviados do cliente ao servidor usando criptografia de 56 bits.

## Notebook remoto se desconectar da rede sem fio

Esse problema pode ocorrer quando um cliente da área de trabalho remoto se conecta a um laptop usando uma rede sem fio 802.1 x. Modo intermitente, o notebook se desconectar da rede sem fio e não reconecte automaticamente.

Isso é um problema conhecido que ocorre quando a configuração de autenticação de rede para a conexão de rede sem fio é a **autenticação do usuário**.

Para contornar esse problema, defina a configuração de autenticação de rede para **autenticação do usuário ou computador** ou **computador**.

 > [!NOTE]  
> Para alterar as configurações de autenticação de rede em um único computador, você precisará usar o painel de controle central de rede e compartilhamento para criar uma nova conexão sem fio com as novas configurações.

Para obter uma descrição de como definir as configurações de rede sem fio usando GPOs completa, consulte [diretivas de configurar a rede sem fio (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## Problemas de aplicativo ou um desempenho ruim as experiências do usuário

Esta seção aborda vários problemas comuns que os usuários podem enfrentar quando usarem a funcionalidade de área de trabalho remota:

  - [Os usuários que se conectam a novas máquinas virtuais Microsoft Azure modo intermitente enfrentar problemas](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Os usuários que se conectam a Windows 10 versão 1709 áreas de trabalho remotas podem ter problemas de reprodução de vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Os usuários com perfis de usuário somente leitura que se conectam a áreas de trabalho remotas do Windows 10 não podem compartilhar suas áreas de trabalho.](#desktop-sharing-issues-on-windows-10)
  - [Os usuários se conectarem a remotas do Windows 10 usando a clientes que executam versões mais antigas do Windows 10 enfrentar um desempenho ruim quando NLA está desabilitado](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando os usuários executem vários aplicativos em sessões de área de trabalho remotas e desconectar e reconectar com frequência, eles poderão encontrar uma tela preta sobre como reconectar.](#black-screen-issue)

### Intermitentes problemas com novas máquinas virtuais do Microsoft Azure

Esse problema afeta as máquinas virtuais que foram provisionadas recentemente. Depois que o usuário se conecta à máquina virtual, a sessão de área de trabalho remota não pode carregar todas as configurações do usuário corretamente.

Para contornar esse problema, desconecte a máquina virtual, aguarde pelo menos 20 minutos e, em seguida, conecte-se novamente.

Para resolver esse problema, aplica as atualizações a seguir para as máquinas virtuais, conforme apropriado:

  - Windows 10 e Windows Server 2016: 4343884, KB [30 de agosto de 2018 — KB4343884 (compilação do sistema operacional 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 — KB4343891 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### Problemas de reprodução de vídeo no Windows 10 versão 1709

Esse problema ocorre quando os usuários se conectam a computadores remotos que executam o Windows 10, versão 1709. Quando esses usuários reproduzir vídeos usando o VMR9 codec (vídeo MixingRenderer9), o player mostra apenas uma janela preta.

Isso é um problema conhecido no Windows 10, versão 1709. O problema não ocorre no Windows 10 versão 1703.

### Área de trabalho do compartilhamento problemas no Windows 10

Esse problema ocorre quando o usuário tem um perfil de usuário somente leitura (e o hive do registro associado), como em um cenário de quiosque. Quando esse usuário se conecta a um computador remoto que está executando o Windows 10, versão 1803, eles não podem compartilhar sua área de trabalho.

Para corrigir esse problema, aplicar a atualização do Windows 10 4340917, [24 de julho de 2018 — KB4340917 (17134.191 de compilação do sistema operacional)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problemas de desempenho quando mistura de versões do Windows 10 se NLA está desabilitado

Esse problema ocorre quando NLA está desabilitada, e os computadores cliente da área de trabalho remota que executam o Windows 10 se conectar a áreas de trabalho remotas que executam diferentes versões do Windows 10. Os usuários de clientes da área de trabalho remotos em computadores que executam o Windows 10, versão 1709 ou versões anteriores enfrentar um desempenho ruim quando se conectarem a áreas de trabalho remotas que executam o Windows 10, versão 1803 ou uma versão posterior.

Isso ocorre porque, quando NLA está desabilitada, os computadores cliente antigos usam um protocolo mais lento quando se conectarem a Windows 10, versão 1803 ou uma versão posterior.

Para resolver esse problema, aplicar 4340917 KB, [24 de julho de 2018 — KB4340917 (17134.191 de compilação do sistema operacional)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problema de tela preta

Esse problema ocorre no Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Um usuário inicia vários aplicativos em uma área de trabalho remota e, em seguida, se desconectar da sessão. Periodicamente, o usuário será reconectado à área de trabalho remota para interagir com os aplicativos e, em seguida, desconecte novamente. Em algum momento, quando o usuário for reconectado, a sessão de área de trabalho remota mostra apenas uma tela preta. O usuário deve encerrar a sessão de área de trabalho remota do console do computador remoto (ou o console do servidor RDSH). Essa ação impede que os aplicativos.

Para resolver esse problema, aplique as atualizações a seguir conforme apropriado:

  - Windows 8 e Windows Server 2012: KB4103719, [17 de maio de 2018 — KB4103719 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB4103724, [17 de maio de 2018 — KB4103724 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) e 4284863, KB [21 de junho de 2018 — KB4284863 (versão prévia do pacote cumulativo de atualizações mensais)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Corrigido em KB4284860, [12 de junho de 2018 — KB4284860 (compilação do sistema operacional 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
