---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: "Lista de verificação - Configurando o AD FS para consumir declarações do AD FS 1. x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fe99487d3a770547af36f69722b442d0e2cbb8b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de verificação: Configurar o AD FS para consumir declarações do AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-ad-fs-1x"></a>Lista de verificação: Configurar o AD FS para consumir declarações do AD FS 1. x  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(AD FS\) serviço de federação no Windows Server 2012 para consumir declarações que são enviadas por um AD FS 1. *x* serviço de Federação.  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um procedimento, retorne para este tópico depois de concluir as etapas no procedimento para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Consumir declarações do AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: configurar o AD FS para consumir declarações do AD FS 1. x**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Consumir declarações do AD FS](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID do nome do tipo de declaração.|![Consumir declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Consumir declarações do AD FS](media/icon_checkboxo.gif)|Antes de você pode interoperar com uma versão anterior do AD FS, você deve primeiro criar uma relação de confiança do provedor de declarações no serviço de Federação AD FS. **Observação:** não é possível criar uma relação de confiança com um 1 do AD FS. *x* serviço de Federação usando metadados de Federação.<br /><br />Quando você configura a relação de confiança usando o procedimento no link para a direita, você deve fazer o seguinte em Adicionar declarações do provedor de confiança Assistente para configurar esta relação de confiança para interoperar com um 1 do AD FS. *x* serviço de Federação:<br /><br />1. Diante o **Selecionar fonte de dados**, selecione **inserir os dados sobre a dependência de terceiros confiança manualmente**.<br />2. Diante do **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3. Na **configurar URL** página, em **WS\ federação passiva URL**, tipo a **URL de ponto de extremidade de serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4. Pelo **configurar identificadores** página, em **identificador de confiança do provedor de declarações**, tipo a **URI do serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.|![Consumir declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar um requerimentos judiciais ou Extrajudiciais provedor confiar manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![Consumir declarações do AD FS](media/icon_checkboxo.gif)|Sobre a confiança do provedor de declarações que você criou anteriormente, você deve criar uma regra de declaração que levará declarações que são recebidas do AD FS 1. x serviço de Federação passagem, filtrar e as transforma em um tipo de declaração de nome ID.<br /><br />Quando o tipo de declaração de nome ID foi passado, filtrados ou transformado, ele pode ser usado como entrada para outra regra ou regras, para que possa ser entendido e consumida pelo serviço de Federação do AD FS no Windows Server 2012.|![Consumir declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1. x reivindicação compatível](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![Consumir declarações do AD FS](media/icon_checkboxo.gif)|Contate o administrador do AD FS 1. *x* serviço de Federação e peça ao administrador do AD FS 1. *x* serviço de Federação configurar uma nova confiança de parceiro de recurso. Além disso, forneça esse administrador com o URI do serviço de Federação \ (em properties\ o serviço de federação), a URL de ponto de extremidade de serviço de Federação e token\-assinatura exportado arquivo de certificado \ (com only\ de chave pública). O administrador precisará desses itens para configurar a relação de confiança.|N\/A|  
  

