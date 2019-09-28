---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 689bd33bc95c2b142dfbe6d0448a604b2971979e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359984"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: Configurando AD FS para enviar declarações para um agente Web com reconhecimento de declaração do AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de verificação: Configurando AD FS para enviar declarações para uma AD FS declarações 1. x @ no__t-0aware Web Agent  
Esta lista de verificação inclui as tarefas necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 Serviço de Federação no Windows Server 2012 para enviar declarações que podem ser compreendidas por um aplicativo hospedado por um servidor Web executando o AD FS 1. *x* Claims @ no__t-3aware Web Agent.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![configure AD FS para enviar declarações @ no__t-1Checklist: Configurar AD FS para enviar declarações para um AD FS declarações 1. x @ no__t-0aware Web Agent @ no__t-1  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|![configure AD FS enviar o](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento de declarações para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Se você ainda não tiver feito isso, use o link à direita para primeiro criar uma relação de confiança de terceira parte confiável entre o AD FS Serviço de Federação no Windows Server 2012 e no AD FS 1. *x* serviço de Federação.|[Lista de verificação: Como configurar o AD FS para enviar declarações a um Serviço de Federação do AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Antes de poder obter interoperação com um aplicativo hospedado pelo AD FS 1. *x* Claims @ no__t-1aware Web Agent, você deve primeiro criar uma relação de confiança de terceira parte confiável no AD FS serviço de Federação no Windows Server 2012 para o AD FS 1. *x* Claims @ no__t-1aware Web Agent. **Observação:** A criação dessa relação de confiança no AD FS Serviço de Federação é o equivalente a adicionar um novo **aplicativo** à AD FS 1. x serviço de Federação \(**serviço de Federação @ No__t-3Trust política @ No__t-4My Organization @ no__t-5Application**\). Essa confiança de terceira parte confiável é necessária porque AD FS não tem um nó de **aplicativo** equivalente em seu próprio snap @ no__t-1in. No entanto, ele ainda deve ter um canal seguro para o aplicativo.<br /><br />Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no assistente Adicionar confiança de terceira parte confiável para configurar essa relação de confiança para interoperar com um AD FS 1. *x* Claims @ no__t-1aware Web Agent:<br /><br />1.  Na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2.  Na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3.  Na página **Configurar URL** , em **URL passiva do WS @ no__t-2Federation**, digite a **URL do aplicativo** , conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4.  Na página **Configurar identificadores** , em **identificador de confiança da parte confiável**, digite a **URL do aplicativo** , conforme definido no AD FS 1. *x* Claims @ no__t-4aware Web Agent|![configure AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma relação de confiança de terceira parte confiável manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Contate o administrador do servidor Web que executa o AD FS 1. *x* Claims @ no__t-1aware Web Agent e fazer com que o administrador edite o arquivo Web. config associado ao aplicativo Claims @ no__t-2aware \(Under o site padrão no serviços de informações da Internet \(IIS @ no__t-5 @ no__t-6 para aponte o agente da Web no Serviço de Federação de AD FS.<br /><br />Por exemplo, substitua *myresourcefederationserver* na marca `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` do arquivo Web. config por um nome de servidor de Federação AD FS válido.<br /><br />Isso é necessário para o aplicativo e AD FS declarações de 1. x @ no__t-0aware Web Agent para poder consumir as declarações enviadas a ele do Serviço de Federação de AD FS no Windows Server 2012.|N\/A|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Na terceira parte confiável que você criou anteriormente, você precisa criar regras de declaração que usarão declarações de entrada que foram extraídas de um repositório de atributos e passarão, filtrarão ou transformarão em um tipo de declaração de ID de nome que pode ser compreendido e consumido pelo AD FS 1. *x* Claims @ no__t-1aware Web Agent. **Observação:** Antes de criar essa regra, certifique-se de que o conjunto de regras de declaração em que você está criando essa regra tenha uma regra que venha antes de extrair primeiro uma declaração de atributo \(LDAP @ no__t-1 de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. Declaração *x*\-compatible. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crie uma regra para enviar uma declaração compatível com AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

