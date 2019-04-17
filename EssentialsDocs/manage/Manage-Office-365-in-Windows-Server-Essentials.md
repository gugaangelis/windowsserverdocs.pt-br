---
title: Gerencie o Office 365 no Windows Server Essentials
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
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gerencie o Office 365 no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você integrar seu servidor Windows Server Essentials com o Microsoft Office 365, você pode gerenciar suas contas online e serviços do Office 365 juntamente com seus recursos locais do painel Windows Server Essentials. Neste tópico, você ll Descubra o que você pode ganhar integrando seu servidor com o Office 365, como fazê-lo e como gerenciar e solucionar problemas a integração do Office 365.  
  
  
  
> [!IMPORTANT]
>   Integração com o Office 365 é compatível apenas com um ambiente de controlador de domínio único. Além disso, o Assistente de integração do Office 365 deve executar em um controlador de domínio.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Por que deve integrar Office 365 com meu servidor?](#BKMK_IntegrationOverview)  
  
-   [Configurar a integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gerenciar a integração do Office 365](#BKMK_ManageIntegration)  
  
-   [Solucionar problemas de integração do Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>Por que deve integrar Office 365 com meu servidor?  
 Há muitas boas razões para integrar o Office 365 com o servidor Windows Server Essentials. Se você gerencia alguns dos seus recursos internos, mas usa o Office 365 para outros serviços, você ll ser capaz de gerenciar os serviços do Office 365 e recursos no painel, juntamente com seus recursos locais, em vez de trabalhar em dois locais.  
  
-   Gerencie contas online que fornecem os usuários acessam para o Office 365 junto com suas contas de usuário:  
  
    -   Crie contas de serviços Online da Microsoft para os usuários em uma única etapa ou criar contas de usuário no servidor para suas contas online existentes. Você também pode adicionar uma conta online para uma conta de usuário novo ou existente.  
  
         Quando você cria as contas online no painel, os usuários fazer logon Office 365 com a mesma senha usarem no servidor. Se ele alterar a senha da sua conta de usuário, a senha online alterações também. E você sabe que as senhas de conta online sempre atendem aos requisitos de segurança que você definiu para suas contas de usuário.  
  
    -   Gerencie uma conta online junto com a conta de usuário em todo o ciclo de vida da conta de usuário. Se você desativar uma conta de usuário, a conta on-line é desativada também. Se você remover uma conta de usuário, a conta online também é removida.  
  
    -   Em um servidor Windows Server Essentials, gerencie grupos de distribuição Exchange Online para email também.  
  
-   Enviar e receber emails do domínio da Internet s sua organização (por exemplo, contoso.com) por meio de um domínio personalizado do Internet links à sua assinatura do Office 365.  
  
-   Gerencie sua assinatura e a integração do Office 365 do painel.  
  
-   Se sua assinatura inclui bibliotecas do SharePoint Online, a integração do Office 365 com um servidor Windows Server Essentials permite que você:  
  
    -   Crie e gerencie suas bibliotecas do SharePoint Online no painel.  
  
        > [!NOTE]
        >  Ll você também poderá usar o aplicativo My Server 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu notebook, o dispositivo móvel ou o Windows phone. Esse recurso só está disponível para Windows Server Essentials. Para obter mais informações, consulte [usar o meu aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
    -   Alterar as permissões para um site da equipe do SharePoint Online no painel ou abrir o site da equipe do painel para fazer outras alterações.  
  
     Para obter mais informações, consulte [Gerenciar SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
-   Se você se inscrever para Exchange Online, integração com o Office 365 com um servidor Windows Server Essentials permite gerenciar dispositivos móveis que os usuários usam para se conectar ao seu servidor de email da empresa:  
  
    -   Exigir proteção por senha quando os dispositivos móveis se conectam ao servidor de email da empresa. Defina um comprimento mínimo da senha, o número de tentativas de entrada com falha para permitir e o tempo mínimo necessário entre as tentativas de entrada.  
  
    -   Bloquear um dispositivo móvel de se conectar ao Exchange Online, se você souber dos problemas de segurança com o modelo do dispositivo.  
  
    -   Se um dispositivo móvel é perdido ou roubado, limpe o dispositivo para excluir todos os dados confidenciais da empresa na próxima vez que o dispositivo está ativado.  
  
-    A integração do Office 365 oferece novas maneiras de se conectar aos recursos e serviços do Office 365:  
  
    -   Abra Serviços do Office 365 do Windows Server Essentials Launchpad. Para obter informações, consulte [guia de início rápido usando o Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
    -   Use o aplicativo My Server 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu notebook, o dispositivo móvel ou o Windows phone. Para obter informações, consulte [usar o meu aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esse recurso só está disponível no Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a>Configurar a integração do Office 365  
 Você pode integrar o servidor com o Office 365 a qualquer momento depois de concluir a instalação do servidor. Se você não t já tiver uma assinatura do Office 365, você pode adquirir um ou inscreva-se para uma assinatura de avaliação gratuita.  
  
 Você fará as seguintes tarefas:  
  
-   [Etapa 1: Verifique se os requisitos de integração do Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Etapa 2: Integrar o servidor com o Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Etapa 3: Vincular o nome de domínio da Internet de organização s para o Office 365 (opcional)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>Etapa 1: Verifique se os requisitos de integração do Office 365  
 Antes de começar, verifique se que o servidor atende a estes requisitos:  
  
-   O servidor pode ter qualquer um destes sistemas operacionais: Windows Server Essentials, Windows Server Essentials ou o sistema operacional Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.  
  
-   O ambiente só pode ter um controlador de domínio e você deve executar a integração do Office 365 no controlador de domínio.  
  
-   Você deve ser capaz de se conectar à Internet do servidor.  
  
-   Você deve instalar todas as atualizações críticas e importantes no servidor antes de começar a integração do Office 365.  
  
-   Se você quiser usar seu domínio da organização s Internet em endereços de email e com os recursos do SharePoint Online, você ll precisa registrar o nome de domínio para que você pode vincular o domínio ao Office 365 durante a integração. Para obter mais informações, consulte [como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Você não precisa assinar para o Office 365 com antecedência. Você poderá comprar uma assinatura ou se inscrever para uma avaliação gratuita durante a integração do Office 365. Se você d deseja dar uma olhada planos e preços do Office 365, [comparar planos do Office 365 para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a>Etapa 2: Integrar o servidor com o Microsoft Office 365  
 Execute o procedimento a seguir no controlador de domínio para integrar seu servidor Windows Server Essentials com o Office 365.  
  
> [!NOTE]
>  O procedimento s o mesmo se você tiver o Windows Server Essentials ou Windows Server Essentials, mas você ll iniciar a partir de um local diferente na Home page. No Windows Server Essentials, você integrar o servidor com o Office 365 e outros serviços Online da Microsoft sobre a **serviços** guia. No Windows Server Essentials, integração com o Office 365 é realizada no **Email** guia.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Integrar o servidor com o Office 365  
  
1.  Fazer logon no servidor como administrador e abra o painel do Windows Server Essentials.  
  
2.  Sobre o **Home** página, clique em **serviços** (no Windows Server Essentials, clique em **Email**), clique em **integrar com o Microsoft Office 365**e, em seguida, clique em **configurar a integração do Microsoft Office 365**.  
  
     Integre com o Assistente do Microsoft Office 365 é exibida.  
  
3.  Sobre o **começar** página, realizar uma das seguintes ações:  
  
    -   Se você don t tiver uma assinatura do Office 365, clique em **próxima**e siga as instruções para se inscrever para Office 365 ou entrar para cima para uma assinatura de avaliação.  
  
         Você precisará fazer logon Office 365 antes de retornar ao assistente. Mas você não precisa realizar qualquer uma das tarefas no **comece aqui** seção do portal do Office 365.  
  
    -   Se você já tiver uma assinatura do Office 365 que você deseja integrar com o servidor, selecione **eu já tenho uma assinatura do Office 365**e clique em **próxima**.  
  
4.  Siga as instruções para concluir o assistente.  
  
 Depois que o assistente for concluído com êxito, você ll observam as seguintes alterações para o painel:  
  
-   Daí s um novo **Office 365** página, que é usada para gerenciar a integração e sua assinatura do Office 365.  
  
-   Sobre o **usuários** página, você veja ll um **contas Online** guia onde você pode criar e gerenciar as contas de serviços Online da Microsoft que oferecem acesso a seus usuários para o Office 365. Se estiver usando o Exchange Online e tiver um servidor Windows Server Essentials, você ll também veja um **grupos de distribuição** guia.  
  
-   O **armazenamento** página em um servidor Windows Server Essentials tem um **SharePoint bibliotecas** guia para gerenciar bibliotecas do SharePoint Online e alteração de permissões para seus sites de equipe. Cada plano de negócios para o Office 365 inclui esses recursos básicos de SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a>Etapa 3: Vincular o nome de domínio da Internet de organização s para o Office 365 (opcional)  
 Se você quiser usar seu próprio domínio da Internet no email para sua organização e as URLs para seus recursos SharePoint Online, você pode vincular um domínio personalizado à sua assinatura do Office 365. Se você integrar seu servidor Windows Server Essentials com o Office 365, você pode fazer isso no painel.  
  
 Ele s mais fácil de fazer isso antes de criar online contas para os usuários para que você possa usar o domínio ao em massa-criar as contas online.  
  
 Você recebe um nome de domínio quando você se inscrever para o Office 365? por exemplo, *contoso*. onmicrosoft.com. Se você d em vez disso, use um nome de domínio diferentes? digamos, simplesmente contoso.com? você pode. Você precisará comprar um nome de domínio se você não t já próprios e alterar alguns dos registros de DNS.  
  
 Configurando um domínio personalizado para usar com o Office 365 envolve quatro tarefas:  
  
1.  **Compre um nome de domínio.** Ou seja, registrá-la com um registrador de domínio ou o provedor de hospedagem de DNS.  
  
    -   Escolha um nome de domínio que funciona com o Office 365. Você pode usar um nome de domínio de nível de 2º? por exemplo, buycontoso.com?, mas não é um nome de domínio de nível de 3ª? por exemplo, marketing.contoso.com. Para obter mais informações sobre como escolher um domínio para usar no Office 365, consulte [domínios](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
    -   Comprá-lo de um registrador de domínio que permite que os registros de servidor (DNS) de nome de domínio exigidos pelo Office 365. Para descobrir qual domínio registradores de permitir que os registros DNS necessários, consulte [como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se você já registrou seu domínio com um registrador diferente, não se preocupar t; Você pode transferir o domínio para um registrador diferente quando você vincula o domínio ao Office 365.  
  
2.  **Configure os registros DNS que permitem que os serviços do Office 365 usar o nome de domínio.** A maneira mais fácil é permitir que o Assistente para configurar os registros de DNS para você quando você vincula o domínio de sua assinatura do Office 365 na etapa 3. Se você d em vez disso, fazer isso por conta própria, consulte [registra como configurar manualmente o DNS para a integração do Office 365](#BKMK_ManuallyConfigureDNS).  
  
3.  **Vincule seu domínio personalizado de Internet para sua assinatura do Office 365.** Uso do ll o **vincular um domínio ao Office 365** ação.  
  
4.  **Verificar se os serviços do Office 365 estão usando o novo nome de domínio.**  
  
 Se você concluir as etapas 1 e 2 antes de usar o **vincular um domínio ao Office 365** tarefa, o assistente pode vincular o nome de domínio ao Office 365. Como alternativa, você pode ter um Assistente para ajudá-lo com algumas ou todas as etapas 1 e 2.  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>Para vincular seu domínio da Internet organização s para o Office 365  
  
1.  No painel, abra o **Office 365** página e clique em **vincular um domínio ao Office 365**.  
  
2.  Siga as instruções para concluir o assistente.  
  
     O assistente pode ajudá-lo com algumas ou todas as etapas para registrar, configurando e vinculando um nome de domínio de Internet novo ou existente para usar no Office 365.  
  
     Clique no link de ajuda na página do Assistente para obter as informações necessárias para concluir uma tarefa. Ou consulte Configurar uma seção de nome de domínio do [gerenciar o acesso à Web remoto no Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para uma visão geral do processo e os requisitos.  
  
    > [!NOTE]
    >  Para usar o Assistente para registrar um novo nome de domínio, você deve usar um dos provedores de serviços de nome de domínio que uma parceria com a Microsoft para fornecer perfeita integração com o assistente. Para encontrar um registrador de nome de domínio, consulte [como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Se o assistente detecta que não seja nome seu domínio gerenciado pelo servidor, ll você precisa configurar manualmente os registros DNS necessários para concluir a configuração. Para obter instruções, consulte [registra como configurar manualmente o DNS para a integração do Office 365](#BKMK_ManuallyConfigureDNS), posteriormente neste tópico.  
  
4.  Verifique se que o domínio está sendo usado no Office 365.  
  
     Há uma espera pouco depois que o assistente for concluído, enquanto o registrador de nome de domínio verifica os registros de DNS s. Isso acontece automaticamente. don t precisa fazer nada. Mas geralmente leva cerca de uma hora? e, às vezes, um pouco mais. Quando a verificação de domínio estiver concluída, o **Office 365** página listará seu domínio da organização s.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>Como configurar manualmente os registros DNS para integração com o Office 365  
 Se Link seu domínio ao Office 365 assistente detecta que o nome de domínio não for gerenciado pelo servidor, para concluir a configuração, você deve configurar manualmente os registros de servidor (DNS) de nomes de domínio necessários. Nesse caso, você ll encontrar uma lista de registros DNS que você deve configurar a **. txt %username%\NewDNSRecords_ (n)**, onde *(n)* é um número aleatório.  
  
 A tabela a seguir descreve os registros de DNS que você deve adicionar. Métodos de entrada podem variar com registradores de nome de domínio diferente. Se você tiver alguma dúvida, peça ajuda ao seu registrador de nome de domínio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Registros DNS necessários para vincular um nome de domínio Internet personalizado para o Office 365  
  
|Serviço|Registros DNS necessários|Finalidade|  
|-------------|--------------------------|-------------|  
|(Vários serviços)|MX| Office 365 usa esse registro para confirmar que você possui um nome de domínio específico. Esse registro MX não interfere na roteamento de mensagens de email.|  
|Exchange Online|MX|Fornece o roteamento de mensagens de email. **Importante:** se você estiver migrando email, não atribua uma preferência de zero (**0**) para o novo registro MX. Verifique se que o valor do registro é maior que o valor que é atribuído ao registro MX atual. Quando a migração de email for concluída e você estará pronto para alterar o servidor de email para o Office 365, ter o registrador de nome de domínio para redefinir o valor de preferência do novo registro MX.|  
|Exchange Online|Alias (CNAME)|Registro de descoberta automática que é usado para ajudar os usuários facilmente configurar uma conexão entre o Exchange Online e seus clientes da área de trabalho Outlook ou um cliente de email móvel. **Observação:** se você preferir acessar o Outlook Web Access usando seu nome de domínio própria organização s (por exemplo, http://mail.contoso.com) em vez da URL padrão (https://outlook.com/owa/office365.com), você pode configurar o registro de Alias (CName) da seguinte forma: **Type = CNAME, TTL = 01: 00:00, HostName = email, endereço = mail.office365.com**|  
|Exchange Online|TXT|Especifica que outlook.com, o domínio usado pelos servidores de email do Office 365, está autorizado para enviar email em nome de seu domínio. Crie esse registro para ajudar a impedir que seu email de saída sinalizado como spam.|  
|Lync Online|SRV|Ajuda a habilitar federação com outros serviços de mensagens instantâneas, como Windows Live ou Yahoo!.|  
|Lync Online|SRV|Registro de descoberta automática que é usado para ajudar os usuários facilmente configurar uma conexão entre o cliente de desktop do Lync e Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Depois de domínio verificação for concluída, não tente adicionar ou fazer outras alterações feitas nos registros DNS do portal do Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>Próxima etapa  
  
-   Crie contas de serviços Online da Microsoft para seus usuários.  
  
     Para usar serviços do Office 365, os usuários devem ter uma conta de serviços Online da Microsoft associada com sua conta de usuário de rede. O painel torna isso fácil. Se você re usando uma nova assinatura do Office 365, você pode em massa-criar contas online para suas contas de usuário existentes. Se você re integrando um novo servidor com uma assinatura do Office 365 que você já usa, você pode importar contas de usuário de existentes online contas. Para obter os procedimentos, consulte [gerenciar contas Online para que os usuários](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  No painel do Windows Server Essentials, contas de serviços Online da Microsoft são chamadas de contas do Office 365. As contas forem iguais. somente a terminologia alterada.  
  
##  <a name="BKMK_ManageIntegration"></a>Gerenciar a integração do Office 365  
 Depois de integrar seu servidor com o Office 365, o **Office 365** página no painel exibe informações sobre sua assinatura do Office 365 e disponibiliza essas tarefas:  
  
-   [Gerencie sua assinatura do Office 365](#BKMK_ManageO365) ? Altere a conta de administrador que você usa para gerenciar a assinatura. Abra o painel de administração do Office 365 para gerenciar sua assinatura.  
  
-   [Vincular seu domínio da Internet organização s para o Office 365](#BKMK_StepThree) ? Se você quiser ser capaz de enviar e receber emails endereçados para seu próprio domínio, você pode vincular o domínio ao Office 365. (Discutido anteriormente, [etapa 3: Vincular seu domínio da organização s para o Office 365](#BKMK_StepThree).)  
  
-   [Desabilitar a integração do Office 365](#BKMK_Disable) ? Se quiser que don t gerenciar seus serviços do Office 365, assinatura e contas online no painel, você pode desabilitar a integração do Office 365. Os serviços ainda estão disponíveis no portal do Office 365.  
  
###  <a name="BKMK_ManageO365"></a>Gerencie sua assinatura do Office 365  
 Se você precisar fazer alterações em sua assinatura do Office 365 enquanto você re funcionar no servidor, você pode abrir a assinatura no Office 365 a partir do **Office 365** página do painel. Você também pode alterar a conta de administrador que o servidor usa para fazer alterações em serviços do Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Para abrir sua assinatura no painel de administração do Office 365  
  
1.  No painel Windows Server Essentials, abra o **Office 365** página.  
  
2.  Em **tarefas de configuração**, clique em **gerenciar o Office 365**.  
  
3.  Faça logon no Office 365 com a conta online da Microsoft que você usa para gerenciar sua assinatura.  
  
     Abre o painel de administração do Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Para alterar a conta de administrador do Office 365  
  
1.  No painel, clique em **Office 365**.  
  
2.  Em **tarefas de configuração**, clique em **alterar a conta de administrador do Office 365**. O Assistente de conta de administrador alteração aparece. (No Windows Server Essentials, o assistente é nomeado configurar o Office 365 administrador conta).  
  
3.  Digite as credenciais da conta que você deseja usar para se conectar à sua assinatura do Office 365 e, em seguida, clique em **próxima**.  
  
4.  Clique em **fechar**. O painel é reiniciado.  
  
###  <a name="BKMK_Disable"></a>Desabilitar a integração do Office 365  
 Se você decidir que você não t deseja gerenciar suas contas online e serviços do Office 365 do painel, você pode desativar a integração do Office 365. Sua assinatura do Office 365 permanece ativa, e as alterações de configuração feitas pelo painel de ficar em vigor. Por exemplo, você ll receber emails endereçados para um nome de domínio que você vinculou à sua assinatura do Office 365. Você ganha t perder nenhum email, e os controles que você definiu para dispositivos móveis ainda são usados Exchange Online.  
  
 Futuramente, você gerenciará sua assinatura do Office 365, serviços e recursos no Office 365, e os usuários precisarão gerenciar as senhas de suas contas online no Office 365. Sincronização de senha não ocorre e desativar ou remover uma conta de usuário não terão efeito da conta online do usuário s.  
  
 Como o software de integração do Office 365 está instalado no servidor local, você pode desabilitar o recurso mesmo se o serviço de integração não consegue se conectar ao Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Desabilitar a integração do Office 365 com o servidor  
  
1.  No painel, clique em **Office 365**.  
  
2.  Clique em **desabilitar a integração do Office 365**. O Assistente de desabilitar de Office 365 aparece.  
  
3.  Siga as instruções para concluir o assistente.  
  
> [!NOTE]
>  Para habilitar novamente a integração do Office 365, use o **integrar com o Office 365** de tarefas no **serviço** guia do painel s **Home** página. Para obter instruções, consulte [etapa 2: integrar seu servidor Windows Server Essentials com o Microsoft Office 365](#BKMK_StepTwo), anteriormente neste tópico.  
  
##  <a name="BKMK_Troubleshoot"></a>Solucionar problemas de integração do Office 365  
 Esta seção fornece informações para ajudá-lo a solucionar problemas comuns que você pode encontrar ao usar os recursos de integração do Office 365 no Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a>Algumas contas de serviços Online da Microsoft não foram criadas  
 **Descrição**  
  
 Uma tentativa de criar uma ou mais contas de serviços Online da Microsoft de t, era o painel bem-sucedida.  
  
 **Solução**  
  
1.  Clique no link na página de conclusão do Assistente para abrir um arquivo de resultados que contém informações detalhadas sobre cada solicitação de criação de conta que não foi concluída com êxito. Por exemplo, um resultado pode informar que já existe uma conta da Microsoft Online Services com o nome de uma conta solicitada.  
  
2.  Execute as ações recomendadas para resolver cada erro.  
  
3.  Se este problema persistir, reiniciar o servidor e, em seguida, tente criar as contas online novamente.  
  
###  <a name="BKMK_ProblemUninstalling"></a>Ocorreu um problema desinstalando a integração do Office 365  
 **Descrição**  
  
 Ocorreu um erro desconhecido quando você tentava desabilitar a integração do Office 365.  
  
 **Solução**  
  
1.  Verifique se que o computador está conectado à Internet e tente novamente.  
  
2.  Se o erro ocorrer novamente, reiniciar o servidor e tente novamente.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Integração com visão geral dos serviços do Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Integração com visão geral dos serviços do Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guia de início rápido para usar o Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gerenciar Serviços Online da Microsoft](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
