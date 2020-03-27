---
title: Gerenciar o Office 365 no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: d8051431f55a7a3e05f0a1917a003df044533571
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311252"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gerenciar o Office 365 no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ao integrar seu servidor do Windows Server Essentials com o Microsoft Office 365, você pode gerenciar seus serviços do Office 365 e contas online junto com seus recursos locais no painel do Windows Server Essentials. Neste tópico, você descobrirá o que pode obter integrando seu servidor com o Office 365, como fazer isso e como gerenciar e solucionar problemas da integração do Office 365.  
  
  
  
> [!IMPORTANT]
>   Há suporte para a integração do Office 365 em um único ambiente do controlador de domínio. Além disso, o assistente de integração do Office 365 deve ser executado em um controlador de domínio.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Por que devo integrar o Office 365 ao meu servidor?](#BKMK_IntegrationOverview)  
  
-   [Configurar a integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gerenciar a integração do Office 365](#BKMK_ManageIntegration)  
  
-   [Solucionar problemas de integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="why-should-i-integrate-office-365-with-my-server"></a><a name="BKMK_IntegrationOverview"></a>Por que devo integrar o Office 365 ao meu servidor?  
 Há muitos bons motivos para integrar o Office 365 ao servidor do Windows Server Essentials. Se você gerenciar alguns de seus recursos internamente, mas usar o Office 365 para outros serviços, poderá gerenciar seus serviços e recursos do Office 365 no painel, juntamente com seus recursos locais, em vez de trabalhar em dois lugares.  
  
- Gerencie as contas online que dão aos usuários acesso ao Office 365 junto com suas contas de usuário:  
  
  -   Criar contas de serviços Online da Microsoft para os usuários em uma única etapa, ou criar contas de usuário no servidor para suas contas online existentes. Você também pode adicionar uma conta online para uma conta de usuário novo ou existente.  
  
       Quando você cria as contas online no painel, os usuários entram no Office 365 com a mesma senha que usam no servidor. Se eles alterarem a senha da conta de usuário, a senha online também será alterada. E você sabe que as senhas de conta online sempre atendem aos requisitos de segurança definidos para suas contas de usuário.  
  
  -   Gerencie uma conta online com a conta de usuário em todo o ciclo de vida da conta de usuário. Se você desativar uma conta de usuário, a conta online será desativada também. Se você remover uma conta de usuário, a conta online também será removida.  
  
  -   Em um servidor do Windows Server Essentials, gerencie também grupos de distribuição do Exchange Online para email.  
  
- Envie e receba emails do domínio da Internet da sua organização (por exemplo, contoso.com) vinculando um domínio de Internet personalizado à sua assinatura do Office 365.  
  
- Gerencie sua assinatura e a integração do Office 365 no painel.  
  
- Se sua assinatura inclui bibliotecas do SharePoint Online, a integração do Office 365 com um servidor do Windows Server Essentials permite que você:  
  
  - Crie e gerencie suas bibliotecas do SharePoint Online no painel.  
  
    > [!NOTE]
    >  Você também poderá usar o aplicativo meu servidor 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou Windows Phone. Este recurso só está disponível para o Windows Server Essentials. Para obter mais informações, consulte [usar o aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Altere as permissões para um site de equipe do SharePoint Online do painel ou abra o site de equipe do painel para fazer outras alterações.  
  
    Para obter mais informações, consulte [gerenciar o SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Se você assinar o Exchange Online, a integração do Office 365 com um servidor do Windows Server Essentials permite que você gerencie os dispositivos móveis que os usuários usam para se conectarem ao servidor de email da empresa:  
  
  -   Requer proteção por senha quando os dispositivos móveis se conectam ao servidor de email da empresa. Defina um tamanho mínimo da senha, o número permitido de tentativas de logon com falha e o tempo mínimo necessário entre as tentativas de logon.  
  
  -   Bloqueie um dispositivo móvel de se conectar ao Exchange Online se você souber de problemas de segurança com o modelo do dispositivo.  
  
  -   Se um dispositivo móvel for perdido ou roubado, limpe o dispositivo para excluir quaisquer dados confidenciais da empresa na próxima vez que o dispositivo for ativado.  
  
- A integração do Office 365 oferece novas maneiras de se conectar a recursos e serviços do Office 365:  
  
  -   Abra os serviços do Office 365 no Launchpad do Windows Server Essentials. Para obter informações, consulte [Guia de início rápido usar Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Use o aplicativo meu servidor 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou Windows Phone. Para obter informações, consulte [usar o aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esse recurso só está disponível no Windows Server Essentials.  
  
##  <a name="set-up-office-365-integration"></a><a name="BKMK_Configure"></a>Configurar a integração do Office 365  
 Você pode integrar seu servidor com o Office 365 a qualquer momento depois de concluir a instalação do servidor. Se você ainda não tiver uma assinatura do Office 365, poderá comprar uma ou inscrever-se para uma assinatura de avaliação gratuita.  
  
 Você fará as seguintes tarefas:  
  
-   [Etapa 1: verificar os requisitos de integração do Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Etapa 2: integrar o servidor com o Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Etapa 3: vincular o nome de domínio da Internet da sua organização ao Office 365 (opcional)](#BKMK_StepThree)  
  
###  <a name="step-1-verify-office-365-integration-requirements"></a><a name="BKMK_StepOne_VERIFY"></a>Etapa 1: verificar os requisitos de integração do Office 365  
 Antes de começar, verifique se que o servidor atende aos seguintes requisitos:  
  
-   O servidor pode ter qualquer um destes sistemas operacionais: Windows Server Essentials, Windows Server Essentials ou o sistema operacional Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.  
  
-   O ambiente pode ter apenas um controlador de domínio e você deve executar a integração do Office 365 no controlador de domínio.  
  
-   Você deve ser capaz de se conectar à Internet do servidor.  
  
-   Você deve instalar todas as atualizações críticas e importantes no servidor antes de iniciar a integração do Office 365.  
  
-   Se você quiser usar o domínio de Internet de sua organização em endereços de email e com seus recursos do SharePoint Online, será necessário registrar o nome de domínio para que você possa vincular o domínio ao Office 365 durante a integração. Para obter mais informações, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Você não precisa assinar o Office 365 com antecedência. Você poderá comprar uma assinatura ou inscrever-se para uma avaliação gratuita durante a integração do Office 365. Se você quiser dar uma olhada em planos e preços para o Office 365, [Compare os planos do office 365 para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="step-2-integrate-the-server-with-microsoft-office-365"></a><a name="BKMK_StepTwo"></a>Etapa 2: integrar o servidor com o Microsoft Office 365  
 Execute o procedimento a seguir no controlador de domínio para integrar o servidor do Windows Server Essentials com o Office 365.  
  
> [!NOTE]
>  O procedimento é o mesmo se você tiver o Windows Server Essentials ou o Windows Server Essentials, mas começará em um local diferente na Home Page. No Windows Server Essentials, você integra o servidor com o Office 365 e outros serviços online da Microsoft na guia **Serviços** . No Windows Server Essentials, a integração do Office 365 é executada na guia **email** .  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Para integrar o servidor com o Office 365  
  
1. Entre no servidor como administrador e abra o painel do Windows Server Essentials.  
  
2. Na **Home** Page do, clique em **Serviços** (no Windows Server Essentials, clique em **email**), clique em **integrar com Microsoft Office 365**e, em seguida, clique em **Configurar Microsoft Office integração 365**.  
  
    O Assistente de Integração com o Microsoft Office 365 é exibido.  
  
3. Na página **Começar**, execute uma das seguintes ações:  
  
   -   Se você não tiver uma assinatura do Office 365, clique em **Avançar**e siga as instruções para assinar o Office 365 ou inscrever-se para uma assinatura de avaliação.  
  
        Você precisará entrar no Office 365 antes de retornar ao assistente. Mas você não precisa executar nenhuma das tarefas na seção **Iniciar aqui** do portal do Office 365.  
  
   -   Se você já tiver uma assinatura do Office 365 que deseja integrar com o servidor, selecione **já tenho uma assinatura do office 365**e clique em **Avançar**.  
  
4. Siga as instruções para concluir o assistente.  
  
   Depois que o assistente for concluído com êxito, você observará as seguintes alterações no painel:  
  
-   Há uma nova página do **office 365** , que é usada para gerenciar a integração e sua assinatura do Office 365.  
  
-   Na página **usuários** , você verá uma guia **contas online** , em que você pode criar e gerenciar as contas do Microsoft Online Services que dão aos usuários acesso ao Office 365. Se você estiver usando o Exchange Online e tiver um servidor do Windows Server Essentials, você também verá uma guia **grupos de distribuição** .  
  
-   A página **armazenamento** em um servidor do Windows Server Essentials tem uma guia **bibliotecas do SharePoint** para gerenciar bibliotecas do SharePoint Online e alterar permissões para seus sites de equipe. Cada plano comercial do Office 365 inclui esses recursos básicos do SharePoint Online.  
  
###  <a name="step-3-link-your-organizations-internet-domain-name-to-office-365-optional"></a><a name="BKMK_StepThree"></a>Etapa 3: vincular o nome de domínio da Internet da sua organização ao Office 365 (opcional)  
 Se você quiser usar seu próprio domínio de Internet em email endereçado à sua organização e às URLs para seus recursos do SharePoint Online, você poderá vincular um domínio personalizado à sua assinatura do Office 365. Se você integrar seu servidor do Windows Server Essentials com o Office 365, poderá fazer isso no painel.  
  
 É mais fácil fazer isso antes de criar contas online para seus usuários para que você possa usar o domínio ao criar contas online em massa.  
  
 Você Obtém um nome de domínio ao se inscrever no Office 365? por exemplo, *contoso*. onmicrosoft.com. Se você preferir usar um nome de domínio diferente? digamos, simplesmente contoso.com? você pode. Você precisará comprar um nome de domínio se ainda não tiver um e alterar alguns dos registros DNS.  
  
 Configurar um domínio personalizado para usar com o Office 365 envolve quatro tarefas:  
  
1. **Comprar um nome de domínio.** Ou seja, registrá-lo com um registrador de domínio ou o provedor de hospedagem de DNS.  
  
   -   Escolha um nome de domínio que funcione com o Office 365. Você pode usar um nome de domínio de segundo nível? por exemplo, buycontoso.com?, mas não um nome de domínio de terceiro nível? por exemplo, marketing.contoso.com. Para obter mais informações sobre como escolher um domínio para usar no Office 365, consulte [domínios](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Comprá-lo de um registrador de domínio que permite os registros DNS (servidor de nomes de domínio) exigidos pelo Office 365. Para saber quais registradores de domínio permitem os registros DNS exigidos, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se você já registrou seu domínio com um registrador diferente, não se preocupe; Você pode transferir o domínio para um registrador diferente ao vincular o domínio ao Office 365.  
  
2. **Configure os registros DNS que permitem que os serviços do Office 365 usem o nome de domínio.** A maneira mais fácil é permitir que o assistente Configure os registros DNS para você ao vincular o domínio à sua assinatura do Office 365 na etapa 3. Se você preferir, consulte [como configurar manualmente os registros DNS para a integração do Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Vincule seu domínio de Internet personalizado à sua assinatura do Office 365.** Você usará a ação **vincular um domínio ao Office 365** .  
  
4. **Verifique se os serviços do Office 365 estão usando o novo nome de domínio.**  
  
   Se você concluir as etapas 1 e 2 antes de usar a tarefa **vincular um domínio ao office 365** , o assistente poderá vincular o nome de domínio ao Office 365. Como alternativa, você pode ter um assistente para ajudá-lo com algumas ou todas as etapas 1 e 2.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Para vincular o domínio de Internet da sua organização ao Office 365  
  
1.  No Painel, abra a página **Office 365** e, em seguida, clique em **Vincular um domínio ao Office 365**.  
  
2.  Siga as instruções para concluir o assistente.  
  
     O assistente pode ajudá-lo com algumas ou todas as etapas para registrar, configurar e vincular um nome de domínio da Internet novo ou existente para usar no Office 365.  
  
     Clique no link de ajuda na página do assistente para obter as informações necessárias para concluir uma tarefa. Ou consulte a seção configurar um nome de domínio de [gerenciar acesso via Web remoto no Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para obter uma visão geral e os requisitos do processo.  
  
    > [!NOTE]
    >  Para usar o assistente para registrar um novo nome de domínio, você deve usar um dos provedores de serviços de nome de domínio que têm parcerias com a Microsoft para fornecer integração com o assistente. Para localizar um registrador de nomes de domínio, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se o assistente detectar que o nome de domínio não é gerenciado pelo servidor, você precisará configurar manualmente os registros DNS necessários para concluir a configuração. Para obter instruções, consulte [Como configurar manualmente os registros DNS para a integração do Office 365](#BKMK_ManuallyConfigureDNS), mais adiante neste tópico.  
  
4.  Verifique se o domínio está sendo usado no Office 365.  
  
     Há um pouco de espera após a conclusão do assistente, enquanto o registrador de nome de domínio verifica os registros DNS. Isso ocorre automaticamente; Você não precisa fazer nada. Mas geralmente leva cerca de uma hora? e, às vezes, um pouco mais. Quando a verificação de domínio for concluída, a página do **Office 365** listará o domínio da sua organização.  
  
####  <a name="how-to-manually-configure-dns-records-for-office-365-integration"></a><a name="BKMK_ManuallyConfigureDNS"></a>Como configurar manualmente os registros DNS para a integração do Office 365  
 Se o Assistente de Link do Domínio ao Office 365 detectar que o nome do domínio não é gerenciado pelo servidor, para concluir a configuração, você deverá configurar manualmente os registros de servidores de nomes de domínio necessários. Nesse caso, você encontrará uma lista de registros DNS que deve configurar em **% username% \ NewDNSRecords_ (n). txt**, em que *(n)* é um número aleatório.  
  
 A tabela a seguir descreve os registros DNS que você deve adicionar. Os métodos de entrada podem variar de acordo com registradores de nome de domínio diferentes. Se tiver alguma dúvida, peça ajuda ao registrador de nome de domínio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Registros DNS necessários para vincular um nome de domínio da Internet personalizado para o Office 365  
  
|Service|Registros DNS necessários|Finalidade|  
|-------------|--------------------------|-------------|  
|(Vários serviços)|MX| O Office 365 usa esse registro para verificar se você possui um nome de domínio específico. Esse registro MX não interfere com o roteamento de mensagens de email.|  
|Exchange Online|MX|Oferece roteamento de mensagens de email. **Importante:**  Se você estiver migrando o email, não atribua uma preferência de zero (**0**) ao novo registro MX. Verifique se que o valor do registro é maior que o valor que é atribuído ao registro MX atual. Quando a migração de email for concluída e você estiver pronto para alterar o servidor de email para o Office 365, faça com que seu registrador de nome de domínio redefina o valor de preferência do novo registro MX.|  
|Exchange Online|Alias (CNAME)|O registro de descoberta automática é usado para ajudar os usuários a estabelecer facilmente uma conexão entre o Exchange Online e seus clientes de área de trabalho do Outlook ou um cliente de email móvel. **Observação:**  Se você preferir acessar o Acesso via Web do Outlook usando o nome de domínio próprio da sua organização (por exemplo, http://mail.contoso.com) em vez da URL padrão (https://outlook.com/owa/office365.com), você pode configurar o registro do alias (CName) da seguinte maneira: **tipo = CNAME, TTL = 01:00:00, hostname = email, endereço = mail. office365. com**|  
|Exchange Online|TXT|Especifica que outlook.com, o domínio usado pelos servidores de email do Office 365, está autorizado a enviar email em nome do seu domínio. Crie este registro para ajudar a impedir que o email de saída seja marcado como spam.|  
|Lync Online|SRV|Ajuda a habilitar a federação com outros serviços de mensagens instantâneas como Windows Live ou Yahoo!.|  
|Lync Online|SRV|Descobre automaticamente o registro que é usado para ajudar os usuários a configurarem facilmente uma conexão entre o cliente de área de trabalho do Lync e o Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Após a conclusão da verificação de domínio, não tente adicionar ou fazer nenhuma alteração adicional nos registros DNS do portal do Office 365.  
  
###  <a name="next-step"></a><a name="BKMK_StepFour_ACCOUNTS"></a>Próxima etapa  
  
-   Criar contas de serviços Online da Microsoft para os usuários.  
  
     Para usar os serviços do Office 365, os usuários devem ter uma conta do Microsoft Online Services associada à sua conta de usuário de rede. O Painel torna isso fácil. Se você estiver usando uma nova assinatura do Office 365, poderá criar contas online em massa para suas contas de usuário existentes. Se estiver integrando um novo servidor com uma assinatura do Office 365 que você já está usando, você poderá importar contas de usuário de suas contas online existentes. Para procedimentos, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  No painel do Windows Server Essentials, as contas do Microsoft Online Services são chamadas de contas do Office 365. As contas são iguais, somente a terminologia foi alterada.  
  
##  <a name="manage-office-365-integration"></a><a name="BKMK_ManageIntegration"></a>Gerenciar a integração do Office 365  
 Depois de integrar seu servidor com o Office 365, a página do **office 365** no painel exibe informações sobre sua assinatura do Office 365 e disponibiliza essas tarefas:  
  
-   [Gerenciar sua assinatura do Office 365](#BKMK_ManageO365) ? Altere a conta de administrador que você usa para gerenciar a assinatura. Abra o painel de administração do Office 365 para gerenciar sua assinatura.  
  
-   [Vincular o domínio de Internet da sua organização ao Office 365](#BKMK_StepThree) ? Se você quiser ser capaz de enviar e receber emails endereçados ao seu próprio domínio, você pode vincular o domínio ao Office 365. (Discutido anteriormente, na [etapa 3: vincular o domínio da sua organização ao Office 365](#BKMK_StepThree).)  
  
-   [Desabilitar a integração do Office 365](#BKMK_Disable) ? Se você não quiser gerenciar seus serviços do Office 365, sua assinatura e contas online no painel, você pode desabilitar a integração do Office 365. Os serviços ainda estão disponíveis no portal do Office 365.  
  
###  <a name="manage-your-office-365-subscription"></a><a name="BKMK_ManageO365"></a>Gerenciar sua assinatura do Office 365  
 Se você precisar fazer alterações na sua assinatura do Office 365 enquanto estiver trabalhando no servidor, poderá abrir a assinatura no Office 365 na página do **office 365** do painel. Você também pode alterar a conta de administrador que o servidor usa para fazer alterações nos serviços do Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Para abrir a sua assinatura do Painel Admin do Office 365  
  
1.  No painel do Windows Server Essentials, abra a página do **Office 365** .  
  
2.  Em **Tarefas de configuração**, clique em **Gerenciar o Office 365**.  
  
3.  Entre no Office 365 com a conta online da Microsoft que você usa para gerenciar sua assinatura.  
  
     O painel de administração do Office 365 é aberto.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Para alterar a conta de administrador do Office 365  
  
1.  No Painel, clique em **Office 365**.  
  
2.  Em **Tarefas de configuração**, clique em **Alterar a conta de administrador do Office 365**. O Assistente de Alteração da Conta de Administrador é exibido. (No Windows Server Essentials, o assistente é chamado configurar a conta de administrador do Office 365.)  
  
3.  Digite as credenciais da conta que você deseja usar para se conectar à sua assinatura do Office 365 e clique em **Avançar**.  
  
4.  Clique em **Fechar**. O Painel é reiniciado.  
  
###  <a name="disable-office-365-integration"></a><a name="BKMK_Disable"></a>Desabilitar a integração do Office 365  
 Se você decidir que não deseja gerenciar seus serviços do Office 365 e contas online no painel, você pode desabilitar a integração do Office 365. Sua assinatura do Office 365 permanece ativa e as alterações de configuração feitas a partir do painel permanecem em vigor. Por exemplo, você receberá email endereçado a um nome de domínio que você vinculou à sua assinatura do Office 365. Você não perderá nenhum email e os controles que você definir para dispositivos móveis ainda serão usados no Exchange Online.  
  
 No futuro, você gerenciará sua assinatura, seus serviços e recursos do Office 365 no Office 365, e seus usuários precisarão gerenciar as senhas para suas contas online no Office 365. A sincronização de senha não ocorre mais, e desabilitar ou remover uma conta de usuário não terá nenhum efeito na conta online do usuário.  
  
 Como o software de integração do Office 365 está instalado no servidor local, você pode desabilitar o recurso mesmo que o serviço de integração não possa se conectar ao Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Para desabilitar a integração do Office 365 com o servidor  
  
1.  No Painel, clique em **Office 365**.  
  
2.  Clique em **Desabilitar a integração do Office 365** O Assistente de Desabilitação do Office 365 aparece.  
  
3.  Siga as instruções para concluir o assistente.  
  
> [!NOTE]
>  Para habilitar a integração do Office 365 novamente, use a tarefa **integrar com o office 365** na guia **serviço** da página **inicial** do painel. Para obter instruções, consulte [Etapa 2: Integrar o servidor do Windows Server Essentials com o Microsoft Office 365](#BKMK_StepTwo), anteriormente neste tópico.  
  
##  <a name="troubleshoot-office-365-integration"></a><a name="BKMK_Troubleshoot"></a>Solucionar problemas de integração do Office 365  
 Esta seção fornece informações para ajudá-lo a solucionar problemas comuns que você pode encontrar ao usar os recursos de integração do Office 365 no Windows Server Essentials.  
  
###  <a name="some-microsoft-online-services-accounts-were-not-created"></a><a name="BKMK_AcctsNotCreated"></a>Algumas contas do Microsoft Online Services não foram criadas  
 **Descrição**  
  
 Uma tentativa de criar uma ou mais contas do Microsoft Online Services a partir do painel não foi bem-sucedida.  
  
 **Solução**  
  
1.  Clique no link na página de conclusão do assistente para abrir um arquivo de resultados que contém informações detalhadas sobre cada solicitação de criação de conta não foi concluída com êxito. Por exemplo, um resultado pode informar que já existe uma conta de serviços Online da Microsoft com o nome de uma conta solicitada.  
  
2.  Execute as ações recomendadas para resolver cada erro.  
  
3.  Se o problema persistir, reinicie o servidor e, em seguida, tente criar contas online novamente.  
  
###  <a name="there-was-a-problem-uninstalling-office-365-integration"></a><a name="BKMK_ProblemUninstalling"></a>Houve um problema ao desinstalar a integração do Office 365  
 **Descrição**  
  
 Ocorreu um erro desconhecido ao tentar desabilitar a integração do Office 365.  
  
 **Solução**  
  
1.  Verifique se o computador está conectado à Internet e tente novamente.  
  
2.  Se o erro ocorrer novamente, reinicie o servidor e tente novamente.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral da integração de serviços para o Windows Server Essentials-parte 1](https://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Visão geral da integração de serviços para o Windows Server Essentials-parte 2](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guia de Início Rápido usar Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
