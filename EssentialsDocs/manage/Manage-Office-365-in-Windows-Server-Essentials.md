---
title: Gerenciar o Office 365 no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d897c9186ff74a80d531cf84961709de54459f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433282"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gerenciar o Office 365 no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você integra seu servidor Windows Server Essentials com o Microsoft Office 365, você pode gerenciar suas contas online e serviços do Office 365, juntamente com seus recursos locais do painel do Windows Server Essentials. Neste tópico, você descobrirá o que você pode se beneficiar com a integração do seu servidor com o Office 365, como fazê-lo e como gerenciar e solucionar problemas de sua integração do Office 365.  
  
  
  
> [!IMPORTANT]
>   Integração do Office 365 só tem suporte em um ambiente de controlador de domínio único. Além disso, o Assistente de integração do Office 365 deve ser executado em um controlador de domínio.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Por que posso integrar o Office 365 com meu servidor?](#BKMK_IntegrationOverview)  
  
-   [Configurar a integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gerenciar integração do Office 365](#BKMK_ManageIntegration)  
  
-   [Solucionar problemas de integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> Por que posso integrar o Office 365 com meu servidor?  
 Há muitos bons motivos para integrar o Office 365 com seu servidor Windows Server Essentials. Se você gerencia alguns dos seus recursos internamente, mas usa o Office 365 para outros serviços, você poderá gerenciar seus serviços do Office 365 e os recursos do painel, juntamente com seus recursos locais, em vez de trabalhar em dois locais.  
  
- Gerencie as contas online que dar a seus usuários acesso ao Office 365, juntamente com suas contas de usuário:  
  
  -   Criar contas de serviços Online da Microsoft para os usuários em uma única etapa, ou criar contas de usuário no servidor para suas contas online existentes. Você também pode adicionar uma conta online para uma conta de usuário novo ou existente.  
  
       Quando você cria as contas online do painel, os usuários entram no Office 365 com a mesma senha que eles usam no servidor. Se eles alterarem a senha da conta de usuário, a senha online também será alterada. E você sabe que as senhas de conta online sempre atendem aos requisitos de segurança definidos para suas contas de usuário.  
  
  -   Gerencie uma conta online com a conta de usuário em todo o ciclo de vida da conta de usuário. Se você desativar uma conta de usuário, a conta online será desativada também. Se você remover uma conta de usuário, a conta online também será removida.  
  
  -   Em um servidor Windows Server Essentials, gerencie grupos de distribuição de Exchange Online para email também.  
  
- Enviar e receber emails do domínio de Internet da sua organização (por exemplo, contoso.com) vinculando um domínio de Internet personalizado à sua assinatura do Office 365.  
  
- Gerencie sua assinatura e a integração do Office 365 do painel.  
  
- Se sua assinatura incluir bibliotecas do SharePoint Online, a integração do Office 365 com um servidor Windows Server Essentials permite que você:  
  
  - Criar e gerenciar suas bibliotecas do SharePoint Online do painel.  
  
    > [!NOTE]
    >  Você também poderá usar o aplicativo My Server 2012 R2 para trabalhar com documentos em bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou telefone do Windows. Esse recurso só está disponível para Windows Server Essentials. Para obter mais informações, consulte [usar o aplicativo My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Alterar permissões para um site de equipe do SharePoint Online do painel ou abra o site de equipe a partir do painel para fazer outras alterações.  
  
    Para obter mais informações, consulte [Gerenciar SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Se você se inscrever para o Exchange Online, a integração do Office 365 com um servidor Windows Server Essentials permite que você gerencie os dispositivos móveis que seus usuários utilizam para se conectar ao seu servidor de email da empresa:  
  
  -   Requer proteção por senha quando os dispositivos móveis se conectam ao servidor de email da empresa. Defina um tamanho mínimo da senha, o número permitido de tentativas de logon com falha e o tempo mínimo necessário entre as tentativas de logon.  
  
  -   Bloqueie um dispositivo móvel de se conectar ao Exchange Online se você souber de problemas de segurança com o modelo do dispositivo.  
  
  -   Se um dispositivo móvel for perdido ou roubado, limpe o dispositivo para excluir quaisquer dados confidenciais da empresa na próxima vez que o dispositivo for ativado.  
  
- Integração do Office 365 oferece novas maneiras de se conectar aos recursos e serviços do Office 365:  
  
  -   Abra os serviços do Office 365 da barra inicial do Windows Server Essentials. Para obter informações, consulte [Quick Start Guide to Using Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Use o aplicativo My Server 2012 R2 para trabalhar com documentos em bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou telefone do Windows. Para obter informações, consulte [usar o aplicativo My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esse recurso só está disponível no Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a> Configurar a integração do Office 365  
 Você pode integrar seu servidor com o Office 365 a qualquer momento depois de concluir a instalação do servidor. Se você ainda não tiver uma assinatura do Office 365, você pode comprar uma ou se inscrever para uma assinatura de avaliação gratuita.  
  
 Você fará as seguintes tarefas:  
  
-   [Etapa 1: Verifique os requisitos de integração do Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Etapa 2: Integrar o servidor com o Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Etapa 3: Vincular o nome de domínio de Internet da sua organização ao Office 365 (opcional)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> Etapa 1: Verifique os requisitos de integração do Office 365  
 Antes de começar, verifique se que o servidor atende aos seguintes requisitos:  
  
-   O servidor pode ter um dos seguintes sistemas operacionais:  Windows Server Essentials, Windows Server Essentials ou o sistema operacional Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter com a função experiência Windows Server Essentials instalada.  
  
-   O ambiente só pode ter um controlador de domínio, e você deve executar a integração do Office 365 no controlador de domínio.  
  
-   Você deve ser capaz de se conectar à Internet do servidor.  
  
-   Você deve instalar todas as atualizações críticas e importantes no servidor antes de iniciar a integração do Office 365.  
  
-   Se você quiser usar o domínio de Internet da sua organização em endereços de email e com os recursos do SharePoint Online, você precisará registrar o nome de domínio para que possa vincular o domínio ao Office 365 durante a integração. Para obter mais informações, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Você não precisa se inscrever para o Office 365 com antecedência. Você poderá comprar uma assinatura ou se inscrever para uma avaliação gratuita durante a integração do Office 365. Se você quiser dar uma olhada planos e preços para o Office 365 [comparar os planos do Office 365 para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a> Etapa 2: Integrar o servidor com o Microsoft Office 365  
 Execute o procedimento a seguir no controlador de domínio para integrar seu servidor Windows Server Essentials com o Office 365.  
  
> [!NOTE]
>  O procedimento 's o mesmo se você tiver o Windows Server Essentials ou Windows Server Essentials, mas você começará a partir de um local diferente na Home page. No Windows Server Essentials, você integrar o servidor com o Office 365 e outros serviços Online da Microsoft sobre o **Services** guia. No Windows Server Essentials, a integração do Office 365 é executada na **Email** guia.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Para integrar o servidor com o Office 365  
  
1. Entrar no servidor como administrador e abra o painel do Windows Server Essentials.  
  
2. Sobre o **Home** , clique em **serviços** (no Windows Server Essentials, clique em **Email**), clique em **integrar com o Microsoft Office 365**, e, em seguida, clique em **configurar a integração do Microsoft Office 365**.  
  
    O Assistente de Integração com o Microsoft Office 365 é exibido.  
  
3. Na página **Começar**, execute uma das seguintes ações:  
  
   -   Se você não tiver uma assinatura do Office 365, clique em **próxima**e siga as instruções para assinar o Office 365 ou o sinal de backup para uma assinatura de avaliação.  
  
        Você precisará entrar no Office 365 antes de retornar para o assistente. Mas você não precisa executar qualquer uma das tarefas a **comece por aqui** seção do portal do Office 365.  
  
   -   Se você já tiver uma assinatura do Office 365 que você deseja integrar com o servidor, selecione **já tenho uma assinatura para o Office 365**e, em seguida, clique em **próxima**.  
  
4. Siga as instruções para concluir o assistente.  
  
   Depois que o assistente for concluído com êxito, você observará as seguintes alterações para o painel:  
  
-   Há uma nova **Office 365** página, que é usada para gerenciar a integração e sua assinatura do Office 365.  
  
-   Sobre o **os usuários** página, você verá um **contas Online** guia em que você pode criar e gerenciar as contas do Microsoft Online Services concedem aos usuários acesso ao Office 365. Se você estiver usando o Exchange Online e tiver um servidor Windows Server Essentials, você verá também uma **grupos de distribuição** guia.  
  
-   O **armazenamento** página em um servidor Windows Server Essentials tem um **bibliotecas do SharePoint** guia para gerenciar bibliotecas do SharePoint Online e alterar as permissões para seus sites de equipe. Cada plano de negócios do Office 365 inclui esses recursos básicos de SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a> Etapa 3: Vincular o nome de domínio de Internet da sua organização ao Office 365 (opcional)  
 Se você quiser usar seu próprio domínio de Internet em emails endereçados à sua organização e as URLs para os recursos do SharePoint Online, você pode vincular um domínio personalizado à sua assinatura do Office 365. Se você integrar seu servidor Windows Server Essentials com o Office 365, você pode fazer isso partir do painel.  
  
 É fácil fazer isso antes de criar contas online para seus usuários, você pode usar o domínio quando você cria em massa as contas online.  
  
 Obtenha um nome de domínio, quando você se inscreve para o Office 365? por exemplo, *contoso*. onmicrosoft.com. Se você preferir usar um nome de domínio diferentes? por exemplo, simplesmente contoso.com? você pode. Você precisará comprar um nome de domínio, se ainda não tiver um e alterar alguns dos registros de DNS.  
  
 Como configurar um domínio personalizado para usar com o Office 365 envolve quatro tarefas:  
  
1. **Comprar um nome de domínio.** Ou seja, registrá-lo com um registrador de domínio ou o provedor de hospedagem de DNS.  
  
   -   Escolha um nome de domínio que funcione com o Office 365. Você pode usar um nome de domínio de 2º nível? por exemplo, buycontoso.com?, mas não é um nome de domínio de 3º nível? por exemplo, marketing.contoso.com. Para obter mais informações sobre como escolher um domínio para usar no Office 365, consulte [domínios](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Compre de um registrador de domínio que permite que os registros DNS (servidor) de nomes de domínio exigidos pelo Office 365. Para saber quais registradores de domínio permitem os registros DNS exigidos, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se você já registrou seu domínio com um registrador diferente, não se preocupe; Você pode transferir o domínio para um registrador diferente ao vincular o domínio ao Office 365.  
  
2. **Configure os registros DNS que permitem que os serviços do Office 365 usem o nome de domínio.** A maneira mais fácil é deixar o Assistente Configurar os registros DNS para você ao vincular o domínio à sua assinatura do Office 365 na etapa 3. Se você preferir fazer isso por conta própria, consulte [como configurar o DNS manualmente os registros para integração do Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Vincule seu domínio de Internet personalizado à sua assinatura do Office 365.** Você usará o **vincular um domínio ao Office 365** ação.  
  
4. **Verifique se os serviços do Office 365 estão usando o novo nome de domínio.**  
  
   Se você concluir as etapas 1 e 2 antes de usar o **vincular um domínio ao Office 365** tarefa, o assistente pode vincular o nome de domínio para o Office 365. Como alternativa, você pode ter um assistente para ajudá-lo com algumas ou todas as etapas 1 e 2.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Para vincular o domínio de Internet da sua organização ao Office 365  
  
1.  No Painel, abra a página **Office 365** e, em seguida, clique em **Vincular um domínio ao Office 365**.  
  
2.  Siga as instruções para concluir o assistente.  
  
     O assistente pode ajudá-lo com algumas ou todas as etapas para registrar, configurar e vincular um nome de domínio de Internet novo ou existente para usar no Office 365.  
  
     Clique no link de ajuda na página do assistente para obter as informações necessárias para concluir uma tarefa. Ou consulte o conjunto de backup de uma seção de nome de domínio [Manage Remote Web Access in Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para uma visão geral do processo e os requisitos.  
  
    > [!NOTE]
    >  Para usar o assistente para registrar um novo nome de domínio, você deve usar um dos provedores de serviços de nome de domínio que têm parcerias com a Microsoft para fornecer integração com o assistente. Para localizar um registrador de nomes de domínio, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se o assistente detectar que o seu nome de domínio não é gerenciado pelo servidor, você precisará configurar manualmente os registros DNS necessários para concluir a configuração. Para obter instruções, consulte [Como configurar manualmente os registros DNS para a integração do Office 365](#BKMK_ManuallyConfigureDNS)posteriormente nesse tópico.  
  
4.  Verifique se que o domínio está sendo usado no Office 365.  
  
     Há uma espera pouco depois de concluir o assistente, enquanto o registrador de nome de domínio verifica os registros DNS. Isso acontece automaticamente. Você não precisa fazer nada. Mas geralmente leva cerca de uma hora? e, às vezes, um pouco mais. Quando a verificação de domínio for concluída, o **Office 365** página listará domínio da sua organização.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> Como configurar manualmente os registros DNS para a integração do Office 365  
 Se o Assistente de Link do Domínio ao Office 365 detectar que o nome do domínio não é gerenciado pelo servidor, para concluir a configuração, você deverá configurar manualmente os registros de servidores de nomes de domínio necessários. Nesse caso, você encontrará uma lista de registros DNS que deve ser configurada no **%username%\NewDNSRecords_ (n). txt**, onde *(n)* é um número aleatório.  
  
 A tabela a seguir descreve os registros DNS que você deve adicionar. Os métodos de entrada podem variar de acordo com registradores de nome de domínio diferentes. Se tiver alguma dúvida, peça ajuda ao registrador de nome de domínio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Registros DNS necessários para vincular um nome de domínio da Internet personalizado para o Office 365  
  
|Fornecer manutenção|Registros DNS necessários|Finalidade|  
|-------------|--------------------------|-------------|  
|(Vários serviços)|MX| O Office 365 usa esse registro para verificar se você possui um nome de domínio específico. Esse registro MX não interfere com o roteamento de mensagens de email.|  
|Exchange Online|MX|Oferece roteamento de mensagens de email. **Importante:**  Se estiver migrando emails, não atribua uma preferência de zero (**0**) ao novo registro MX. Verifique se que o valor do registro é maior que o valor que é atribuído ao registro MX atual. Quando a migração de emails for concluída e você está pronto para alterar o servidor de email para o Office 365, ter seu registrador de nome de domínio redefina o valor de preferência do novo registro MX.|  
|Exchange Online|Alias (CNAME)|O registro de descoberta automática é usado para ajudar os usuários a estabelecer facilmente uma conexão entre o Exchange Online e seus clientes de área de trabalho do Outlook ou um cliente de email móvel. **Observação:**  Se você preferir acessar o Outlook Web Access usando seu nome de domínio da organização (por exemplo, http://mail.contoso.com) em vez da URL padrão (https://outlook.com/owa/office365.com), você pode configurar o registro de Alias (CName) da seguinte maneira: **Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|Especifica que outlook.com, o domínio usado pelos servidores de email do Office 365, está autorizado a enviar emails em nome de seu domínio. Crie este registro para ajudar a impedir que o email de saída seja marcado como spam.|  
|Lync Online|SRV|Ajuda a habilitar a federação com outros serviços de mensagens instantâneas como Windows Live ou Yahoo!.|  
|Lync Online|SRV|Descobre automaticamente o registro que é usado para ajudar os usuários a configurarem facilmente uma conexão entre o cliente de área de trabalho do Lync e o Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Domínio após a verificação for concluída, não tente adicionar ou fazer outras alterações para os registros DNS no portal do Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> Próxima etapa  
  
-   Criar contas de serviços Online da Microsoft para os usuários.  
  
     Para usar os serviços do Office 365, seus usuários devem ter uma conta de serviços Online da Microsoft associada a sua conta de usuário de rede. O Painel torna isso fácil. Se você estiver usando uma nova assinatura do Office 365, você pode criar em massa contas online para suas contas de usuário existentes. Se você estiver integrando um novo servidor com uma assinatura do Office 365 que você já está usando, você pode importar contas de usuário de suas contas online existentes. Para procedimentos, consulte [gerenciar contas Online para usuários](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  No painel do Windows Server Essentials, contas de serviços Online da Microsoft são denominadas contas do Office 365. As contas são iguais, somente a terminologia foi alterada.  
  
##  <a name="BKMK_ManageIntegration"></a> Gerenciar integração do Office 365  
 Após você integrar seu servidor com o Office 365, o **Office 365** página no painel exibe informações sobre sua assinatura do Office 365 e disponibilizará as seguintes tarefas:  
  
-   [Gerenciar sua assinatura do Office 365](#BKMK_ManageO365) ? Altere a conta de administrador que você usa para gerenciar a assinatura. Abra o painel de administração do Office 365 para gerenciar sua assinatura.  
  
-   [Vincular o domínio de Internet da sua organização ao Office 365](#BKMK_StepThree) ? Se você quiser ser capaz de enviar e receber emails endereçados ao seu próprio domínio, você pode vincular o domínio ao Office 365. (Discutido anteriormente, na [etapa 3: Vincular o domínio da sua organização ao Office 365](#BKMK_StepThree).)  
  
-   [Desabilitar a integração do Office 365](#BKMK_Disable) ? Se você não quiser gerenciar seus serviços do Office 365, assinatura e contas online do painel, você pode desabilitar a integração do Office 365. Os serviços ainda estão disponíveis no portal do Office 365.  
  
###  <a name="BKMK_ManageO365"></a> Gerenciar sua assinatura do Office 365  
 Se você precisar fazer alterações à sua assinatura do Office 365, enquanto estiver trabalhando no servidor, você pode abrir a assinatura no Office 365 desde o **Office 365** página do painel. Você também pode alterar a conta de administrador que o servidor usa para fazer alterações em serviços do Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Para abrir a sua assinatura do Painel Admin do Office 365  
  
1.  No painel do Windows Server Essentials, abra o **Office 365** página.  
  
2.  Em **Tarefas de configuração**, clique em **Gerenciar o Office 365**.  
  
3.  Entrar no Office 365 com a conta online da Microsoft que você usa para gerenciar sua assinatura.  
  
     Abre o painel de administração do Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Para alterar a conta de administrador do Office 365  
  
1.  No Painel, clique em **Office 365**.  
  
2.  Em **Tarefas de configuração**, clique em **Alterar a conta de administrador do Office 365**. O Assistente de Alteração da Conta de Administrador é exibido. (No Windows Server Essentials, o assistente é chamado configurar o Office 365 conta de administrador).  
  
3.  Digite as credenciais para a conta que você deseja usar para se conectar à sua assinatura do Office 365 e, em seguida, clique em **próxima**.  
  
4.  Clique em **Fechar**. O Painel é reiniciado.  
  
###  <a name="BKMK_Disable"></a> Desabilitar a integração do Office 365  
 Se você decidir que não deseja gerenciar suas contas online e serviços do Office 365 do painel, você pode desabilitar a integração do Office 365. Sua assinatura do Office 365 permanece ativa, e quaisquer alterações de configuração feitas no painel permanecem em vigor. Por exemplo, você receberá o email endereçado a um nome de domínio que você vinculou à sua assinatura do Office 365. Você não perderá quaisquer emails, e os controles que você definir para dispositivos móveis ainda são usados no Exchange Online.  
  
 No futuro, você gerenciará sua assinatura do Office 365, serviços e recursos no Office 365 e seus usuários precisam gerenciar as senhas das contas online no Office 365. A sincronização de senha não acontece e desabilitação ou remoção de uma conta de usuário não terá efeito na conta do usuário online.  
  
 Como o software de integração do Office 365 é instalado no servidor local, você pode desabilitar o recurso mesmo se o serviço de integração não pode se conectar ao Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Para desabilitar a integração do Office 365 com o servidor  
  
1.  No Painel, clique em **Office 365**.  
  
2.  Clique em **Desabilitar a integração do Office 365** O Assistente de Desabilitação do Office 365 aparece.  
  
3.  Siga as instruções para concluir o assistente.  
  
> [!NOTE]
>  Para habilitar novamente a integração do Office 365, use o **integrar com o Office 365** tarefa a **Service** no painel **Home** página. Para obter instruções, consulte [etapa 2: Integrar seu servidor Windows Server Essentials com o Microsoft Office 365](#BKMK_StepTwo), anteriormente neste tópico.  
  
##  <a name="BKMK_Troubleshoot"></a> Solucionar problemas de integração do Office 365  
 Esta seção fornece informações para ajudá-lo a solucionar problemas comuns que você pode encontrar ao usar os recursos de integração do Office 365 no Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a> Algumas contas de serviços Online da Microsoft não foram criadas.  
 **Descrição**  
  
 Uma tentativa de criar uma ou mais contas de serviços Online da Microsoft do painel não foi bem-sucedida.  
  
 **Solução**  
  
1.  Clique no link na página de conclusão do assistente para abrir um arquivo de resultados que contém informações detalhadas sobre cada solicitação de criação de conta não foi concluída com êxito. Por exemplo, um resultado pode informar que já existe uma conta de serviços Online da Microsoft com o nome de uma conta solicitada.  
  
2.  Execute as ações recomendadas para resolver cada erro.  
  
3.  Se o problema persistir, reinicie o servidor e, em seguida, tente criar contas online novamente.  
  
###  <a name="BKMK_ProblemUninstalling"></a> Houve um problema desinstalando a integração do Office 365  
 **Descrição**  
  
 Erro desconhecido ao tentar desabilitar a integração do Office 365.  
  
 **Solução**  
  
1.  Verifique se o computador está conectado à Internet e tente novamente.  
  
2.  Se o erro ocorrer novamente, reinicie o servidor e tente novamente.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da integração de serviços do Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Visão geral da integração de serviços do Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guia de início rápido para usar o Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gerenciar Serviços Online da Microsoft](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
