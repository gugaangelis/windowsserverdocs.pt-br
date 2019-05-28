---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Políticas de controle de acesso no AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c690f81620f97622a2f068b07c36e0a6c59e90d4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190344"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Políticas de controle de acesso no AD FS para Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modelos de política de controle de acesso no AD FS  
Serviços de Federação do Active Directory agora oferece suporte ao uso de modelos de política de controle de acesso.  Usando modelos de política de controle de acesso, um administrador pode impor configurações de política, atribuindo o modelo de política a um grupo de terceiras partes confiáveis (RPs). Administrador também pode fazer atualizações para o modelo de política e as alterações serão aplicadas para partes confiáveis automaticamente se não houver nenhuma interação do usuário necessária.  
  
## <a name="what-are-access-control-policy-templates"></a>Quais são os modelos de política de controle de acesso?  
O pipeline de núcleo do AD FS para o processamento de diretiva tem três fases: autenticação, autorização e a declaração de emissão. No momento, os administradores do AD FS precisam configurar uma política para cada uma dessas fases separadamente.  Isso também envolve a compreensão das implicações dessas políticas e se essas políticas têm dependência entre. Além disso, os administradores precisam compreender as declaração regra idioma e autor regras personalizadas para habilitar alguns política simples/comum (por exemplo bloquear o acesso externo).  
  
Esse modelo antigo em que os administradores precisam configurar regras de autorização de emissão usando qual política de controle de acesso modelos fazer é substituir declarações da linguagem.  Os cmdlets do PowerShell antigos de regras de autorização de emissão ainda se aplicam, mas ele é mutuamente exclusivo o novo modelo. Os administradores podem optar por usar o novo modelo ou o modelo antigo.  O novo modelo permite aos administradores controlar quando conceder acesso, incluindo a imposição de autenticação multifator.  
  
Modelos de política de controle de acesso usam um modelo de permissão.  Isso significa, por padrão, que ninguém tenha acesso e que o acesso deve ser concedido explicitamente.  No entanto, isso não é apenas um todos ou permitir que nada.  Os administradores podem adicionar exceções à regra de permissão.  Por exemplo, um administrador poderá conceder acesso com base em uma rede específica selecionando essa opção e especificando o intervalo de endereços IP.  Mas o administrador pode adicionar e exceção, por exemplo, o administrador pode adicionar uma exceção de uma rede específica e especificar esse intervalo de endereços IP.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Acesso interno controle modelos vs acesso personalizado controle política modelos de política  
O AD FS inclui vários modelos de política de controle de acesso interno.  Estes destino alguns cenários comuns que têm o mesmo conjunto de requisitos de política, por exemplo política de acesso de cliente para o Office 365.  Esses modelos não podem ser modificados.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para fornecer maior flexibilidade para atender às necessidades de negócios, os administradores podem criar seus próprios acessos a modelos de política.  Elas podem ser modificadas após a criação e alterações no modelo de política personalizada serão aplicadas a todas as RPs que são controladas por esses modelos de política.  Para adicionar um modelo de política personalizada simplesmente clique adicionar política de controle de acesso de dentro do gerenciamento do AD FS.  
  
Para criar um modelo de política, um administrador precisa primeiro especificar condições sob as quais uma solicitação será autorizada para emissão de token e/ou delegação. Opções de condição e ação são mostradas na tabela a seguir.   Condições em negrito podem ser ainda mais configuradas pelo administrador com valores diferentes ou novos. Administrador também pode especificar as exceções se houver algum. Quando uma condição for atendida, uma ação de permissão não será disparada se não houver uma exceção especificada e a solicitação de entrada corresponder à condição especificada na exceção.  
  
|Permitir que os usuários|Exceto| 
| --- | --- | 
 |Partir **específico** rede|Partir **específico** rede<br /><br />Partir **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com o **específico** declarações na solicitação|  
|Partir **específico** grupos|Partir **específico** rede<br /><br />Partir **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com o **específico** declarações na solicitação|  
|Em dispositivos com **específico** níveis de confiança|Partir **específico** rede<br /><br />Partir **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com o **específico** declarações na solicitação|  
|Com o **específico** declarações na solicitação|Partir **específico** rede<br /><br />Partir **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com o **específico** declarações na solicitação|  
|E exigir a autenticação multifator|Partir **específico** rede<br /><br />Partir **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com o **específico** declarações na solicitação|  
  
Se o administrador seleciona várias condições, elas são de **AND** relação. Ações são mutuamente exclusivas e para a regra de uma política, você só pode escolher uma ação. Se o administrador seleciona várias exceções, elas são de um **ou** relação. Alguns exemplos de regra de política são mostrados abaixo:  
  
