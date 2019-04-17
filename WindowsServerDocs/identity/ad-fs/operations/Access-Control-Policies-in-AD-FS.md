---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: "Políticas de controle de acesso no AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Políticas de controle de acesso no Windows Server 2016 AD FS

>Aplica-se a: Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modelos de política de controle de acesso no AD FS  
Serviços de Federação do Active Directory agora dá suporte ao uso de modelos de política de controle de acesso.  Usando modelos de política de controle de acesso, um administrador pode impor configurações de política atribuindo o modelo da política em um grupo de partes confiantes (RPs). Administrador também pode fazer atualizações para o modelo de política e as alterações serão aplicadas para as partes confiantes automaticamente se não houver nenhuma interação do usuário necessária.  
  
## <a name="what-are-access-control-policy-templates"></a>Quais são os modelos de política de controle de acesso?  
O pipeline de núcleo do AD FS para processamento de política tem três fases: emissão de autenticação, autorização e reivindicação. Atualmente, os administradores do AD FS precisam configurar uma política para cada uma dessas fases separadamente.  Isso também envolve Noções básicas sobre as implicações dessas políticas e se essas políticas têm dependência entre. Além disso, os administradores têm a entender as reivindicação regra idioma e autor regras personalizadas para permitir que alguns política simples/comum (ex. Bloquear acesso externo).  
  
Qual política de controle de acesso modelos fazem é substituir esse modelo antigo onde os administradores precisam configurar regras de autorização de emissão usando declarações do idioma.  Os cmdlets do PowerShell antigos de regras de autorização de emissão ainda se aplicam, mas é mutuamente excluem o novo modelo. Os administradores podem escolher para usar o novo modelo ou o modelo antigo.  O novo modelo permite que os administradores controlem quando conceder acesso, inclusive aplicando a autenticação multifator.  
  
Modelos de política de controle de acesso usam um modelo de permissão.  Isso significa que por padrão, ninguém tem acesso e que o acesso deve ser concedido explicitamente.  No entanto, isso não é apenas um tudo ou nada permitir.  Os administradores podem adicionar exceções à regra permitir.  Por exemplo, um administrador pode querer conceder acesso com base em uma rede específica, selecionar essa opção e especificando o intervalo de endereços IP.  Mas o administrador pode adicionar e exceção, por exemplo, o administrador pode adicionar uma exceção em uma rede específica e especificar esse intervalo de endereços IP.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Acesso interno política modelos vs acesso personalizado controle política modelos de controle  
AD FS inclui vários modelos de política de controle de acesso interno.  Esses direcionar alguns cenários comuns que têm o mesmo conjunto de requisitos da política, por exemplo política de acesso do cliente para o Office 365.  Esses modelos não podem ser modificados.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para proporcionar maior flexibilidade para atender às necessidades de negócios, os administradores podem criar seu próprios acesso modelos de política.  Eles podem ser modificados após a criação e alterações ao modelo de política personalizada serão aplicadas a todos os RPs que são controladas por esses modelos de política.  Para adicionar um modelo de política personalizada simplesmente clicar adicionar política de controle de acesso de dentro de gerenciamento do AD FS.  
  
Para criar um modelo de política, o administrador precisa primeiro especificar condições sob as quais uma solicitação será autorizada para a emissão de token e/ou delegação. Opções de condição e ações são mostradas na tabela a seguir.   Condições em negrito podem ser ainda mais configuradas pelo administrador com valores novos ou diferentes. Administrador também pode especificar exceções, se houver alguma. Quando uma condição é atendida, uma ação de permissão não será acionada se há uma exceção especificada e a solicitação de entrada corresponda a condição especificada na exceção.  
  
|Permitir que os usuários|Exceto| 
| --- | --- | 
 |De **específico** rede|De **específico** rede<br /><br />De **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|  
