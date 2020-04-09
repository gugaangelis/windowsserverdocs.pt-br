---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Deploy Access-Denied Assistance (Demonstration Steps)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fc23e9d9dae9118bf6d489ed8697ce5bac44e7ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861259"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Deploy Access-Denied Assistance (Demonstration Steps)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como configurar a assistência para acesso negado e verificar se ele está funcionando corretamente.  
  
**Neste documento**  
  
-   [Etapa 1: configurar a assistência de acesso negado](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Etapa 2: definir as configurações de notificação de email](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Etapa 3: verificar se a assistência de acesso negado está configurada corretamente](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Este tópico inclui cmdlets de exemplo do Windows PowerShell que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="step-1-configure-access-denied-assistance"></a><a name="BKMK_1"></a>Etapa 1: configurar a assistência de acesso negado  
Você pode configurar a assistência para acesso negado dentro de um domínio usando a Política de Grupo, ou pode configurar a assistência individualmente em cada servidor de arquivos usando o console do Gerenciador de Recursos de Servidor de Arquivos. Você também pode alterar a mensagem de acesso negado para uma pasta compartilhada específica em um servidor de arquivos.  
  
Você pode configurar a assistência para acesso negado para o domínio usando a Política de Grupo, como segue:  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Para configurar a assistência para acesso negado usando a Política de Grupo  
  
1.  Abra Gerenciamento de Política de Grupo. No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Gerenciamento de Política de Grupo**.  
  
2.  Clique com o botão direito do mouse na Política de Grupo apropriada e clique em **Editar**.  
  
3.  Clique em **Configuração do Computador**, clique em **Políticas**, clique em **Modelos Administrativos**, clique em **Sistema** e depois em **Assistência para Acesso Negado**.  
  
4.  Clique com o botão direito do mouse em **Personalizar mensagem para erros de Acesso Negado**e clique em **Editar**.  
  
5.  Selecione a opção **Habilitado**.  
  
6.  Configure as seguintes opções:  
  
    1.  Na caixa **Exibir a seguinte mensagem aos usuários com acesso negado** , digite uma mensagem que os usuários verão quando lhes for negado acesso a um arquivo ou pasta.  
  
        Você pode adicionar à mensagem macros que inserirão texto personalizado. As macros são:  
  
        -   **[Caminho do arquivo original]** O caminho do arquivo original acessado pelo usuário.  
  
        -   **[Pasta do caminho do arquivo original]** A pasta pai do caminho do arquivo original acessada pelo usuário.  
  
        -   **[Email adminl]** A lista de destinatários de email do administrador.  
  
        -   **[Email do proprietário dos dados]** A lista de destinatários de email do proprietário dos dados.  
  
    2.  Marque a caixa de seleção **Permitir que os usuários solicitem assistência** .  
  
    3.  Deixe as configurações padrão restantes.  
  
![guias de solução](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
Você também pode configurar a assistência para acesso negado individualmente em cada servidor de arquivos usando o console do Gerenciador de Recursos de Servidor de Arquivos.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Para configurar a assistência para acesso negado usando o Gerenciador de Recursos de Servidor de Arquivos  
  
1.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
2.  Clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos (Local)** e em **Configurar Opções**.  
  
3.  Clique na guia **Assistência para Acesso Negado** .  
  
4.  Marque a caixa de seleção **Permitir assistência para acesso negado** .  
  
5.  Na caixa **Exibir a seguinte mensagem aos usuários com acesso negado**, digite uma mensagem que os usuários verão quando lhes for negado acesso a um arquivo ou pasta.  
  
    Você pode adicionar à mensagem macros que inserirão texto personalizado. As macros são:  
  
    -   **[Caminho do arquivo original]** O caminho do arquivo original acessado pelo usuário.  
  
    -   **[Pasta do caminho do arquivo original]** A pasta pai do caminho do arquivo original acessada pelo usuário.  
  
    -   **[Email adminl]** A lista de destinatários de email do administrador.  
  
    -   **[Email do proprietário dos dados]** A lista de destinatários de email do proprietário dos dados.  
  
6.  Clique em **Configurar solicitações de email**, marque a caixa de seleção **Permitir que os usuários solicitem assistência** e depois clique em **OK**.  
  
7.  Clique em **Visualizar** se quiser ver como a mensagem de erro será vista pelo usuário.  
  
8.  Clique em **OK**.  
  
![guias de solução](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Depois de configurar a assistência para acesso negado, você deve habilitá-la para todos os tipos de arquivos usando a Política de Grupo.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Para configurar a assistência para acesso negado para todos os tipos de arquivos usando a Política de Grupo  
  
1.  Abra Gerenciamento de Política de Grupo. No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Gerenciamento de Política de Grupo**.  
  
2.  Clique com o botão direito do mouse na Política de Grupo apropriada e clique em **Editar**.  
  
3.  Clique em **Configuração do Computador**, clique em **Políticas**, clique em **Modelos Administrativos**, clique em **Sistema** e depois em **Assistência para Acesso Negado**.  
  
4.  Clique com o botão direito do mouse em **Permitir assistência para acesso negado ao cliente para todos os tipos de arquivo**e depois clique em **Editar**.  
  
5.  Clique em **Habilitado** e em **OK**.  
  
![guias de solução](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
Você também pode especificar uma mensagem de acesso negado separada para cada pasta compartilhada em um servidor de arquivos usando o console do Gerenciador de Recursos de Servidor de Arquivos.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Para especificar uma mensagem de acesso negado separada para uma pasta compartilhada usando o Gerenciador de Recursos de Servidor de Arquivos  
  
1.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
2.  Expanda o **Gerenciador de Recursos de Servidor de Arquivos (Local)** e depois clique em **Gerenciamento de Classificação**.  
  
3.  Clique com o botão direito do mouse em **Propriedades de Classificação** e depois em **Definir Propriedades de Gerenciamento de Pastas**.  
  
4.  Na caixa **Propriedades**, clique em **Mensagem de Assistência para Acesso Negado** e depois clique em **Adicionar**.  
  
5.  Clique em **Procurar**, em seguida, escolha a pasta que deve ter a mensagem de acesso negado personalizada.  
  
6.  Na caixa **Valor**, digite a mensagem que deve ser apresentada aos usuários quando eles não puderem acessar um recurso dentro dessa pasta.  
  
    Você pode adicionar à mensagem macros que inserirão texto personalizado. As macros são:  
  
    -   **[Caminho do arquivo original]** O caminho do arquivo original acessado pelo usuário.  
  
    -   **[Pasta do caminho do arquivo original]** A pasta pai do caminho do arquivo original acessada pelo usuário.  
  
    -   **[Email adminl]** A lista de destinatários de email do administrador.  
  
    -   **[Email do proprietário dos dados]** A lista de destinatários de email do proprietário dos dados.  
  
7.  Clique em **OK** e em **Fechar**.  
  
![guias de solução](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="step-2-configure-the-email-notification-settings"></a><a name="BKMK_2"></a>Etapa 2: definir as configurações de notificação de email  
Você deve definir as configurações de notificação por email em cada servidor de arquivos que enviará as mensagens de assistência para acesso negado.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
2.  Clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos (Local)** e em **Configurar Opções**.  
  
3.  Clique na guia **Notificações por Email**.  
  
4.  Defina as seguintes configurações:  
  
    -   Na caixa **Nome do servidor SMTP ou endereço IP** , digite o nome do endereço IP do servidor SMTP em sua organização.  
  
    -   Nas caixas **destinatários do administrador padrão** e **endereço de email padrão ' de '** , digite o endereço de email do administrador do servidor de arquivos.  
  
5.  Clique em **Enviar Email de Teste** para assegurar que as notificações de email estejam configuradas corretamente.  
  
6.  Clique em **OK**.  
  
![guias de solução](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="step-3-verify-that-access-denied-assistance-is-configured-correctly"></a><a name="BKMK_3"></a>Etapa 3: verificar se a assistência de acesso negado está configurada corretamente  
Você pode verificar se a assistência com acesso negado está configurada corretamente, tendo um usuário que está executando o Windows 8, tentar acessar um compartilhamento ou um arquivo nesse compartilhamento ao qual eles não têm acesso. Quando a mensagem de acesso negado aparecer, o usuário deverá ver um botão **Solicitar Assistência** . Depois de clicar no botão Solicitar Assistência, o usuário pode especificar um motivo para o acesso e, em seguida, enviar um email para o proprietário da pasta ou o administrador do servidor de arquivos. O proprietário da pasta ou o administrador do servidor de arquivos pode verificar se o email chegou e se contém os elementos adequados.  
  
> [!IMPORTANT]  
> Se você quiser verificar a assistência com acesso negado tendo um usuário que esteja executando o Windows Server 2012, você deve instalar a experiência desktop antes de se conectar ao compartilhamento de arquivos.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: assistência com acesso negado](Scenario--Access-Denied-Assistance.md)  
  
-   [Planejar a assistência de acesso negado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

