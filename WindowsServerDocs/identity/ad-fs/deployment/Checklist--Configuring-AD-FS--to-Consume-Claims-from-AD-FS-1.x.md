---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: Lista de verificação-Configurando AD FS para consumir declarações de AD FS 1. x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408488"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de verificação: Configurando AD FS para consumir declarações do AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Lista de verificação: Configurando AD FS para consumir declarações do AD FS 1. x  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 Serviço de Federação no Windows Server 2012 para consumir declarações enviadas por um AD FS 1. *x* serviço de Federação.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
declarações de @no__t 0consume de AD FS @ no__t-1Checklist: Configurando AD FS para consumir declarações do AD FS 1. x @ no__t-0  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![consumir declarações de AD FS](media/icon_checkboxo.gif)|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|declarações de @no__t 0consume de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejando a interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![consumir declarações de AD FS](media/icon_checkboxo.gif)|Antes de poder interoperar com uma versão anterior do AD FS, você deve primeiro criar uma confiança do provedor de declarações no Serviço de Federação AD FS. **Observação:** Não é possível criar uma relação de confiança com um AD FS 1. *x* serviço de Federação usando metadados de Federação.<br /><br />Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no Assistente para adicionar confiança do provedor de declarações para configurar essa relação de confiança para interoperar com um AD FS 1. Serviço de Federação *x* :<br /><br />1.  Na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2.  Na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3.  Na página **Configurar URL** , em **URL passiva do WS @ no__t-2Federation**, digite a **url do ponto de extremidade serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4.  Na página **Configurar identificadores** , em **identificador de confiança do provedor de declarações**, digite o URI de **serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.|declarações de @no__t 0consume de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma confiança de provedor de declarações manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![consumir declarações de AD FS](media/icon_checkboxo.gif)|Na confiança do provedor de declarações que você criou anteriormente, você deve criar uma regra de declaração que receberá declarações que são recebidas do AD FS 1. x Serviço de Federação e passar, filtrar ou transformá-las em um tipo de declaração de ID de nome.<br /><br />Quando o tipo de declaração de ID de nome tiver sido transmitido, filtrado ou transformado, ele poderá ser usado como entrada para outra regra ou regras para que possa ser compreendido e consumido pelo AD FS Serviço de Federação no Windows Server 2012.|declarações de @no__t 0consume do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar uma declaração compatível com o AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![consumir declarações de AD FS](media/icon_checkboxo.gif)|Contate o administrador do AD FS 1. *x* serviço de Federação e ter o administrador do AD FS 1. *x* serviço de Federação configurar uma nova relação de confiança de parceiro de recurso. Além disso, forneça a esse administrador o URI de Serviço de Federação \(in as propriedades de Serviço de Federação @ no__t-1, a URL do ponto de extremidade Serviço de Federação e um arquivo de certificado de token @ no__t-2signing exportado @no__t chave pública de 3With somente @ no__t-4. O administrador precisará desses itens para configurar a relação de confiança.|N\/A|  
  

