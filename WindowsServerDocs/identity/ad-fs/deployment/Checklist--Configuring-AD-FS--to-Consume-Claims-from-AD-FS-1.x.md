---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: Lista de verificação – configuração do AD FS para consumir as declarações do AD FS 1.x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 41a71ff49d211d294768c0e4a55692ced3f2d844
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192452"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de verificação: Configurar o AD FS para consumir as declarações do AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Lista de verificação: Configurar o AD FS para consumir as declarações do AD FS 1.x  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(do AD FS\) serviço de federação no Windows Server 2012 consumam as declarações que são enviadas por um AD FS 1. *x* serviço de Federação.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![consumam as declarações do AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Configurar o AD FS para consumir as declarações do AD FS 1.x**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![consumam as declarações do AD FS](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID de nome de tipo de declaração.|![consumam as declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![consumam as declarações do AD FS](media/icon_checkboxo.gif)|Antes de você pode interoperar com uma versão anterior do AD FS, você primeiro deve criar uma relação de confiança do provedor de declarações no serviço de Federação do AD FS. **Observação:** É possível criar uma relação de confiança com um AD FS 1. *x* serviço de Federação usando metadados de Federação.<br /><br />Quando você configura a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no Assistente adicionar declarações provedor de confiança para configurar essa relação de confiança para interoperar com um AD FS 1. *x* serviço de Federação:<br /><br />1.  Sobre o **Selecionar fonte de dados** , selecione **inserir os dados sobre a terceira parte confiável de terceiros de confiança manualmente**.<br />2.  Sobre o **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3.  No **configurar URL do** página, em **WS\-URL f PRP**, tipo o **URL de ponto de extremidade de serviço de Federação** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4.  Sobre o **configurar identificadores** página em **identificador de relação de confiança do provedor de declarações**, tipo o **URI do serviço de Federação** conforme definido no AD FS, 1. *x* serviço de Federação do parceiro.|![consumam as declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar um Claims Provider Trust Manually](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![consumam as declarações do AD FS](media/icon_checkboxo.gif)|Sobre o provedor de declarações que você criou anteriormente, você deverá criar uma regra de declaração que levará declarações de entrada do AD FS 1.x Federation Service e passem, filtrar ou transformá-las em um tipo de declaração de ID de nome.<br /><br />Quando o tipo de declaração de ID de nome foi passado, filtrados ou transformados, ele pode ser usado como entrada para outra regra ou regras, para que possa ser compreendido e consumido pelo serviço de Federação do AD FS no Windows Server 2012.|![consumam as declarações do AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1.x declaração compatível com](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![consumam as declarações do AD FS](media/icon_checkboxo.gif)|Entre em contato com o administrador do AD FS 1. *x* serviço de Federação e que o administrador do AD FS 1. *x* serviço de Federação configurar uma nova relação de confiança de parceiro de recurso. Além disso, forneça ao administrador que o URI do serviço de Federação \(nas propriedades do serviço de Federação\), a URL de ponto de extremidade de serviço de Federação e um token exportado\-arquivo de certificado de assinatura \(com Somente chave pública\). O administrador precisará desses itens para configurar a relação de confiança.|N\/A|  
  

