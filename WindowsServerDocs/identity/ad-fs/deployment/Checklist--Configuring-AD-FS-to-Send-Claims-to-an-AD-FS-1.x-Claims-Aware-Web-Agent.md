---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Implantando servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 874984b469303c0f8a40a676632c144ee6079f44
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192433"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: Configurar o AD FS para enviar solicitações para um agente Web com reconhecimento de declarações do AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de verificação: Configurar o AD FS para enviar as declarações de um AD FS 1.x\-Agente Web com reconhecimento  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(do AD FS\) serviço de federação no Windows Server 2012 para enviar as declarações que podem ser compreendidas por um aplicativo que é hospedado por um Servidor Web em execução do AD FS 1. *x* declarações\-Agente Web com reconhecimento.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![Configurar o AD FS para enviar declarações](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: Configurar o AD FS para enviar as declarações de um AD FS 1.x\-Agente Web com reconhecimento**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID de nome de tipo de declaração.|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Se você ainda não fez isso, use o link à direita para primeiro criar uma terceira parte confiável entre o serviço de Federação do AD FS no Windows Server 2012 e do AD FS 1. *x* serviço de Federação.|[Lista de verificação: Como configurar o AD FS para enviar declarações a um Serviço de Federação do AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Antes, você pode obter a interoperação com um aplicativo que é hospedado pelo AD FS 1. *x* declarações\-Agente Web com reconhecimento, você deve primeiro criar uma terceira parte confiável no serviço de Federação do AD FS no Windows Server 2012 para o AD FS 1. *x* declarações\-Agente Web com reconhecimento. **Observação:** Criando essa relação de confiança no serviço de Federação do AD FS é equivalente a adicionar um novo **Application** para o AD FS 1.x Federation Service \( **serviço de Federação\\diretiva de confiança\\ Minha organização\\Application**\). Essa terceira parte confiável é necessário porque o AD FS não tem um equivalente **Application** nó no seu próprio snap\-no. No entanto, ele ainda deve ter um canal seguro para o aplicativo.<br /><br />Quando você configura a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no Assistente de adição de terceira parte confiável terceiros de confiança para configurar essa relação de confiança para interoperar com um AD FS 1. *x* declarações\-Agente Web com reconhecimento:<br /><br />1.  Sobre o **Selecionar fonte de dados** , selecione **inserir os dados sobre a terceira parte confiável de terceiros de confiança manualmente**.<br />2.  Sobre o **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3.  No **configurar URL do** página, em **WS\-URL f PRP**, tipo o **URL do aplicativo** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4.  Sobre o **configurar identificadores** página em **identificador de relação de confiança de partes de terceira parte confiável**, tipo o **URL do aplicativo** conforme definido no AD FS, 1. *x* declarações\-Agente Web com reconhecimento|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar um Relying Party Trust Manually](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|Entre em contato com o administrador do servidor Web em execução do AD FS 1. *x* declarações\-ciente da Web agente e ter o administrador edite o arquivo Web. config que está associado com as declarações\-aplicativo com reconhecimento de \(sob o Site padrão em informações da Internet Os serviços \(IIS\) \) para apontar o agente da Web no serviço de Federação do AD FS.<br /><br />Por exemplo, substitua *myresourcefederationserver* na marca `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` do arquivo Web. config com um nome válido de servidor de Federação do AD FS.<br /><br />Isso é necessário para o aplicativo e as declarações do AD FS 1.x\-Agente Web com reconhecimento de ser capaz de consumir as declarações que são enviadas do serviço de Federação do AD FS no Windows Server 2012.|N\/A|  
|![Configurar o AD FS para enviar declarações](media/icon_checkboxo.gif)|A terceira parte confiável que você criou anteriormente, você precisa criar regras de declaração que serão levar as declarações de entrada que foram extraídas de um repositório de atributos e passar, filtrar ou transformá-los em um tipo de declaração de ID de nome que pode ser entendido e consumido pelo O AD FS 1. *x* declarações\-Agente Web com reconhecimento. **Observação:** Antes de criar essa regra, certifique-se de que o conjunto de regras de declaração em que você está criando essa regra tem uma regra que vem antes de ele que extrai primeiro um Lightweight Directory Access Protocol \(LDAP\) declaração de atributo de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x*\-declaração compatível com. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [crie uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar o AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1.x declaração compatível com](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

