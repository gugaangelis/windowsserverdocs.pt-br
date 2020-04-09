---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Implantando servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 450364d7fe760276b0d8c47167f8e65559b6f93f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854129"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: configurando AD FS para enviar declarações a um Agente Web com reconhecimento de declaração do AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de verificação: Configurar AD FS para enviar declarações a um AD FS declarações 1. x\-Agente Web ciente  
Esta lista de verificação inclui as tarefas necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \(AD FS\) Serviço de Federação no Windows Server 2012 para enviar declarações que podem ser compreendidas por um aplicativo hospedado por um servidor Web que executa o AD FS 1. as declarações *x*\-o Agente Web ciente.  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![configurar AD FS para enviar a](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de declarações: configurar AD FS para enviar declarações para um AD FS declarações 1. x\-o Agente Web ciente**  
  
||{1&gt;Tarefa&lt;1}|Referência|  
|-|--------|-------------|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|![configurar AD FS para enviar o](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento de declarações para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Se você ainda não tiver feito isso, use o link à direita para primeiro criar uma relação de confiança de terceira parte confiável entre o AD FS Serviço de Federação no Windows Server 2012 e no AD FS 1. *x* serviço de Federação.|[Lista de verificação: Configurar AD FS para enviar declarações para um AD FS 1. x Serviço de Federação](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Antes de poder obter interoperação com um aplicativo hospedado pelo AD FS 1. *x* alega\-Agente Web ciente, você deve primeiro criar uma relação de confiança de terceira parte confiável no Serviço de Federação de AD FS no Windows Server 2012 para o AD FS 1. as declarações *x*\-o Agente Web ciente. **Observação:** A criação dessa relação de confiança no AD FS Serviço de Federação é o equivalente à adição de um novo **aplicativo** à política de \(de confiança do AD FS 1. x serviço de Federação serviço de Federação\\\\**minha organização\\aplicativo**\). Essa confiança de terceira parte confiável é necessária porque AD FS não tem um nó de **aplicativo** equivalente em seu próprio snap\-no. No entanto, ele ainda deve ter um canal seguro para o aplicativo.<p>Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no assistente Adicionar confiança de terceira parte confiável para configurar essa relação de confiança para interoperar com um AD FS 1. solicitações *x*\-Agente Web ciente:<p>1. na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2. na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3. na página **Configurar URL** , em **URL passiva da federação do WS\-** , digite a **URL do aplicativo** , conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4. na página **Configurar identificadores** , em **identificador de confiança da parte confiável**, digite a **URL do aplicativo** , conforme definido no AD FS 1.\-do Agente Web com reconhecimento de declarações *x*|![configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma relação de confiança de terceira parte confiável manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Contate o administrador do servidor Web que executa o AD FS 1. as declarações *x*\-o Agente Web ciente e têm esse administrador para editar o arquivo Web. config associado ao aplicativo claims\-Aware \(no site padrão no serviços de informações da Internet \(IIS\)\) para apontar o Agente Web no AD FS serviço de Federação.<p>Por exemplo, substitua *myresourcefederationserver* na marca `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` do arquivo Web. config por um nome de servidor de Federação AD FS válido.<p>Isso é necessário para o aplicativo e AD FS declarações 1. x\-que o agente da Web ciente possa consumir as declarações enviadas a ele da Serviço de Federação de AD FS no Windows Server 2012.|N\/um|  
|![Configurar AD FS para enviar declarações](media/icon_checkboxo.gif)|Na terceira parte confiável que você criou anteriormente, você precisa criar regras de declaração que usarão declarações de entrada que foram extraídas de um repositório de atributos e passarão, filtrarão ou transformarão em um tipo de declaração de ID de nome que pode ser compreendido e consumido pelo AD FS 1. as declarações *x*\-o Agente Web ciente. **Observação:** Antes de criar essa regra, certifique-se de que o conjunto de regras de declaração em que você está criando essa regra tenha uma regra que venha antes de extrair primeiro um protocolo de acesso de diretório Lightweight \(declaração de atributo de\) LDAP de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. declaração compatível com *x*\-. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crie uma regra para enviar uma declaração compatível com o AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

