---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Políticas de controle de acesso no AD FS Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8c28d24484adf73b1a769a02b817ba48f591f194
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863823"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Políticas de controle de acesso no AD FS para Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modelos de política de controle de acesso no AD FS  
O Serviços de Federação do Active Directory (AD FS) agora dá suporte ao uso de modelos de política de controle de acesso.  Ao usar modelos de política de controle de acesso, um administrador pode impor as configurações de política atribuindo o modelo de política a um grupo de partes confiáveis (RPs). O administrador também pode fazer atualizações no modelo de política e as alterações serão aplicadas automaticamente às terceiras partes confiáveis se não houver nenhuma interação do usuário necessária.  
  
## <a name="what-are-access-control-policy-templates"></a>O que são modelos de política de controle de acesso?  
O pipeline AD FS Core para processamento de política tem três fases: autenticação, autorização e emissão de declaração. Atualmente, AD FS administradores precisam configurar uma política para cada uma dessas fases separadamente.  Isso também envolve a compreensão das implicações dessas políticas e se essas políticas têm a dependência entre elas. Além disso, os administradores precisam entender a linguagem de regra de declaração e criar regras personalizadas para habilitar algumas políticas simples/comuns (por exemplo, bloquear acesso externo).  
  
O que os modelos de política de controle de acesso fazem é substituir esse modelo antigo, em que os administradores precisam configurar regras de autorização de emissão usando linguagem de declarações.  Os antigos cmdlets do PowerShell das regras de autorização de emissão ainda se aplicam, mas são mutuamente exclusivos do novo modelo. Os administradores podem optar por usar o novo modelo ou o modelo antigo.  O novo modelo permite que os administradores controlem quando conceder acesso, incluindo a imposição da autenticação multifator.  
  
Os modelos de política de controle de acesso usam um modelo de permissão.  Isso significa, por padrão, que ninguém tem acesso e que o acesso deve ser concedido explicitamente.  No entanto, isso não é apenas uma permissão de tudo ou nada.  Os administradores podem adicionar exceções à regra de permissão.  Por exemplo, um administrador pode desejar conceder acesso com base em uma rede específica selecionando essa opção e especificando o intervalo de endereços IP.  Mas o administrador pode adicionar e exceção, por exemplo, o administrador pode adicionar uma exceção de uma rede específica e especificar esse intervalo de endereços IP.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Modelos de política de controle de acesso interno vs. modelos de política de controle de acesso personalizados  
O AD FS inclui vários modelos de política de controle de acesso interno.  Eles visam alguns cenários comuns que têm o mesmo conjunto de requisitos de política, por exemplo, política de acesso de cliente para o Office 365.  Esses modelos não podem ser modificados.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para fornecer maior flexibilidade para atender às suas necessidades de negócios, os administradores podem criar seus próprios modelos de política de acesso.  Eles podem ser modificados após a criação e as alterações no modelo de política personalizada serão aplicadas a todos os RPs que são controlados por esses modelos de política.  Para adicionar um modelo de política personalizada, basta clicar em Adicionar política de controle de acesso de dentro do gerenciamento de AD FS.  
  
Para criar um modelo de política, um administrador precisa primeiro especificar sob quais condições uma solicitação será autorizada para emissão de token e/ou delegação. As opções de condição e ação são mostradas na tabela a seguir.   As condições em negrito podem ser configuradas ainda mais pelo administrador com valores diferentes ou novos. O administrador também pode especificar exceções se houver alguma. Quando uma condição for atendida, uma ação de permissão não será disparada se houver uma exceção especificada e a solicitação de entrada corresponder à condição especificada na exceção.  
  
|Permitir usuários|Except| 
| --- | --- | 
 |De uma rede **específica**|De uma rede **específica**<p>De grupos **específicos**<p>De dispositivos com níveis de confiança **específicos**<p>Com declarações **específicas** na solicitação|  
