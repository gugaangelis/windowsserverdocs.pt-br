---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 994eadfa205b62c5a512ab130c71fa6c22d1cff6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357538"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode definir períodos de retenção para pastas e colocar arquivos em retenção legal usando a Infraestrutura de Classificação de Arquivos e o Gerenciador de Recursos de Servidor de Arquivos.  
  
**Neste documento**  
  
-   [Pré-requisitos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [Etapa 1: criar definições de propriedade de recurso](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Etapa 2: configurar notificações](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [Etapa 3: criar uma tarefa de gerenciamento de arquivos](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Etapa 4: classificar um arquivo manualmente](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="prerequisites"></a>Pré-requisitos  
As etapas neste tópico presumem que você tenha um servidor SMTP configurado para notificações de expiração de arquivo.  
  
## <a name="BKMK_Step1"></a>Etapa 1: criar definições de propriedade de recurso  
Nessa etapa, habilitamos as propriedades do recurso Período de Retenção e Descoberta de forma que a Infraestrutura de Classificação de Arquivos possa usar essas propriedades de recurso para marcar os arquivos examinados em uma pasta de rede compartilhada.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Para criar definições de propriedade de recurso  
  
1.  No controlador de domínio, entre no servidor como membro do grupo de segurança Admins. do Domínio.  
  
2.  Abra o Centro Administrativo do Active Directory. No Gerenciador do Servidor, clique em **Ferramentas** e em **Centro Administrativo do Active Directory**.  
  
3.  Expanda **Controle de Acesso Dinâmico** e clique em **Propriedades do Recurso**.  
  
4.  Clique com o botão direito do mouse em **Período de Retenção** e em **Habilitar**.  
  
5.  Clique com o botão direito do mouse em **Descoberta**e em **Habilitar**.  
  
![guias de solução](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Etapa 2: configurar notificações  
Nessa etapa, usamos o console do Gerenciador de Recursos de Servidor de Arquivos para configurar o servidor SMTP, o endereço de email do administrador padrão e o endereço de email padrão de onde os relatórios são enviados.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>Para configurar notificações  
  
1.  Entre no servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  No prompt de comando do Windows PowerShell, digite **Update-FsrmClassificationPropertyDefinition** e pressione ENTER. Isso sincronizará as definições de propriedade criadas no controlador de domínio para o servidor de arquivos.  
  
3.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
4.  Clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos (local)** e em **Configurar Opções**.  
  
5.  Na guia **Notificação de Email**, configure as seguintes opções:  
  
    -   Na caixa **Nome do servidor SMTP ou endereço IP**, digite o nome do servidor SMTP em sua rede.  
  
    -   Na caixa **Administradores destinatários padrão**, digite o endereço de email do administrador que deve receber a notificação.  
  
    -   Na caixa de **endereço de email padrão "de"** , digite o endereço de email que deve ser usado para enviar as notificações.  
  
6.  Clique em **OK**.  
  
![guias de solução](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>Etapa 3: criar uma tarefa de gerenciamento de arquivos  
Nessa etapa, usamos o console do Gerenciador de Recursos de Servidor de Arquivos para criar uma tarefa de gerenciamento de arquivos que será executada no último dia do mês e expirará quaisquer arquivos com os seguintes critérios:  
  
-   O arquivo não está classificado como estando em retenção legal.  
  
-   O arquivo está classificado como tendo um período de retenção de longo prazo.  
  
-   O arquivo não foi modificado nos últimos 10 anos.  
  
[Siga esta etapa usando o Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>Para criar uma tarefa de gerenciamento de arquivos  
  
1.  Entre no servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  Abra o Gerenciador de Recursos de Servidor de Arquivos. No Gerenciador do Servidor, clique em **Ferramentas** e em **Gerenciador de Recursos de Servidor de Arquivos**.  
  
3.  Clique com o botão direito do mouse em **Tarefas de Gerenciamento de Arquivos**e clique em **Criar Tarefa de Gerenciamento de Arquivos**.  
  
4.  Na guia **Geral**, na caixa **Nome da tarefa**, digite um nome para a tarefa de gerenciamento de arquivos, como Tarefa de Retenção.  
  
5.  Na guia **Escopo** , clique em **Adicionar**e escolha as pastas que devem ser incluídas nessa regra, como D:\Finance Documents.  
  
6.  Na guia **Ação** , na caixa **Tipo** , clique em **Expiração do arquivo**. Na caixa **Diretório de expiração** , digite um caminho para a pasta no servidor de arquivos local para o qual os arquivos expirados serão movidos. Essa pasta deve ter uma lista de controle de acesso que conceda somente acesso aos administradores do servidor de arquivos.  
  
7.  Na guia **Notificação**, clique em **Adicionar**.  
  
    -   Selecione a caixa de seleção **Enviar email para os seguintes administradores**.  
  
    -   Selecione a caixa de seleção **Enviar um email aos usuários com arquivos atingidos** e clique em **OK**.  
  
8.  Na guia **Condição**, clique em **Adicionar** e adicione as seguintes propriedades:  
  
    -   Na lista **Propriedade** , clique em **Descoberta**. Na lista **Operador**, clique em **Diferente de**. Na lista **Valor** , clique em **Esperar**.  
  
    -   Na lista **Propriedade**, clique em **Período de Retenção**. Na lista **Operador**, clique em **Igual**. Na lista **Valor** , clique em **Longo Prazo**.  
  
9. Na guia **Condição**, selecione a caixa de seleção **Dias passados desde a última modificação do arquivo** e defina o valor como **3650**.  
  
10. Na guia **Agendar** , clique na opção **Mensal** , e selecione a caixa de seleção **Último** .  
  
11. Clique em **OK**.  
  
![guias de solução](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
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
  
## <a name="BKMK_Step4"></a>Etapa 4: classificar um arquivo manualmente  
Nessa etapa, classificamos manualmente um arquivo para ficar em retenção legal. A pasta pai desse arquivo será classificada com um período de retenção de longo prazo.  
  
#### <a name="to-manually-classify-a-file"></a>Para classificar um arquivo manualmente  
  
1.  Entre no servidor de arquivos como um membro do grupo de segurança Administradores.  
  
2.  Navegue até a pasta configurada no escopo da tarefa de gerenciamento de arquivos criada na Etapa 3.  
  
3.  Clique com o botão direito do mouse na pasta e, em seguida, clique em **Propriedades**.  
  
4.  Na guia **Classificação** , clique em **Período de Retenção**, clique em **Longo Prazo**, em seguida e em **OK**.  
  
5.  Clique com o botão direito do mouse naquela pasta e em **Propriedades**.  
  
6.  Na guia **Classificação** , clique em **Descoberta**, em **Esperar**, em **Aplicar**e em **OK**.  
  
7.  No servidor de arquivos, execute a tarefa de gerenciamento de arquivos usando o Gerenciador de Recursos de Servidor de Arquivos. Depois de concluir a tarefa de gerenciamento de arquivos, verifique a pasta e assegure que o arquivo não tenha sido movido para o diretório de expiração.  
  
8.  Clique com o botão direito do mouse no mesmo arquivo dentro da pasta e clique em **Propriedades**.  
  
9. Na guia **Classificação**, clique em **Descoberta**, clique em **Não Aplicável**, em **Aplicar** e em **OK**.  
  
10. No servidor de arquivos, execute a tarefa de gerenciamento de arquivos novamente usando o Gerenciador de Recursos de Servidor de Arquivos. Depois de concluir a tarefa de gerenciamento de arquivos, verifique a pasta e assegure que o arquivo tenha sido movido para o diretório de expiração.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: implementar a retenção de informações em servidores de arquivos](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [Planejar a retenção de informações em servidores de arquivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

