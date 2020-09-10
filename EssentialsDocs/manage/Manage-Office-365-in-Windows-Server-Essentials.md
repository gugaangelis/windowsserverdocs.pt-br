---
title: Gerenciar Microsoft 365 no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 71e8204c8b11f423871f890badffdfb96206feb8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623059"
---
# <a name="manage-microsoft-365-in-windows-server-essentials"></a>Gerenciar Microsoft 365 no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ao integrar seu servidor do Windows Server Essentials com o Microsoft 365, você pode gerenciar seus serviços de Microsoft 365 e contas online junto com seus recursos locais no painel do Windows Server Essentials. Neste tópico, você descobrirá o que pode obter integrando seu servidor com o Microsoft 365, como fazer isso e como gerenciar e solucionar problemas de integração de seu Microsoft 365.



> [!IMPORTANT]
>   Só há suporte para a integração Microsoft 365 em um único ambiente de controlador de domínio. Além disso, o assistente de integração do Microsoft 365 deve ser executado em um controlador de domínio.

## <a name="in-this-topic"></a>Neste tópico

-   [Por que devo integrar Microsoft 365 com meu servidor?](#BKMK_IntegrationOverview)

-   [Configurar a integração do Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)

-   [Gerenciar integração de Microsoft 365](#BKMK_ManageIntegration)

-   [Solucionar problemas de integração do Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)

##  <a name="why-should-i-integrate-microsoft-365-with-my-server"></a><a name="BKMK_IntegrationOverview"></a> Por que devo integrar Microsoft 365 com meu servidor?
 Há muitos bons motivos para integrar o Microsoft 365 ao servidor do Windows Server Essentials. Se você gerenciar alguns de seus recursos internamente, mas usar Microsoft 365 para outros serviços, poderá gerenciar seus serviços de Microsoft 365 e recursos no painel, juntamente com seus recursos locais, em vez de trabalhar em dois lugares.

- Gerencie as contas online que dão aos usuários acesso a Microsoft 365 junto com suas contas de usuário:

  -   Criar contas de serviços Online da Microsoft para os usuários em uma única etapa, ou criar contas de usuário no servidor para suas contas online existentes. Você também pode adicionar uma conta online para uma conta de usuário novo ou existente.

       Quando você cria as contas online no painel, os usuários entram no Microsoft 365 com a mesma senha que usam no servidor. Se eles alterarem a senha da conta de usuário, a senha online também será alterada. E você sabe que as senhas de conta online sempre atendem aos requisitos de segurança definidos para suas contas de usuário.

  -   Gerencie uma conta online com a conta de usuário em todo o ciclo de vida da conta de usuário. Se você desativar uma conta de usuário, a conta online será desativada também. Se você remover uma conta de usuário, a conta online também será removida.

  -   Em um servidor do Windows Server Essentials, gerencie também grupos de distribuição do Exchange Online para email.

- Envie e receba emails do domínio da Internet da sua organização (por exemplo, contoso.com) vinculando um domínio de Internet personalizado à sua assinatura do Microsoft 365.

- Gerencie sua assinatura e Microsoft 365 integração a partir do painel.

- Se sua assinatura inclui bibliotecas do SharePoint Online, Microsoft 365 integração com um servidor do Windows Server Essentials permite que você:

  - Crie e gerencie suas bibliotecas do SharePoint Online no painel.

    > [!NOTE]
    >  Você também poderá usar o aplicativo meu servidor 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou Windows Phone. Este recurso só está disponível para o Windows Server Essentials. Para obter mais informações, consulte [usar o aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).

  - Altere as permissões para um site de equipe do SharePoint Online do painel ou abra o site de equipe do painel para fazer outras alterações.

    Para obter mais informações, consulte [gerenciar o SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).

- Se você assinar o Exchange Online, Microsoft 365 integração com um servidor do Windows Server Essentials permite que você gerencie os dispositivos móveis que os usuários usam para se conectar ao servidor de email da empresa:

  -   Requer proteção por senha quando os dispositivos móveis se conectam ao servidor de email da empresa. Defina um tamanho mínimo da senha, o número permitido de tentativas de logon com falha e o tempo mínimo necessário entre as tentativas de logon.

  -   Bloqueie um dispositivo móvel de se conectar ao Exchange Online se você souber de problemas de segurança com o modelo do dispositivo.

  -   Se um dispositivo móvel for perdido ou roubado, limpe o dispositivo para excluir quaisquer dados confidenciais da empresa na próxima vez que o dispositivo for ativado.

- Microsoft 365 integração oferece novas maneiras de se conectar a recursos e serviços do Microsoft 365:

  -   Abra Microsoft 365 Services no Launchpad do Windows Server Essentials. Para obter informações, consulte [Guia de início rápido usar Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).

  -   Use o aplicativo meu servidor 2012 R2 para trabalhar com documentos em suas bibliotecas do SharePoint Online de seu laptop, dispositivo móvel ou Windows Phone. Para obter informações, consulte [usar o aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esse recurso só está disponível no Windows Server Essentials.

##  <a name="set-up-microsoft-365-integration"></a><a name="BKMK_Configure"></a> Configurar a integração do Microsoft 365
 Você pode integrar seu servidor com o Microsoft 365 a qualquer momento depois de concluir a instalação do servidor. Se você ainda não tiver uma assinatura Microsoft 365, poderá comprar uma ou inscrever-se para uma assinatura de avaliação gratuita.

 Você fará as seguintes tarefas:

-   [Etapa 1: verificar os requisitos de integração de Microsoft 365](#BKMK_StepOne_VERIFY)

-   [Etapa 2: integrar o servidor com o Microsoft 365](#BKMK_StepTwo)

-   [Etapa 3: vincular o nome de domínio da Internet da sua organização ao Microsoft 365 (opcional)](#BKMK_StepThree)

###  <a name="step-1-verify-microsoft-365-integration-requirements"></a><a name="BKMK_StepOne_VERIFY"></a> Etapa 1: verificar os requisitos de integração de Microsoft 365
 Antes de começar, verifique se que o servidor atende aos seguintes requisitos:

-   O servidor pode ter qualquer um destes sistemas operacionais: Windows Server Essentials, Windows Server Essentials ou o sistema operacional Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.

-   O ambiente só pode ter um controlador de domínio e você deve executar Microsoft 365 integração no controlador de domínio.

-   Você deve ser capaz de se conectar à Internet do servidor.

-   Você deve instalar todas as atualizações críticas e importantes no servidor antes de iniciar Microsoft 365 integração.

-   Se você quiser usar o domínio de Internet de sua organização em endereços de email e com seus recursos do SharePoint Online, será necessário registrar o nome de domínio para que você possa vincular o domínio a Microsoft 365 durante a integração. Para obter mais informações, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).

> [!NOTE]
>  Você não precisa assinar Microsoft 365 com antecedência. Você poderá comprar uma assinatura ou inscrever-se para uma avaliação gratuita durante a integração de Microsoft 365. Se você quiser dar uma olhada em planos e preços para Microsoft 365, [compare Microsoft 365 planos para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).

###  <a name="step-2-integrate-the-server-with-microsoft-365"></a><a name="BKMK_StepTwo"></a> Etapa 2: integrar o servidor com o Microsoft 365
 Execute o procedimento a seguir no controlador de domínio para integrar o servidor do Windows Server Essentials com o Microsoft 365.

> [!NOTE]
>  O procedimento é o mesmo se você tiver o Windows Server Essentials ou o Windows Server Essentials, mas começará em um local diferente na Home Page. No Windows Server Essentials, você integra o servidor com Microsoft 365 e outros serviços online da Microsoft na guia **Serviços** . No Windows Server Essentials, Microsoft 365 integração é executada na guia **email** .

##### <a name="to-integrate-the-server-with-microsoft-365"></a>Para integrar o servidor com o Microsoft 365

1. Entre no servidor como administrador e abra o painel do Windows Server Essentials.

2. Na **Home** Page do, clique em **Serviços** (no Windows Server Essentials, clique em **email**), clique em **integrar com Microsoft 365**e, em seguida, clique em **Configurar a integração Microsoft 365**.

    O assistente para integrar com Microsoft 365 é exibido.

3. Na página **Começar**, execute uma das seguintes ações:

   -   Se você não tiver uma assinatura para Microsoft 365, clique em **Avançar**e siga as instruções para assinar Microsoft 365 ou inscrever-se para uma assinatura de avaliação.

        Você precisará entrar no Microsoft 365 antes de retornar ao assistente. Mas você não precisa executar nenhuma das tarefas na seção **Iniciar aqui** do portal de Microsoft 365.

   -   Se você já tiver uma assinatura para Microsoft 365 que deseja integrar ao servidor, selecione **já tenho uma assinatura para Microsoft 365**e clique em **Avançar**.

4. Siga as instruções para concluir o assistente.

   Depois que o assistente for concluído com êxito, você observará as seguintes alterações no painel:

-   Há uma nova página **Microsoft 365** , que é usada para gerenciar a integração e sua assinatura do Microsoft 365.

-   Na página **usuários** , você verá uma guia **contas online** , na qual poderá criar e gerenciar as contas do Microsoft Online Services que concedem aos usuários acesso ao Microsoft 365. Se você estiver usando o Exchange Online e tiver um servidor do Windows Server Essentials, você também verá uma guia **grupos de distribuição** .

-   A página **armazenamento** em um servidor do Windows Server Essentials tem uma guia **bibliotecas do SharePoint** para gerenciar bibliotecas do SharePoint Online e alterar permissões para seus sites de equipe. Cada plano de negócios para o Microsoft 365 inclui esses recursos básicos do SharePoint Online.

###  <a name="step-3-link-your-organizations-internet-domain-name-to-microsoft-365-optional"></a><a name="BKMK_StepThree"></a> Etapa 3: vincular o nome de domínio da Internet da sua organização ao Microsoft 365 (opcional)
 Se você quiser usar seu próprio domínio de Internet em email endereçado à sua organização e às URLs para seus recursos do SharePoint Online, você poderá vincular um domínio personalizado à sua assinatura do Microsoft 365. Se você integrar seu servidor do Windows Server Essentials com o Microsoft 365, poderá fazer isso no painel.

 É mais fácil fazer isso antes de criar contas online para seus usuários para que você possa usar o domínio ao criar contas online em massa.

 Você Obtém um nome de domínio ao se inscrever no Microsoft 365? por exemplo, *contoso*. onmicrosoft.com. Se você preferir usar um nome de domínio diferente? digamos, simplesmente contoso.com? você pode. Você precisará comprar um nome de domínio se ainda não tiver um e alterar alguns dos registros DNS.

 Configurar um domínio personalizado para usar com o Microsoft 365 envolve quatro tarefas:

1. **Comprar um nome de domínio.** Ou seja, registrá-lo com um registrador de domínio ou o provedor de hospedagem de DNS.

   -   Escolha um nome de domínio que funcione com Microsoft 365. Você pode usar um nome de domínio de segundo nível? por exemplo, buycontoso.com?, mas não um nome de domínio de terceiro nível? por exemplo, marketing.contoso.com. Para obter mais informações sobre como escolher um domínio para usar em Microsoft 365, consulte [domínios](/office365/servicedescriptions/office-365-platform-service-description/domains).

   -   Comprá-lo de um registrador de domínio que permite os registros DNS (servidor de nomes de domínio) exigidos pelo Microsoft 365. Para saber quais registradores de domínio permitem os registros DNS exigidos, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Se você já registrou seu domínio com um registrador diferente, não se preocupe; Você pode transferir o domínio para um registrador diferente ao vincular o domínio a Microsoft 365.

2. **Configure os registros DNS que permitem que Microsoft 365 serviços usem o nome de domínio.** A maneira mais fácil é permitir que o assistente Configure os registros DNS para você ao vincular o domínio à sua assinatura do Microsoft 365 na etapa 3. Se você preferir, consulte [como configurar manualmente os registros DNS para a integração de Microsoft 365](#BKMK_ManuallyConfigureDNS).

3. **Vincule seu domínio de Internet personalizado à sua assinatura do Microsoft 365.** Você usará a ação **vincular um domínio ao Microsoft 365** .

4. **Verifique se os serviços de Microsoft 365 estão usando o novo nome de domínio.**

   Se você concluir as etapas 1 e 2 antes de usar o **link um domínio para Microsoft 365** tarefa, o assistente poderá vincular o nome de domínio a Microsoft 365. Como alternativa, você pode ter um assistente para ajudá-lo com algumas ou todas as etapas 1 e 2.

##### <a name="to-link-your-organizations-internet-domain-to-microsoft-365"></a>Para vincular o domínio de Internet da sua organização ao Microsoft 365

1.  No painel, abra a página **Microsoft 365** e clique em **vincular um domínio a Microsoft 365**.

2.  Siga as instruções para concluir o assistente.

     O assistente pode ajudá-lo com algumas ou todas as etapas para registrar, configurar e vincular um nome de domínio da Internet novo ou existente para uso no Microsoft 365.

     Clique no link de ajuda na página do assistente para obter as informações necessárias para concluir uma tarefa. Ou consulte a seção configurar um nome de domínio de [gerenciar acesso via Web remoto no Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para obter uma visão geral e os requisitos do processo.

    > [!NOTE]
    >  Para usar o assistente para registrar um novo nome de domínio, você deve usar um dos provedores de serviços de nome de domínio que têm parcerias com a Microsoft para fornecer integração com o assistente. Para localizar um registrador de nomes de domínio, consulte [Como comprar um nome de domínio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).

3.  Se o assistente detectar que o nome de domínio não é gerenciado pelo servidor, você precisará configurar manualmente os registros DNS necessários para concluir a configuração. Para obter instruções, consulte [como configurar manualmente os registros DNS para integração de Microsoft 365](#BKMK_ManuallyConfigureDNS), mais adiante neste tópico.

4.  Verifique se o domínio está sendo usado no Microsoft 365.

     Há um pouco de espera após a conclusão do assistente, enquanto o registrador de nome de domínio verifica os registros DNS. Isso ocorre automaticamente; Você não precisa fazer nada. Mas geralmente leva cerca de uma hora? e, às vezes, um pouco mais. Quando a verificação de domínio for concluída, a página **Microsoft 365** listará o domínio da sua organização.

####  <a name="how-to-manually-configure-dns-records-for-microsoft-365-integration"></a><a name="BKMK_ManuallyConfigureDNS"></a> Como configurar manualmente os registros DNS para integração de Microsoft 365
 Se o link vincular seu domínio ao Microsoft 365 o assistente detectar que o nome de domínio não é gerenciado pelo servidor, para concluir a configuração, você deverá configurar manualmente os registros DNS (servidor de nomes de domínio) necessários. Nesse caso, você encontrará uma lista de registros DNS que deve configurar em **% username% \ NewDNSRecords_ (n). txt**, em que *(n)* é um número aleatório.

 A tabela a seguir descreve os registros DNS que você deve adicionar. Os métodos de entrada podem variar de acordo com registradores de nome de domínio diferentes. Se tiver alguma dúvida, peça ajuda ao registrador de nome de domínio.

### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-microsoft-365"></a>Registros DNS necessários para vincular um nome de domínio da Internet personalizado a Microsoft 365

|Serviço|Registros DNS necessários|Finalidade|
|-------------|--------------------------|-------------|
|(Vários serviços)|MX| Microsoft 365 usa esse registro para verificar se você possui um nome de domínio específico. Esse registro MX não interfere com o roteamento de mensagens de email.|
|Exchange Online|MX|Oferece roteamento de mensagens de email. **Importante:**  Se você estiver migrando o email, não atribua uma preferência de zero (**0**) ao novo registro MX. Verifique se que o valor do registro é maior que o valor que é atribuído ao registro MX atual. Quando a migração de email for concluída e você estiver pronto para alterar o servidor de email para Microsoft 365, faça com que seu registrador de nome de domínio redefina o valor de preferência do novo registro MX.|
|Exchange Online|Alias (CNAME)|O registro de descoberta automática é usado para ajudar os usuários a estabelecer facilmente uma conexão entre o Exchange Online e seus clientes de área de trabalho do Outlook ou um cliente de email móvel. **Observação:**  Se você preferir acessar o Outlook Acesso via Web usando o próprio nome de domínio da sua organização (por exemplo, http://mail.contoso.com) em vez da URL padrão ( https://outlook.com/owa/office365.com) , você pode configurar o registro de alias (CNAME) da seguinte maneira: **tipo = CNAME, TTL = 01:00:00, hostname = mail, endereço = mail. office365. com**|
|Exchange Online|TXT|Especifica que outlook.com, o domínio usado pelo Microsoft 365 servidores de email, está autorizado a enviar email em nome do seu domínio. Crie este registro para ajudar a impedir que o email de saída seja marcado como spam.|
|Lync Online|SRV|Ajuda a habilitar a federação com outros serviços de mensagens instantâneas como Windows Live ou Yahoo!.|
|Lync Online|SRV|Descobre automaticamente o registro que é usado para ajudar os usuários a configurarem facilmente uma conexão entre o cliente de área de trabalho do Lync e o Microsoft Lync Online.|

> [!IMPORTANT]
>  Após a conclusão da verificação de domínio, não tente adicionar ou fazer nenhuma alteração adicional nos registros DNS do portal de Microsoft 365.

###  <a name="next-step"></a><a name="BKMK_StepFour_ACCOUNTS"></a> Próxima etapa

-   Criar contas de serviços Online da Microsoft para os usuários.

     Para usar os serviços de Microsoft 365, os usuários devem ter uma conta do Microsoft Online Services associada à sua conta de usuário de rede. O Painel torna isso fácil. Se você estiver usando uma nova assinatura Microsoft 365, poderá criar contas online em massa para suas contas de usuário existentes. Se estiver integrando um novo servidor com uma assinatura Microsoft 365 que você já está usando, você poderá importar contas de usuário de suas contas online existentes. Para procedimentos, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).

> [!NOTE]
>  No painel do Windows Server Essentials, as contas do Microsoft Online Services são chamadas de contas de Microsoft 365. As contas são iguais, somente a terminologia foi alterada.

##  <a name="manage-microsoft-365-integration"></a><a name="BKMK_ManageIntegration"></a> Gerenciar integração de Microsoft 365
 Depois de integrar seu servidor com o Microsoft 365, a página **Microsoft 365** no painel exibe informações sobre sua assinatura do Microsoft 365 e disponibiliza essas tarefas:

-   [Gerenciar sua assinatura do Microsoft 365](#BKMK_ManageO365) ? Altere a conta de administrador que você usa para gerenciar a assinatura. Abra o painel de administração do Microsoft 365 para gerenciar sua assinatura.

-   [Vincular o domínio de Internet da sua organização a Microsoft 365](#BKMK_StepThree) ? Se você quiser ser capaz de enviar e receber emails endereçados ao seu próprio domínio, você pode vincular o domínio a Microsoft 365. (Discutido anteriormente, na [etapa 3: vincular o domínio da sua organização a Microsoft 365](#BKMK_StepThree).)

-   [Desabilitar a integração de Microsoft 365](#BKMK_Disable) ? Se você não quiser gerenciar seus serviços Microsoft 365, assinatura e contas online no painel, poderá desabilitar Microsoft 365 integração. Os serviços ainda estão disponíveis no portal de Microsoft 365.

###  <a name="manage-your-microsoft-365-subscription"></a><a name="BKMK_ManageO365"></a> Gerenciar sua assinatura do Microsoft 365
 Se você precisar fazer alterações em sua assinatura do Microsoft 365 enquanto estiver trabalhando no servidor, poderá abrir a assinatura no Microsoft 365 na página **Microsoft 365** do painel. Você também pode alterar a conta de administrador que o servidor usa para fazer alterações nos serviços de Microsoft 365.

##### <a name="to-open-your-subscription-on-the-microsoft-365-admin-dashboard"></a>Para abrir sua assinatura no painel de administração do Microsoft 365

1.  No painel do Windows Server Essentials, abra a página **Microsoft 365** .

2.  Em **tarefas de configuração**, clique em **gerenciar Microsoft 365**.

3.  Entre no Microsoft 365 com a conta online da Microsoft que você usa para gerenciar sua assinatura.

     O painel de administração do Microsoft 365 é aberto.

##### <a name="to-change-the-microsoft-365-administrator-account"></a>Para alterar a conta de administrador do Microsoft 365

1.  No painel, clique em **Microsoft 365**.

2.  Em **tarefas de configuração**, clique em **alterar a conta de administrador do Microsoft 365**. O Assistente de Alteração da Conta de Administrador é exibido. (No Windows Server Essentials, o assistente é chamado configurar a conta de administrador do Microsoft 365.)

3.  Digite as credenciais da conta que você deseja usar para se conectar à sua assinatura do Microsoft 365 e clique em **Avançar**.

4.  Clique em **Fechar**. O Painel é reiniciado.

###  <a name="disable-microsoft-365-integration"></a><a name="BKMK_Disable"></a> Desabilitar integração de Microsoft 365
 Se você decidir que não deseja gerenciar seus serviços de Microsoft 365 e contas online no painel, você pode desabilitar a integração de Microsoft 365. Sua assinatura do Microsoft 365 permanece ativa e as alterações de configuração feitas a partir do painel permanecem em vigor. Por exemplo, você receberá email endereçado a um nome de domínio que você vinculou à sua assinatura do Microsoft 365. Você não perderá nenhum email e os controles que você definir para dispositivos móveis ainda serão usados no Exchange Online.

 No futuro, você gerenciará seus Microsoft 365 assinatura, serviços e recursos no Microsoft 365, e os usuários precisarão gerenciar as senhas de suas contas online no Microsoft 365. A sincronização de senha não ocorre mais, e desabilitar ou remover uma conta de usuário não terá nenhum efeito na conta online do usuário.

 Como o software de integração do Microsoft 365 está instalado no servidor local, você pode desabilitar o recurso mesmo que o serviço de integração não possa se conectar ao Microsoft 365.

##### <a name="to-disable-microsoft-365-integration-with-the-server"></a>Para desabilitar a integração de Microsoft 365 com o servidor

1.  No painel, clique em **Microsoft 365**.

2.  Clique em **desabilitar integração de Microsoft 365**. O assistente para desabilitar Microsoft 365 é exibido.

3.  Siga as instruções para concluir o assistente.

> [!NOTE]
>  Para habilitar a integração do Microsoft 365 novamente, use a tarefa **integrar com Microsoft 365** na guia **serviço** da página **inicial** do painel. Para obter instruções, consulte [etapa 2: integrar seu servidor do Windows Server Essentials com o Microsoft 365](#BKMK_StepTwo), anteriormente neste tópico.

##  <a name="troubleshoot-microsoft-365-integration"></a><a name="BKMK_Troubleshoot"></a> Solucionar problemas de integração do Microsoft 365
 Esta seção fornece informações para ajudá-lo a solucionar problemas comuns que você pode encontrar ao usar os recursos de integração do Microsoft 365 no Windows Server Essentials.

###  <a name="some-microsoft-online-services-accounts-were-not-created"></a><a name="BKMK_AcctsNotCreated"></a> Algumas contas do Microsoft Online Services não foram criadas
 **Descrição**

 Uma tentativa de criar uma ou mais contas do Microsoft Online Services a partir do painel não foi bem-sucedida.

 **Solução**

1.  Clique no link na página de conclusão do assistente para abrir um arquivo de resultados que contém informações detalhadas sobre cada solicitação de criação de conta não foi concluída com êxito. Por exemplo, um resultado pode informar que já existe uma conta de serviços Online da Microsoft com o nome de uma conta solicitada.

2.  Execute as ações recomendadas para resolver cada erro.

3.  Se o problema persistir, reinicie o servidor e, em seguida, tente criar contas online novamente.

###  <a name="there-was-a-problem-uninstalling-microsoft-365-integration"></a><a name="BKMK_ProblemUninstalling"></a> Houve um problema ao desinstalar a integração de Microsoft 365
 **Descrição**

 Ocorreu um erro desconhecido ao tentar desabilitar a integração de Microsoft 365.

 **Solução**

1.  Verifique se o computador está conectado à Internet e tente novamente.

2.  Se o erro ocorrer novamente, reinicie o servidor e tente novamente.

## <a name="additional-references"></a>Referências adicionais

-   [Visão geral da integração de serviços para o Windows Server Essentials-parte 1](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-1)

-   [Visão geral da integração de serviços para o Windows Server Essentials-parte 2](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-2)

-   [Guia de Início Rápido usar Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)

-   [Gerenciar serviços online da Microsoft](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)