|De grupos **específicos**|De uma rede **específica**<p>De grupos **específicos**<p>De dispositivos com níveis de confiança **específicos**<p>Com declarações **específicas** na solicitação|  
|De dispositivos com níveis de confiança **específicos**|De uma rede **específica**<p>De grupos **específicos**<p>De dispositivos com níveis de confiança **específicos**<p>Com declarações **específicas** na solicitação|  
|Com declarações **específicas** na solicitação|De uma rede **específica**<p>De grupos **específicos**<p>De dispositivos com níveis de confiança **específicos**<p>Com declarações **específicas** na solicitação|  
|E exigem a autenticação multifator|De uma rede **específica**<p>De grupos **específicos**<p>De dispositivos com níveis de confiança **específicos**<p>Com declarações **específicas** na solicitação|  
  
Se um administrador selecionar várias condições, elas serão de **e** a relação. As ações são mutuamente exclusivas e para uma regra de política, você só pode escolher uma ação. Se o administrador selecionar várias exceções, elas serão de uma relação **ou** . Alguns exemplos de regras de política são mostrados abaixo:  
  
|**Política**|**Regras de política**|
| --- | --- |  
|O acesso à extranet requer MFA<p>Todos os usuários são permitidos|**#1 de regra**<p>da **extranet**<p>e com MFA<p>Permitir<p>**Regra n º 2**<p>da **intranet**<p>Permitir|  
|Acesso externo não é permitido, exceto não FTE<p>O acesso à intranet para FTE no dispositivo ingressado no local de trabalho é permitido|**#1 de regra**<p>Da **extranet**<p>e de um grupo **não FTE**<p>Permitir<p>**#2 de regra**<p>da **intranet**<p>e do dispositivo **ingressado no local de trabalho**<p>e do grupo **FTE**<p>Permitir|  
|O acesso à extranet requer MFA, exceto "administrador do serviço"<p>Todos os usuários têm permissão para acessar|**#1 de regra**<p>da **extranet**<p>e com MFA<p>Permitir<p>**Grupo de administradores de serviço** , exceto<p>**#2 de regra**<p>always<p>Permitir|  
|o dispositivo ingressado no local de não trabalho de extranet requer MFA<p>Permitir acesso à infraestrutura de intranet e extranet do AD|**#1 de regra**<p>da **intranet**<p>E do grupo do **ad Fabric**<p>Permitir<p>**#2 de regra**<p>da **extranet**<p>e do dispositivo **não ingressado no local de trabalho**<p>e do grupo do **ad Fabric**<p>e com MFA<p>Permitir<p>**#3 de regra**<p>da **extranet**<p>e do dispositivo **ingressado no local de trabalho**<p>e do grupo do **ad Fabric**<p>Permitir|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modelo de política com parâmetros vs. modelo de política não parametrizada  
As políticas de controle de acesso podem ser  
  
Um modelo de política com parâmetros é um modelo de política que tem parâmetros. Um administrador precisa inserir o valor para esses parâmetros ao atribuir esse modelo ao administrador do RPs.An não é possível fazer alterações no modelo de política com parâmetros depois que ele tiver sido criado.  Um exemplo de uma política com parâmetros é a política interna, permitir grupo específico.  Sempre que essa política é aplicada a um RP, esse parâmetro precisa ser especificado.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Um modelo de política não parametrizado é um modelo de política que não tem parâmetros. Um administrador pode atribuir esse modelo ao RPs sem nenhuma entrada necessária e pode fazer alterações em um modelo de política não parametrizado depois que ele tiver sido criado.  Um exemplo disso é a política interna, permitir todos e exigir MFA.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso sem parâmetros  
Para criar uma política de controle de acesso sem parâmetros, use o procedimento a seguir  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso sem parâmetros  
  
1.  Em gerenciamento de AD FS à esquerda, selecione políticas de controle de acesso e, à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir usuários com dispositivos autenticados.  
  
3.  Em **permitir acesso se qualquer uma das regras a seguir for atendida**, clique em **Adicionar**.  
  
