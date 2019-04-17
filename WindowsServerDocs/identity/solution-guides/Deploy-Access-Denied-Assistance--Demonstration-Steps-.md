---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: "Implantar assistência de acesso negado (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Implantar assistência de acesso negado (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como configurar o acesso negado assistência e verificar se ele está funcionando corretamente.  
  
**Neste documento**  
  
-   [Etapa 1: Configurar a assistência de acesso negado](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [Etapa 2: Definir as configurações de notificação de email](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [Etapa 3: Verificar se o acesso negado assistência está configurada corretamente](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [usando os Cmdlets do](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1"></a>Etapa 1: Configurar a assistência de acesso negado  
Você pode configurar o acesso negado assistência dentro de um domínio usando a política de grupo, ou você pode configurar a assistência individualmente em cada servidor de arquivos usando o console do Gerenciador de recursos do servidor de arquivos. Você também pode alterar a mensagem de acesso negado para uma pasta compartilhada específica em um servidor de arquivos.  
  
Você pode configurar o acesso negado assistência para o domínio usando a política de grupo da seguinte maneira:  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>Para configurar o acesso negado assistência usando política de grupo  
  
1.  Gerenciamento de política de grupo aberto. No Gerenciador do servidor, clique em **ferramentas**e clique em **Group Policy Management**.  
  
2.  Clique com botão direito a política de grupo apropriado e, em seguida, clique em **editar**.  
  
3.  Clique em **configuração do computador**, clique em **políticas**, clique em **modelos administrativos**, clique em **sistema**e clique em **Access-Denied assistência**.  
  
4.  Clique com botão direito **Personalizar mensagem para erros de acesso negado**e clique em **editar**.  
  
5.  Selecione o **Enabled** opção.  
  
6.  Defina as seguintes opções:  
  
    1.  No **exibir a seguinte mensagem para usuários que têm acesso negados**, digite uma mensagem que os usuários verão quando eles têm acesso negados a um arquivo ou pasta.  
  
        Você pode adicionar macros para a mensagem que irá inserir texto personalizado. As macros incluem:  
  
        -   **[Caminho do arquivo original] **o caminho do arquivo original que foi acessado pelo usuário.  
  
        -   **[Pasta de caminho de arquivo original] **a pasta pai do caminho do arquivo original que foi acessado pelo usuário.  
  
        -   **[Admin Email] **a lista de destinatários de email de administrador.  
  
        -   **[Dados proprietário Email] **a lista de destinatários de email do proprietário dados.  
  
    2.  Selecione o **permitem que os usuários solicitar assistência** caixa de seleção.  
  
    3.  Deixe o restante das configurações padrão.  
  
![guias de soluções](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
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
  
Como alternativa, você pode configurar o acesso negado assistência individualmente em cada servidor de arquivos usando o console do Gerenciador de recursos do servidor de arquivos.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>Para configurar o acesso negado assistência usando o Gerenciador de recursos do servidor de arquivos  
  
1.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
2.  Clique com botão direito **Gerenciador de recursos de servidor de arquivos (Local)**e clique em **configurar opções**.  
  
3.  Clique no **Access-Denied assistência** guia.  
  
4.  Selecione o **habilitar acesso negado assistência** caixa de seleção.  
  
5.  No **exibir a seguinte mensagem para usuários que têm acesso negados a um arquivo ou pasta**, digite uma mensagem que os usuários verão quando eles têm acesso negados a um arquivo ou pasta.  
  
    Você pode adicionar macros para a mensagem que irá inserir texto personalizado. As macros incluem:  
  
    -   **[Caminho do arquivo original] **o caminho do arquivo original que foi acessado pelo usuário.  
  
    -   **[Pasta de caminho de arquivo original] **a pasta pai do caminho do arquivo original que foi acessado pelo usuário.  
  
    -   **[Admin Email] **a lista de destinatários de email de administrador.  
  
    -   **[Dados proprietário Email] **a lista de destinatários de email do proprietário dados.  
  
6.  Clique em **solicitações de email configurar**, selecione o **permitem que os usuários solicitar assistência** caixa de seleção e, em seguida, clique em **Okey**.  
  
7.  Clique em **Preview** se você quiser ver como a mensagem de erro será exibida para o usuário.  
  
8.  Clique em **Okey**.  
  
![guias de soluções](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
Depois de configurar a assistência de acesso negado, você deve habilitá-lo para todos os tipos de arquivo usando a política de grupo.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>Para configurar a assistência de acesso negado para todos os tipos de arquivo usando a política de grupo  
  
1.  Gerenciamento de política de grupo aberto. No Gerenciador do servidor, clique em **ferramentas**e clique em **Group Policy Management**.  
  
2.  Clique com botão direito a política de grupo apropriado e, em seguida, clique em **editar**.  
  
3.  Clique em **configuração do computador**, clique em **políticas**, clique em **modelos administrativos**, clique em **sistema**e clique em **Access-Denied assistência**.  
  
4.  Clique com botão direito **habilitar acesso negado assistência no cliente para todos os tipos de arquivo**e clique em **editar**.  
  
5.  Clique em **Enabled**e clique em **Okey**.  
  
![guias de soluções](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação. 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
Você também pode especificar uma mensagem de acesso negado separada para cada pasta compartilhada em um servidor de arquivos usando o console do Gerenciador de recursos do servidor de arquivos.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>Para especificar uma mensagem de acesso negado separada para uma pasta compartilhada usando o Gerenciador de recursos do servidor de arquivos  
  
1.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
2.  Expanda **Gerenciador de recursos de servidor de arquivos (Local)**e clique em **gerenciamento de classificação**.  
  
3.  Clique com botão direito **propriedades de classificação**e clique em **definir propriedades de gerenciamento de pasta**.  
  
4.  No **propriedade**, clique em **Access-Denied assistência mensagem**e clique em **adicionar**.  
  
5.  Clique em **procurar**e, em seguida, escolha a pasta que deve ter a mensagem personalizada de acesso negado.  
  
6.  No **valor**, digite a mensagem que deve ser apresentada aos usuários quando eles não podem acessar um recurso dentro dessa pasta.  
  
    Você pode adicionar macros para a mensagem que irá inserir texto personalizado. As macros incluem:  
  
    -   **[Caminho do arquivo original] **o caminho do arquivo original que foi acessado pelo usuário.  
  
    -   **[Pasta de caminho de arquivo original] **a pasta pai do caminho do arquivo original que foi acessado pelo usuário.  
  
    -   **[Admin Email] **a lista de destinatários de email de administrador.  
  
    -   **[Dados proprietário Email] **a lista de destinatários de email do proprietário dados.  
  
7.  Clique em **Okey**e clique em **fechar**.  
  
![guias de soluções](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação. 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>Etapa 2: Definir as configurações de notificação de email  
Você deve configurar as configurações de notificação de email em cada servidor de arquivos que envia as mensagens de assistência de acesso negado.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
2.  Clique com botão direito **Gerenciador de recursos de servidor de arquivos (Local)**e clique em **configurar opções**.  
  
3.  Clique no **notificações por Email** guia.  
  
4.  Defina as seguintes configurações:  
  
    -   No **endereço IP ou nome do servidor SMTP**, digite o nome do endereço IP do servidor SMTP em sua organização.  
  
    -   No **administradores destinatários padrão** e **padrão 'De' endereço de email** caixas, digite o endereço de email do administrador do servidor de arquivos.  
  
5.  Clique em **enviar email de teste** para garantir que as notificações de email estão configuradas corretamente.  
  
6.  Clique em **Okey**.  
  
![guias de soluções](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>Etapa 3: Verificar se o acesso negado assistência está configurada corretamente  
Você pode verificar se a Ajuda de acesso negado está configurada corretamente fazendo com que um usuário que está executando o Windows 8 tentam acessar um compartilhamento ou um arquivo em que compartilham a que eles não têm acesso a. Quando aparecer a mensagem de acesso negado, o usuário deve ver uma **solicitar assistência** botão. Depois de clicar no botão solicitar assistência, o usuário pode especificar um motivo do acesso e, em seguida, envie um email para o proprietário da pasta ou o administrador de servidor de arquivos. O proprietário da pasta ou o administrador de servidor de arquivos pode verificar para você que o email chegou e contém os detalhes apropriados.  
  
> [!IMPORTANT]  
> Se você quiser verificar o acesso negado assistência fazendo com que um usuário que está executando o Windows Server 2012, você deve instalar a experiência de área de trabalho antes de se conectar ao compartilhamento de arquivos.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: Assistência de acesso negado](Scenario--Access-Denied-Assistance.md)  
  
-   [Planejar para obter assistência de acesso negado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

