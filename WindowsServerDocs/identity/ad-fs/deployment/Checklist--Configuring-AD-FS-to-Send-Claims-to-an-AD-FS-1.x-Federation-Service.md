---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: Lista de verificação – configuração do AD FS para consumir as declarações do AD FS 1.x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bd4c436c806074f63bf51f497429532d7be32f75
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192406"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de verificação: Configurar o AD FS para enviar solicitações para um serviço de Federação do AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Lista de verificação: Configurar o AD FS para enviar solicitações ao an AD FS 1.x Federation Service  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(do AD FS\) serviço de federação no Windows Server 2012 para enviar as declarações que podem ser compreendidas por um AD FS 1. *x* serviço de Federação.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![Configurar o AD FS para enviar declarações](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Configurar o AD FS para enviar solicitações ao an AD FS 1.x Federation Service**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID de nome de tipo de declaração.|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Antes de alcançar interoperabilidade com uma versão anterior do AD FS, você deve primeiro criar uma terceira parte confiável no serviço de Federação do AD FS para o AD FS 1. *x* serviço de Federação. **Observação:** É possível criar uma relação de confiança com um AD FS 1. *x* serviço de Federação usando metadados de Federação.<br /><br />Quando você configura a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no Assistente de adição de terceira parte confiável terceiros de confiança para configurar essa relação de confiança para interoperar com um AD FS 1. *x* serviço de Federação:<br /><br />1.  Sobre o **Selecionar fonte de dados** , selecione **inserir os dados sobre a terceira parte confiável de terceiros de confiança manualmente**.<br />2.  Sobre o **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3.  No **configurar URL do** página, em **WS\-URL f PRP**, tipo o **URL de ponto de extremidade de serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4.  Sobre o **configurar identificadores** página em **identificador de relação de confiança de partes de terceira parte confiável**, tipo o **URI do serviço de Federação** conforme definido no AD FS, 1. *x* serviço de Federação do parceiro.|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar um Relying Party Trust Manually](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Sobre a terceira parte confiável que você criou anteriormente, você deve criar regras que irá levar as declarações de entrada que foram extraídas de um repositório de atributos e passar pelo, filtrar ou transformá-las em uma ID de nome de declaração de tipo que pode ser entendido e consumido pelo AD de declaração FS 1. *x* serviço de Federação. **Observação:** Antes de criar essa regra, certifique-se de que o conjunto de regras de declaração em que você está criando essa regra tem uma regra que vem antes de ele que extrai primeiro um Lightweight Directory Access Protocol \(LDAP\) declaração de atributo de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x*\-declaração compatível com. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [crie uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1.x declaração compatível com](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Entre em contato com o administrador do AD FS 1. *x* serviço de Federação e que o administrador do AD FS 1. *x* serviço de Federação configurar uma nova relação de confiança de parceiro de conta. Além disso, forneça ao administrador que o URI do serviço de Federação \(nas propriedades do serviço de Federação\), o WS\-URL de ponto de extremidade de federação passiva \(a URL de ponto de extremidade de serviço de Federação\), e um token exportado\-arquivo de certificado de assinatura \(com a chave pública só\). Esse administrador precisará desses itens para configurar a relação de confiança.|N\/A|  
  

