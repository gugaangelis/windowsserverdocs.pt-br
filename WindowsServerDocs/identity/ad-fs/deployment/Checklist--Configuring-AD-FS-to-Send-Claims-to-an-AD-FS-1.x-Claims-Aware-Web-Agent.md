---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Como configurar o AD FS para enviar declarações a um Agente Web com reconhecimento de declarações do AD FS 1.x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 2303570097193ee564254357452784be28b29ac1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969763"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: configurando AD FS para enviar declarações a um Agente Web com reconhecimento de declaração do AD FS 1.x


## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de verificação: Configurar AD FS para enviar declarações para um agente Web com reconhecimento de declarações AD FS 1. x \-
Esta lista de verificação inclui as tarefas que são necessárias para configurar seu Serviços de Federação do Active Directory (AD FS) \( AD FS \) serviço de Federação no Windows Server 2012 para enviar declarações que podem ser compreendidas por um aplicativo hospedado por um servidor Web que executa o AD FS 1.* *Agente Web com reconhecimento de declarações x \- .

> [!NOTE]
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.

![Configurar AD FS para enviar a](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação de declarações: Configurando AD FS para enviar declarações para um \- Agente Web com reconhecimento de declarações AD FS 1. x**

|Tarefa|Referência|
|--------|-------------|
|Planeje a interoperabilidade entre AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba mais sobre o tipo de declaração ID de nome.|![Configurar AD FS para enviar o](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento de declarações para interoperabilidade com AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|Se você ainda não tiver feito isso, use o link à direita para primeiro criar uma relação de confiança de terceira parte confiável entre o AD FS Serviço de Federação no Windows Server 2012 e no AD FS 1. *x* serviço de Federação.|[Lista de verificação: Como configurar o AD FS para enviar declarações a um Serviço de Federação do AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|
|Antes de poder obter interoperação com um aplicativo hospedado pelo AD FS 1. Agente Web com reconhecimento de declarações *x* \- , você deve primeiro criar uma relação de confiança de terceira parte confiável no AD FS serviço de Federação no Windows Server 2012 para o AD FS 1. Agente Web com reconhecimento de declarações *x* \- . **Observação:** A criação dessa relação de confiança no AD FS Serviço de Federação é o equivalente a adicionar um novo **aplicativo** ao AD FS 1. x serviço de Federação \( **serviço de Federação \\ política de confiança \\ meu \\ aplicativo da organização** \) . Essa confiança de terceira parte confiável é necessária porque AD FS não tem um nó de **aplicativo** equivalente em seu próprio snap- \- in. No entanto, ele ainda deve ter um canal seguro para o aplicativo.<p>Ao configurar a relação de confiança usando o procedimento no link à direita, você deve fazer o seguinte no assistente Adicionar confiança de terceira parte confiável para configurar essa relação de confiança para interoperar com um AD FS 1. Agente Web com reconhecimento de declarações *x* \- :<p>1. na página **selecionar fonte de dados** , selecione **inserir dados sobre a confiança da terceira parte confiável manualmente**.<br />2. na página **escolher perfil** , selecione **AD FS perfil 1,0 e 1,1**.<br />3. na página **Configurar URL** , em ** \- URL passiva do WS Federation**, digite a **URL do aplicativo** , conforme definido no AD FS 1.* x* serviço de Federação do parceiro.<br />4. na página **Configurar identificadores** , em **identificador de confiança da parte confiável**, digite a **URL do aplicativo** , conforme definido no AD FS 1. Agente Web com reconhecimento de declarações *x* \-|![Configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma confiança de terceira parte confiável manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|
|Contate o administrador do servidor Web que executa o AD FS 1. *x* \- Agente Web com reconhecimento de declarações e fazer com que o administrador edite o arquivo de web.config associado ao aplicativo com reconhecimento de declarações no \- \( site padrão no serviços de informações da Internet \( \) \) o IIS para apontar o agente da Web no serviço de Federação de AD FS.<p>Por exemplo, substitua *myresourcefederationserver* na marca `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` do arquivo de web.config por um nome de servidor de Federação AD FS válido.<p>Isso é necessário para que o aplicativo e o agente da Web com reconhecimento de declarações do AD FS 1. x \- consigam consumir as declarações enviadas a ele do serviço de Federação de AD FS no Windows Server 2012.|N\/D|
|Na terceira parte confiável que você criou anteriormente, você precisa criar regras de declaração que usarão declarações de entrada que foram extraídas de um repositório de atributos e passarão, filtrarão ou transformarão em um tipo de declaração de ID de nome que pode ser compreendido e consumido pelo AD FS 1. Agente Web com reconhecimento de declarações *x* \- . **Observação:** Antes de criar essa regra, verifique se o conjunto de regras de declaração em que você está criando essa regra tem uma regra que vem antes de extrair primeiro uma \( declaração de atributo LDAP do protocolo de acesso ao diretório Lightweight \) de um repositório de atributos. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x* \- declaração compatível. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar AD FS para enviar declarações](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar uma declaração compatível com o AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|