4.  Em permitir, coloque uma marca na caixa ao lado **de dispositivos com nível de confiança específico**  
  
5.  Na parte inferior, selecione o **específico** sublinhado  
  
6.  Na janela que aparece, selecione **autenticado** na lista suspensa.  Clique em **OK**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Clique em **OK**. Clique em **OK**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso com parâmetros  
Para criar uma política de controle de acesso com parâmetros, use o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso com parâmetros  
  
1.  Em gerenciamento de AD FS à esquerda, selecione políticas de controle de acesso e, à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir que usuários com uma declaração específica.  
  
3.  Em **permitir acesso se qualquer uma das regras a seguir for atendida**, clique em **Adicionar**.  
  
4.  Em permitir, marque a caixa ao lado de **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o **específico** sublinhado  
  
6.  Na janela que aparece, selecione **o parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **OK**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Clique em **OK**. Clique em **OK**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Como criar uma política de controle de acesso personalizada com uma exceção  
Para criar uma política de controle de acesso com uma exceção, use o procedimento a seguir.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para criar uma política de controle de acesso personalizada com uma exceção  
  
1.  Em gerenciamento de AD FS à esquerda, selecione políticas de controle de acesso e, à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir usuários com dispositivos autenticados, mas não gerenciados.  
  
3.  Em **permitir acesso se qualquer uma das regras a seguir for atendida**, clique em **Adicionar**.  
  
4.  Em permitir, coloque uma marca na caixa ao lado **de dispositivos com nível de confiança específico**  
  
5.  Na parte inferior, selecione o **específico** sublinhado  
  
6.  Na janela que aparece, selecione **autenticado** na lista suspensa.  Clique em **OK**.  
  
7.  Em Except, coloque uma marca na caixa ao lado **de dispositivos com nível de confiança específico**  
  
8.  Na parte inferior sob Except, selecione a linha de base **específica**  
  
9. Na janela que aparece, selecione **gerenciado** na lista suspensa.  Clique em **OK**.  
  
10. Clique em **OK**. Clique em **OK**.  
  
    ![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Como criar uma política de controle de acesso personalizada com várias condições de permissão  
Para criar uma política de controle de acesso com várias condições de permissão, use o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso com parâmetros  
  
1.  Em gerenciamento de AD FS à esquerda, selecione políticas de controle de acesso e, à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir que usuários com uma declaração específica e de um grupo específico.  
  
3.  Em **permitir acesso se qualquer uma das regras a seguir for atendida**, clique em **Adicionar**.  
  
4.  Em permitir, marque a caixa ao lado **de um grupo específico** e **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o sublinhado **específico** para a primeira condição, ao lado de grupos  
  
6.  Na janela que aparece, selecione **o parâmetro especificado quando a política é atribuída**.  Clique em **OK**.  
  
7.  Na parte inferior, selecione o sublinhado **específico** para a segunda condição, ao lado de declarações  
  
8.  Na janela que aparece, selecione **o parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **OK**.  
  
9. Clique em **OK**. Clique em **OK**.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Como atribuir uma política de controle de acesso a um novo aplicativo  
A atribuição de uma política de controle de acesso a um novo aplicativo é muito simples e agora foi integrada ao Assistente para adicionar um RP.  No assistente de confiança de terceira parte confiável, você pode selecionar a política de controle de acesso que deseja atribuir.  Esse é um requisito ao criar uma nova relação de confiança de terceira parte confiável.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Como atribuir uma política de controle de acesso a um aplicativo existente  
Atribuir uma política de controle de acesso a um aplicativo existente simplesmente selecione o aplicativo de relações de confiança de terceira parte confiável e clique com o botão direito do mouse em **Editar política de controle de acesso**.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
A partir daqui, você pode selecionar a política de controle de acesso e aplicá-la ao aplicativo.  
  
![políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Consulte Também  
[Operações do AD FS](../ad-fs-operations.md) 
