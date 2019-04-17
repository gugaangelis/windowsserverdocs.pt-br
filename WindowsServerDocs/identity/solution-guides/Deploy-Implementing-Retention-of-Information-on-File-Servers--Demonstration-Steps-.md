---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: "Implantar a implementação de retenção de informações em servidores de arquivos (etapas de demonstração)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Implantar a implementação de retenção de informações em servidores de arquivos (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode definir os períodos de retenção para pastas e colocar arquivos no controle legal usando a infraestrutura de classificação de arquivo e o Gerenciador de recursos do servidor de arquivos.  
  
**Neste documento**  
  
-   [Pré-requisitos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Etapa 1: Criar recursos definições de propriedade](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Etapa 2: Configurar notificações](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Etapa 3: Criar uma tarefa de gerenciamento de arquivos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Etapa 4: Classificar manualmente um arquivo](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [usando os Cmdlets do](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Pré-requisitos  
As etapas neste tópico presumem que você tem um servidor de SMTP configurado para notificações de expiração de arquivo.  
  
## <a name="BKMK_Step1"></a>Etapa 1: Criar recursos definições de propriedade  
Nesta etapa, nós permitimos que as propriedades do recurso período de retenção e capacidade de detecção para que a infraestrutura de classificação de arquivo pode usar essas propriedades do recurso para marcar os arquivos que são verificados em uma pasta compartilhada da rede.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>A criação de definições de propriedade de recurso  
  
1.  No controlador de domínio, faça logon no servidor como um membro do grupo de segurança Administradores de domínio.  
  
2.  Abra o Centro Administrativo do Active Directory. No Gerenciador do servidor, clique em **ferramentas**e clique em **Centro Administrativo do Active Directory**.  
  
3.  Expanda **controle de acesso dinâmico**e clique em **propriedades do recurso**.  
  
4.  Clique com botão direito **período de retenção**e clique em **habilitar**.  
  
5.  Clique com botão direito **detectabilidade**e clique em **habilitar**.  
  
![guias de soluções](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Etapa 2: Configurar notificações  
Nesta etapa, nós usamos o console do Gerenciador de recursos do servidor de arquivos para configurar o servidor de SMTP, o endereço de email de administrador padrão e o endereço de email padrão que os relatórios são enviados de.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Para configurar notificações  
  
1.  Fazer logon servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **FsrmClassificationPropertyDefinition atualização**, e pressione ENTER. Isso sincronizará as definições de propriedade que são criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
4.  Clique com botão direito **Gerenciador de recursos de servidor de arquivos (local)**e clique em **configurar opções**.  
  
5.  Sobre o **notificações por Email** guia, configure o seguinte:  
  
    -   No **endereço IP ou nome do servidor SMTP**, digite o nome do servidor SMTP em sua rede.  
  
    -   No **administradores destinatários padrão** caixa, digite o endereço de email do administrador que deve receber a notificação.  
  
    -   Na **padrão "De" email**, digite o endereço de email que deve ser usado para enviar as notificações.  
  
6.  Clique em **Okey**.  
  
![guias de soluções](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Etapa 3: Criar uma tarefa de gerenciamento de arquivos  
Nesta etapa, nós usamos o console do Gerenciador de recursos do servidor de arquivos para criar uma tarefa de gerenciamento de arquivo que será executado no último dia do mês e expiram todos os arquivos com os seguintes critérios:  
  
-   O arquivo não é classificado como se estivessem em legais.  
  
-   O arquivo é classificado como tendo um período de retenção de longo prazo.  
  
-   O arquivo não foi modificado nos últimos 10 anos.  
  
[Fazer isso usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Para criar uma tarefa de gerenciamento de arquivos  
  
1.  Fazer logon servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  Abra o Gerenciador de recursos do servidor de arquivos. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de recursos do servidor de arquivos**.  
  
3.  Clique com botão direito **tarefas de gerenciamento de arquivo**e clique em **criar tarefas de gerenciamento de arquivo**.  
  
4.  No **geral** guia, o **nome da tarefa**, digite um nome para a tarefa de gerenciamento de arquivo, como tarefas de retenção.  
  
5.  Sobre o **escopo**, clique em **adicionar**e escolher as pastas que devem ser incluídas nessa regra, como documentos D:\Finance.  
  
6.  Sobre o **ação** guia, o **tipo**, clique em **expiração do arquivo**. No **diretório de expiração**, digite um caminho para uma pasta no servidor de arquivos local onde os arquivos expirados serão movidos. Esta pasta deve ter uma lista de controle de acesso que concede acesso de somente administradores de servidor de arquivos.  
  
7.  Sobre o **notificação**, clique em **adicionar**.  
  
    -   Selecione o **enviar email para os seguintes administradores** caixa de seleção.  
  
    -   Selecione o **enviar um email para os usuários com arquivos afetados** caixa de seleção e, em seguida, clique em **Okey**.  
  
8.  Sobre o **condição**, clique em **adicionar**e adicione as seguintes propriedades:  
  
    -   No **propriedade** listar, clique em **detectabilidade**. No **operador** listar, clique em **não igual**. No **valor** listar, clique em **segure**.  
  
    -   No **propriedade** listar, clique em **período de retenção**. No **operador** listar, clique em **igual**. No **valor** listar, clique em **a longo prazo**.  
  
9. Sobre o **condição**, selecione o **dias desde que o arquivo foi modificado** caixa de seleção e, em seguida, defina o valor como **3650**.  
  
10. No **agendamento**, clique no **mensal** opção e, em seguida, selecione o **última** caixa de seleção.  
  
11. Clique em **Okey**.  
  
![guias de soluções](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O cmdlet do Windows PowerShell ou os cmdlets a seguir realizam as mesmas funções que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles possam aparecer com quebras automáticas em várias linhas devido a restrições de formatação.  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>Etapa 4: Classificar manualmente um arquivo  
Nesta etapa, nós classificamos manualmente um arquivo para ficar em espera legal. A pasta pai desse arquivo será classificada com um período de retenção de longo prazo.  
  
#### <a name="to-manually-classify-a-file"></a>Para classificar manualmente um arquivo  
  
1.  Fazer logon servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  Navegue até a pasta que foi configurada no escopo da tarefa de gerenciamento de arquivo criado na etapa 3.  
  
3.  Clique com botão direito na pasta e clique em **propriedades**.  
  
4.  Sobre o **Classification**, clique em **período de retenção**, clique em **a longo prazo**e, em seguida, clique em **Okey**.  
  
5.  Clique com botão direito a um arquivo dentro dessa pasta e clique em **propriedades**.  
  
6.  No **Classification**, clique em **detectabilidade**, clique em **segure**, clique em **aplicar**e clique em **Okey**.  
  
7.  No servidor de arquivos, execute a tarefa de gerenciamento de arquivos usando o console do Gerenciador de recursos do servidor de arquivos. Depois que a tarefa de gerenciamento de arquivo for concluída, verifique a pasta e certifique-se de que o arquivo não foi movido para o diretório de expiração.  
  
8.  Clique com botão direito do mesmo arquivo dentro dessa pasta e clique em **propriedades**.  
  
9. No **Classification**, clique em **detectabilidade**, clique em **não aplicável**, clique em **aplicar**e clique em **Okey**.  
  
10. No servidor de arquivos, reexecutar a tarefa de gerenciamento de arquivos usando o console do Gerenciador de recursos do servidor de arquivos. Depois que a tarefa de gerenciamento de arquivo for concluída, verifique a pasta e certifique-se de que o arquivo foi movido para o diretório de expiração.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: Implementar retenção de informações em servidores de arquivos](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planejar a retenção de informações em servidores de arquivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

