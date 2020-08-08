---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: Lista de verificação-Configurando AD FS para consumir declarações de AD FS 1. x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 27965c489b1f4a678d7d6212eb858b094b050dc3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969753"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de verificação: configurando AD FS para enviar declarações a um Serviço de Federação do AD FS 1.x


## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Lista de verificação: Configurar AD FS para enviar declarações para um AD FS 1. x Serviço de Federação
Esta lista de verificação inclui as tarefas que são necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \( AD FS \) serviço de Federação no Windows Server 2012 para enviar declarações que podem ser compreendidas por um AD FS 1.* x* serviço de Federação.

> [!NOTE]
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.

![Configurar AD FS para enviar a](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de declarações: Configurando AD FS para enviar declarações para um AD FS 1. x serviço de Federação**

|Tarefa|Referência|
|--------|-------------|
|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|![Configurar AD FS para enviar o](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento de declarações para interoperabilidade com AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|Antes de poder obter interoperabilidade com uma versão anterior do AD FS, você deve primeiro criar uma terceira parte confiável no Serviço de Federação de AD FS para o AD FS 1. *x* serviço de Federação. **Observação:** Não é possível criar uma relação de confiança com um AD FS 1. *x* serviço de Federação usando metadados de Federação.<p>Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no assistente Adicionar confiança de terceira parte confiável para configurar essa relação de confiança para interoperar com um AD FS 1. Serviço de Federação *x* :<p>1. na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2. na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3. na página **Configurar URL** , em ** \- URL passiva do WS Federation**, digite a **URL do ponto de extremidade serviço de Federação** conforme definido no AD FS 1.* x* serviço de Federação do parceiro.<br />4. na página **Configurar identificadores** , em **identificador de confiança de parte confiável**, digite o **URI de serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.|![Configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma confiança de terceira parte confiável manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|
|Na terceira parte confiável que você criou anteriormente, você deve criar regras de declaração que usarão declarações de entrada que foram extraídas de um repositório de atributos e passarão, filtrarão ou transformarão em um tipo de declaração de ID de nome que pode ser compreendido e consumido pelo AD FS 1. *x* serviço de Federação. **Observação:** Antes de criar essa regra, verifique se o conjunto de regras de declaração em que você está criando essa regra tem uma regra que vem antes de extrair primeiro uma \( declaração de atributo LDAP do protocolo de acesso ao diretório Lightweight \) de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x* \- declaração compatível. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar uma declaração compatível com o AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|
|Contate o administrador do AD FS 1. *x* serviço de Federação e ter o administrador do AD FS 1. *x* serviço de Federação configurar uma nova relação de confiança de parceiro de conta. Além disso, forneça a esse administrador o URI de Serviço de Federação \( nas propriedades de serviço de Federação \) , a URL do ponto de \- extremidade passivo do WS Federation \( a URL do ponto de extremidade do serviço de Federação \) e um arquivo de certificado de assinatura de token exportado \- \( somente com chave pública \) . Esse administrador precisará desses itens para configurar a relação de confiança.|N\/D|

