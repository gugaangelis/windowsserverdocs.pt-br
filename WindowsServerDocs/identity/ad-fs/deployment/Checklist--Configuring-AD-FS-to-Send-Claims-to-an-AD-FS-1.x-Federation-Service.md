---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: "Lista de verificação - Configurando o AD FS para consumir declarações do AD FS 1. x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de verificação: Configurar o AD FS para enviar solicitações a um serviço de Federação do AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de verificação: Configurar o AD FS para enviar declarações para um AD FS 1. x serviço de Federação  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(AD FS\) serviço de federação no Windows Server 2012 para enviar declarações que podem ser compreendidas por um AD FS 1. *x* serviço de Federação.  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um procedimento, retorne para este tópico depois de concluir as etapas no procedimento para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: configurar o AD FS para enviar declarações para um AD FS 1. x serviço de Federação**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID do nome do tipo de declaração.|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Antes de você pode alcançar a interoperabilidade com uma versão anterior do AD FS, você deve primeiro criar uma terceira confiança de terceiros no serviço de Federação AD FS para o AD FS 1. *x* serviço de Federação. **Observação:** não é possível criar uma relação de confiança com um 1 do AD FS. *x* serviço de Federação usando metadados de Federação.<br /><br />Quando você configura a relação de confiança usando o procedimento no link para a direita, você deve fazer o seguinte no Adicionar dependência terceiros confiança Assistente para configurar esta relação de confiança para interoperar com um 1 do AD FS. *x* serviço de Federação:<br /><br />1. Diante o **Selecionar fonte de dados**, selecione **inserir os dados sobre a dependência de terceiros confiança manualmente**.<br />2. Diante do **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3. Na **configurar URL** página, em **WS\ federação passiva URL**, tipo a **URL de ponto de extremidade de serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4. Pelo **configurar identificadores** página, em **identificador de confiança parte Relying**, tipo a **URI do serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma dependência terceiros confiar manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Na terceira confiança de terceiros que você criou anteriormente, você deve criar reivindicação regras que serão tirar declarações de entrada que foram extraídas de uma loja de atributo e passagem, filtrar ou transformá-las em uma ID de nome reivindicar tipo que pode ser entendido e consumido pelo AD FS 1. *x* serviço de Federação. **Observação:** antes de criar essa regra, certifique-se de que o conjunto de regras de declaração onde você estiver criando essa regra tem uma regra que vem antes que extrai primeiro uma reivindicação de atributo Lightweight Directory Access Protocol \(LDAP\) de um repositório de atributo. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x*\-compatible reivindicação. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar LDAP atributos como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1. x reivindicação compatível](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Contate o administrador do AD FS 1. *x* serviço de Federação e peça ao administrador do AD FS 1. *x* serviço de Federação configurar uma nova confiança de parceiro de conta. Além disso, forneça esse administrador com o URI do serviço de Federação \ (em properties\ o serviço de federação), a URL de ponto de extremidade WS\ federação passiva \ (o serviço de Federação ponto de extremidade URL\), e um exportados token\ assinatura de arquivo de certificado \ (com only\ de chave pública). Esse administrador terá esses itens para configurar a relação de confiança.|N\/A|  
  