|De **específico** grupos|De **específico** rede<br /><br />De **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|  
|Em dispositivos com **específico** níveis de confiança|De **específico** rede<br /><br />De **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|  
|Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|De **específico** rede<br /><br />De **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|  
|E exigir autenticação multifator|De **específico** rede<br /><br />De **específico** grupos<br /><br />Em dispositivos com **específico** níveis de confiança<br /><br />Com **específico** requerimentos judiciais ou Extrajudiciais na solicitação|  
  
Se um administrador selecionar várias condições, eles são do **AND** relação. As ações são mutuamente exclusivas e para a regra de uma política, você pode escolher apenas uma ação. Se o administrador seleciona vários exceções, eles são de um **ou** relação. Alguns exemplos de regras de política são mostrados abaixo:  
  
|**Política**|**Regras de política**|
| --- | --- |  
|Acesso à extranet requer MFA<br /><br />Todos os usuários são permitidos|**Regra #1**<br /><br />De **extranet**<br /><br />E com MFA<br /><br />Permitir<br /><br />**Regra n º 2**<br /><br />De **intranet**<br /><br />Permitir|  
|Acesso externo não são permitidos exceto não FTE<br /><br />Acesso à intranet para FTE no dispositivo da empresa associado são permitidos|**Regra #1**<br /><br />De **extranet**<br /><br />Em **não FTE** grupo<br /><br />Permitir<br /><br />**Regra #2**<br /><br />De **intranet**<br /><br />Em **local de trabalho ingressado** dispositivo<br /><br />Em **FTE** grupo<br /><br />Permitir|  
|Acesso à extranet requer MFA exceto "admin serviço"<br /><br />Todos os usuários têm permissão para acessar|**Regra #1**<br /><br />De **extranet**<br /><br />E com MFA<br /><br />Permitir<br /><br />Exceto **grupo de administração de serviço**<br /><br />**Regra #2**<br /><br />sempre<br /><br />Permitir|  
|Dispositivo associado ao local de trabalho acessando de extranet requer MFA<br /><br />Permitir a malha de anúncios de intranet e extranet acesso|**Regra #1**<br /><br />De **intranet**<br /><br />Em **AD Fabric** grupo<br /><br />Permitir<br /><br />**Regra #2**<br /><br />De **extranet**<br /><br />Em **não-local de trabalho ingressado** dispositivo<br /><br />Em **AD Fabric** grupo<br /><br />E com MFA<br /><br />Permitir<br /><br />**Regra #3**<br /><br />De **extranet**<br /><br />Em **local de trabalho ingressado** dispositivo<br /><br />Em **AD Fabric** grupo<br /><br />Permitir|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modelo de política sem parâmetros política parametrizados template vs  
Políticas de controle de acesso podem ser  
  
Um modelo de política parametrizados é um modelo de política que tem parâmetros. Um administrador precisar informar o valor para esses parâmetros ao atribuir este modelo para RPs.An administrador não pode fazer alterações ao modelo de política parametrizados após ele ter sido criado.  Um exemplo de uma política parametrizado é a política interna, o grupo específico de permissão.  Sempre que essa política é aplicada a uma RP, este parâmetro deve ser especificado.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Um modelo de política sem parâmetros é um modelo de política que não tem parâmetros. Um administrador pode atribuir este modelo RPS sem qualquer entrada necessária e pode fazer alterações em um modelo de política sem parâmetros após ele ter sido criado.  Um exemplo disso é a política interna, permitir que todos e exigem MFA.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso sem parâmetros  
Para criar um acesso sem parâmetros política de controle use o procedimento a seguir  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso sem parâmetros  
  
1.  Do AD FS gerenciamento no lado esquerdo selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir que os usuários com dispositivos autenticados.  
  
3.  Em **permitir acesso se alguma das regras seguir forem atendidas**, clique em **adicionar**.  
  
