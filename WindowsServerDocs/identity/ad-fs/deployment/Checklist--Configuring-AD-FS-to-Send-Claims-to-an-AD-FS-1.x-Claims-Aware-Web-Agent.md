---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: "Implantando servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: Configurar o AD FS para enviar solicitações para um agente de Web com reconhecimento de declarações do AD FS 1. x

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de verificação: Configurar o AD FS para enviar solicitações para um agente de Web claims\ ciente do AD FS 1. x  
Esta lista de verificação inclui as tarefas que são necessárias para configurar seus serviços de Federação do Active Directory \(AD FS\) serviço de federação no Windows Server 2012 para enviar declarações que podem ser entendidas por um aplicativo que está hospedado por um servidor Web executando o AD FS 1. *x* claims\ reconhecimento agente de Web.  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um procedimento, retorne para este tópico depois de concluir as etapas no procedimento para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: configurar o AD FS para enviar solicitações para um agente de Web claims\ ciente do AD FS 1. x**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Planejar a interoperabilidade entre o AD FS no Windows Server 2012 e versões anteriores do AD FS e saiba que mais sobre a ID do nome do tipo de declaração.|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planejamento para interoperabilidade com o AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Se você não tiver feito isso, use o link à direita para primeiro, crie uma terceira parte relação de confiança entre o serviço de Federação do AD FS no Windows Server 2012 e o AD FS 1. *x* serviço de Federação.|[Lista de verificação: Configurar o AD FS para enviar solicitações a um serviço de Federação do AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Antes, você pode conseguir interoperação com um aplicativo que está hospedado pelo AD FS 1. *x* claims\ reconhecimento agente de Web, você deve primeiro criar uma terceira confiança de terceiros no serviço de Federação AD FS no Windows Server 2012 para o AD FS 1. *x* claims\ reconhecimento agente de Web. **Observação:** criar esta relação de confiança no serviço de Federação AD FS é o equivalente da adição de um novo **aplicativo** para o AD FS 1. x serviço de Federação \ (**federação Service\\Trust Policy\\My Organization\\Application**\). Essa terceira confiança de terceiros é necessária porque o AD FS não tem um equivalente **aplicativo** nó no seu próprio snap\-in. No entanto, ele ainda deve ter um canal seguro para o aplicativo.<br /><br />Quando você configura a relação de confiança usando o procedimento no link para a direita, você deve fazer o seguinte no Adicionar dependência terceiros confiança Assistente para configurar esta relação de confiança para interoperar com um 1 do AD FS. *x* claims\ reconhecimento agente de Web:<br /><br />1. Diante o **Selecionar fonte de dados**, selecione **inserir os dados sobre a dependência de terceiros confiança manualmente**.<br />2. Diante do **Escolher perfil** página, selecione **perfil do AD FS 1.0 e 1.1**.<br />3. Na **configurar URL** página, em **WS\ federação passiva URL**, tipo a **URL do aplicativo** conforme definido no AD FS 1. *x* serviço de Federação do parceiro.<br />4. Pelo **configurar identificadores** página, em **identificador de confiança parte Relying**, tipo a **URL do aplicativo** conforme definido no AD FS 1. *x* claims\ reconhecimento agente de Web|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma dependência terceiros confiar manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Contate o administrador do servidor Web que executa o AD FS 1. *x* claims\ ciente da Web agente e ter esse administrador editar o arquivo Web. config que está associado com o aplicativo com reconhecimento de claims\ \ (sob o site padrão no Internet Information Services \(IIS\)\) para apontar o agente da Web para o serviço de Federação do AD FS.<br /><br />Por exemplo, substituir *myresourcefederationserver* na marca `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>`do arquivo Web. config com um nome de servidor de Federação do AD FS válido.<br /><br />Isso é necessário para o aplicativo e o agente de Web claims\ ciente do AD FS 1. x ser capazes de consumir as declarações são enviadas a ela de serviço de Federação do AD FS no Windows Server 2012.|N\/A|  
|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/icon_checkboxo.gif)|Na terceira confiança de terceiros que você criou anteriormente, você precisa criar reivindicação regras que serão tirar declarações de entrada que foram extraídas de uma loja de atributo e passagem, filtrar ou transformá-las em uma ID de nome reivindicar tipo que pode ser entendido e consumido pelo AD FS 1. *x* claims\ reconhecimento agente de Web. **Observação:** antes de criar essa regra, certifique-se de que o conjunto de regras de declaração onde você estiver criando essa regra tem uma regra que vem antes que extrai primeiro uma reivindicação de atributo Lightweight Directory Access Protocol \(LDAP\) de um repositório de atributo. Essa declaração será usada como entrada para a regra que você cria para enviar um AD FS 1. *x*\-compatible reivindicação. Para obter mais informações sobre como criar uma regra para extrair um atributo LDAP, consulte [criar uma regra para enviar LDAP atributos como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![Configurar o AD FS para enviar requerimentos judiciais ou Extrajudiciais](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[criar uma regra para enviar um AD FS 1. x reivindicação compatível](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