|**Política**|**Regras de política**|
| --- | --- |  
|Acesso à extranet requer MFA<br /><br />Todos os usuários são permitidos|**Regra #1**<br /><br />de **extranet**<br /><br />e com o MFA<br /><br />Permissão<br /><br />**Rule#2**<br /><br />de **intranet**<br /><br />Permissão|  
|Acesso externo não são permitidas, exceto não FTE<br /><br />Acesso à intranet para FTE em dispositivos ingressados no local são permitidos|**Regra #1**<br /><br />de **extranet**<br /><br />bidirecionalmente **não FTE** grupo<br /><br />Permissão<br /><br />**Regra #2**<br /><br />de **intranet**<br /><br />bidirecionalmente **ingresso** dispositivo<br /><br />bidirecionalmente **FTE** grupo<br /><br />Permissão|  
|Acesso à extranet requer a MFA, exceto o "administrador de serviço"<br /><br />Todos os usuários têm permissão para acessar|**Regra #1**<br /><br />de **extranet**<br /><br />e com o MFA<br /><br />Permissão<br /><br />Exceto **grupo de administradores de serviço**<br /><br />**Regra #2**<br /><br />Sempre<br /><br />Permissão|  
|dispositivo ingressado no local de trabalho, acessando da extranet requer MFA<br /><br />Permitir que a malha do AD para acesso extranet e intranet|**Regra #1**<br /><br />de **intranet**<br /><br />Bidirecionalmente **AD Fabric** grupo<br /><br />Permissão<br /><br />**Regra #2**<br /><br />de **extranet**<br /><br />bidirecionalmente **não-ingresso** dispositivo<br /><br />Bidirecionalmente **AD Fabric** grupo<br /><br />e com o MFA<br /><br />Permissão<br /><br />**Regra #3**<br /><br />de **extranet**<br /><br />bidirecionalmente **ingresso** dispositivo<br /><br />Bidirecionalmente **AD Fabric** grupo<br /><br />Permissão|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modelo de política sem parâmetros de vs de modelo de política com parâmetros  
As políticas de controle de acesso podem ser  
  
Um modelo de política com parâmetros é um modelo de política que tem parâmetros. Um administrador precisa inserir o valor para esses parâmetros ao atribuir a esse modelo ao RPs.An administrador não pode fazer alterações ao modelo de política com parâmetros após ele ter sido criado.  Um exemplo de uma política com parâmetros é a política interna, o grupo de permissão específico.  Sempre que essa política é aplicada a uma RP, esse parâmetro precisa ser especificado.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Um modelo de política sem parâmetros é um modelo de política que não tem parâmetros. Um administrador pode atribuir esse modelo para o RPs sem qualquer entrada necessária e pode fazer alterações para um modelo de política sem parâmetros após ele ter sido criado.  Um exemplo disso é a política interna, permitir todos e exigir a MFA.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso sem parâmetros  
Para criar um acesso sem parâmetros, política de controle de usar o procedimento a seguir  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso sem parâmetros  
  
1.  De gerenciamento do AD FS à esquerda selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo:   Permitir que usuários com dispositivos autenticados.  
  
3.  Sob **permitir o acesso se qualquer uma das regras a seguir forem atendidas**, clique em **Add**.  
  
4.  Em permitir, marque a caixa próximo a **de dispositivos com o nível de confiança específico**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela que pops-up, selecione **autenticado** na lista suspensa.  Clique em **Ok**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Clique em **Ok**. Clique em **Ok**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso com parâmetros  
Para criar um controle de acesso com parâmetros política usar o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso com parâmetros  
  
1.  De gerenciamento do AD FS à esquerda selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo:  Permitir que usuários com uma declaração específica.  
  
3.  Sob **permitir o acesso se qualquer uma das regras a seguir forem atendidas**, clique em **Add**.  
  
4.  Em permitir, marque a caixa próximo a **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela que pops-up, selecione **parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **Ok**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Clique em **Ok**. Clique em **Ok**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Como criar uma política de controle de acesso personalizado com uma exceção  
Para criar um controle de acesso a política com uma exceção use o procedimento a seguir.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para criar uma política de controle de acesso personalizado com uma exceção  
  
1.  De gerenciamento do AD FS à esquerda selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo:   Permitir que os usuários com dispositivos de autenticado, mas não gerenciado.  
  
3.  Sob **permitir o acesso se qualquer uma das regras a seguir forem atendidas**, clique em **Add**.  
  
4.  Em permitir, marque a caixa próximo a **de dispositivos com o nível de confiança específico**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela que pops-up, selecione **autenticado** na lista suspensa.  Clique em **Ok**.  
  
7.  Exceto em, marque a caixa ao lado **de dispositivos com o nível de confiança específico**  
  
8.  Na parte inferior, exceto em, selecione o sublinhado **específico**  
  
9. Na janela que pops-up, selecione **gerenciados** na lista suspensa.  Clique em **Ok**.  
  
10. Clique em **Ok**. Clique em **Ok**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Como criar uma política de controle de acesso personalizada com várias condições de permissão  
Para criar uma política de controle de acesso com permissão de várias condições de usam o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso com parâmetros  
  
1.  De gerenciamento do AD FS à esquerda selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo:   Permitir que usuários com uma declaração específica e de grupo específico.  
  
3.  Sob **permitir o acesso se qualquer uma das regras a seguir forem atendidas**, clique em **Add**.  
  
4.  Em permitir, marque a caixa de lado **de um grupo específico** e **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o sublinhado **específico** para a primeira condição, ao lado de grupos  
  
6.  Na janela que pops-up, selecione **parâmetro especificado quando a política é atribuída**.  Clique em **Ok**.  
  
7.  Na parte inferior, selecione o sublinhado **específico** para a segunda condição, ao lado de declarações  
  
8.  Na janela que pops-up, selecione **parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **Ok**.  
  
9. Clique em **Ok**. Clique em **Ok**.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Como atribuir uma política de controle de acesso a um novo aplicativo  
Atribuir uma política de controle de acesso para um novo aplicativo é bastante direto e agora foi integrado ao Assistente para adicionar uma RP.  Do Assistente de confiança de terceira parte confiável terceiros, você pode selecionar a política de controle de acesso que você deseja atribuir.  Este é um requisito ao criar uma novo terceira parte confiável.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Como atribuir uma política de controle de acesso a um aplicativo existente  
Atribuir uma política de controle de acesso a um aplicativo existente simplesmente selecione o aplicativo de terceira e na direita, clique em **Editar política de controle de acesso**.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
A partir daqui, você pode selecionar a política de controle de acesso e aplicá-la ao aplicativo.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