4.  Em permitir, coloque uma seleção na caixa ao lado de **de dispositivos com nível de confiança específico**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela ou estalos-up, selecione **autenticados** na lista suspensa.  Clique em **Okey**.  
  
    ![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Clique em **Okey**. Clique em **Okey**.  
  
    ![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Como criar uma política de controle de acesso parametrizados  
Para criar um controle de acesso parametrizados diretiva use o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso parametrizados  
  
1.  Do AD FS gerenciamento no lado esquerdo selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir que os usuários com uma declaração específica.  
  
3.  Em **permitir acesso se alguma das regras seguir forem atendidas**, clique em **adicionar**.  
  
4.  Em permitir, coloque uma seleção na caixa ao lado de **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela ou estalos-up, selecione **parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **Okey**.  
  
    ![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Clique em **Okey**. Clique em **Okey**.  
  
    ![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Como criar uma política de controle de acesso personalizado com uma exceção  
Para criar um controle de acesso à política com uma exceção use o procedimento a seguir.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para criar uma política de controle de acesso personalizado com uma exceção  
  
1.  Do AD FS gerenciamento no lado esquerdo selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir aos usuários autenticados dispositivos, mas não gerenciados.  
  
3.  Em **permitir acesso se alguma das regras seguir forem atendidas**, clique em **adicionar**.  
  
4.  Em permitir, coloque uma seleção na caixa ao lado de **de dispositivos com nível de confiança específico**  
  
5.  Na parte inferior, selecione o sublinhado **específico**  
  
6.  Na janela ou estalos-up, selecione **autenticados** na lista suspensa.  Clique em **Okey**.  
  
7.  Exceto em, coloque uma seleção na caixa ao lado de **de dispositivos com nível de confiança específico**  
  
8.  Na parte inferior em exceto, selecione o sublinhado **específico**  
  
9. Na janela ou estalos-up, selecione **gerenciado** na lista suspensa.  Clique em **Okey**.  
  
10. Clique em **Okey**. Clique em **Okey**.  
  
    ![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Como criar uma política de controle de acesso personalizado com várias condições permitir  
Para criar uma política de controle de acesso com permitir várias condições usam o procedimento a seguir  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para criar uma política de controle de acesso parametrizados  
  
1.  Do AD FS gerenciamento no lado esquerdo selecione políticas de controle de acesso e à direita, clique em Adicionar política de controle de acesso.  
  
2.  Insira um nome e uma descrição.  Por exemplo: permitir que os usuários com uma declaração específica e de grupo específico.  
  
3.  Em **permitir acesso se alguma das regras seguir forem atendidas**, clique em **adicionar**.  
  
4.  Em permitir, coloque uma seleção na caixa ao lado de **de um grupo específico** e **com declarações específicas na solicitação**  
  
5.  Na parte inferior, selecione o sublinhado **específico** referentes à primeira condição, ao lado de grupos  
  
6.  Na janela ou estalos-up, selecione **parâmetro especificado quando a política for atribuída**.  Clique em **Okey**.  
  
7.  Na parte inferior, selecione o sublinhado **específico** para a segunda condição, ao lado de declarações  
  
8.  Na janela ou estalos-up, selecione **parâmetro especificado quando a política de controle de acesso é atribuída**.  Clique em **Okey**.  
  
9. Clique em **Okey**. Clique em **Okey**.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Como atribuir uma política de controle de acesso a um novo aplicativo  
Atribuir uma política de controle de acesso a um novo aplicativo é muito direto e agora foi integrado ao Assistente para adicionar uma RP.  O Assistente para contar confiança de terceiros, você pode selecionar a política de controle de acesso que você deseja atribuir.  Esse é um requisito ao criar uma nova confiança confiante de terceiros.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Como atribuir uma política de controle de acesso a um aplicativo existente  
Atribuindo uma política de controle de acesso a um aplicativo existente basta selecionar o aplicativo de dependência confie de terceiros e sobre o botão direito do mouse **Editar política de controle de acesso**.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
A partir daqui, você pode selecionar a política de controle de acesso e aplicá-lo para o aplicativo.  
  
![Políticas de controle de acesso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

