---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: Lista de verificação-Configurando AD FS para consumir declarações de AD FS 1. x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cba16383b83fa8a53b28e4e1f70ffcebe63098d7
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520105"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de verificação: Configurando AD FS para consumir declarações de AD FS 1. x


## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Lista de verificação: Configurando AD FS para consumir declarações de AD FS 1. x
Esta lista de verificação inclui as tarefas necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \( AD FS \) serviço de Federação no Windows Server 2012 para consumir declarações enviadas por um AD FS 1.* x* serviço de Federação.

> [!NOTE]
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.

![consumir declarações da lista de verificação de AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: Configurando AD FS para consumir declarações de AD FS 1. x**

|Tarefa|Referência|
|--------|-------------|
|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|![consumir declarações do planejamento de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[para interoperabilidade com AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
| Antes de poder interoperar com uma versão anterior do AD FS, você deve primeiro criar uma confiança do provedor de declarações no Serviço de Federação AD FS. **Observação:** Não é possível criar uma relação de confiança com um AD FS 1. *x* serviço de Federação usando metadados de Federação.<p>Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no Assistente para adicionar confiança do provedor de declarações para configurar essa relação de confiança para interoperar com um AD FS 1. Serviço de Federação *x* :<p>1. na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2. na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3. na página **Configurar URL** , em ** \- URL passiva do WS Federation**, digite a **URL do ponto de extremidade serviço de Federação** conforme definido no AD FS 1.* x* serviço de Federação do parceiro.<br />4. na página **Configurar identificadores** , em **identificador de confiança do provedor de declarações**, digite o URI de **serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.|![consumir declarações de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma confiança do provedor de declarações manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|
| Na confiança do provedor de declarações que você criou anteriormente, você deve criar uma regra de declaração que receberá declarações que são recebidas do AD FS 1. x Serviço de Federação e passar, filtrar ou transformá-las em um tipo de declaração de ID de nome.<p>Quando o tipo de declaração de ID de nome tiver sido transmitido, filtrado ou transformado, ele poderá ser usado como entrada para outra regra ou regras para que possa ser compreendido e consumido pelo AD FS Serviço de Federação no Windows Server 2012.|![consumir declarações de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar uma declaração compatível com AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|
| Contate o administrador do AD FS 1. *x* serviço de Federação e ter o administrador do AD FS 1. *x* serviço de Federação configurar uma nova relação de confiança de parceiro de recurso. Além disso, forneça a esse administrador o URI de Serviço de Federação \( nas propriedades de serviço de Federação \) , a URL do ponto de extremidade serviço de Federação e um arquivo de certificado de assinatura de token exportado \- \( somente com a chave pública \) . O administrador precisará desses itens para configurar a relação de confiança.|N\/D|
