---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: "Quando usar a delegação de identidade"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-use-identity-delegation"></a>Quando usar a delegação de identidade

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>Qual é a delegação de identidade?  
Delegação de identidade é um recurso de serviços de Federação do Active Directory \(AD FS\) que permite que as contas especificadas administrator\ representar os usuários. A conta que representa o usuário é chamada a *delegar*. Essa funcionalidade de delegação é essencial para muitos aplicativos distribuídos para os quais há uma série de acesso verificações de controle que devem ser feitas sequencialmente para cada aplicativo, o banco de dados ou o serviço que está na cadeia de autorização da solicitação origem. Muitos cenários do mundo real\ existem em que um aplicativo "front-end Web" deve recuperar dados de um mais seguro "back-end", como um serviço Web que é conectado a um banco de dados do Microsoft SQL Server.  
  
Por exemplo, um site existente ordenação parts\ pode ser aperfeiçoado programaticamente para que ele permite que organizações parceiras exibir o status de histórico e conta sua própria compra. Por motivos de segurança, todos os dados financeiros de parceiros é armazenado em um banco de dados seguro em um servidor de linguagem de consulta estruturada \(SQL\) dedicado. Nessa situação, o código no aplicativo final front\ desconhece dados financeiros da organização de parceiro. Portanto, ele deve recuperar dados de outro computador em outro lugar na rede que hospeda \ (nesse case\) o serviço Web para o banco de dados de partes \(the back end\).  
  
Para esse processo de recuperação data\ ter sucesso, alguns sucessão de autorização "vibração hand\" deve assumir coloque entre o aplicativo Web e o serviço Web para o banco de dados de partes, conforme mostrado na ilustração a seguir.  
  
![delegação de identidade](media/adfs2_identitydelegationconcept.gif)  
  
Porque a solicitação original foi criada para o próprio servidor Web, que é provável que estejam localizados em uma organização completamente diferente da organização do usuário que está tentando acessar o servidor Web, o token de segurança que é enviado juntamente com a solicitação não atende aos critérios de autorização necessários para acessar qualquer outro computador além do servidor Web. Portanto, a única maneira que a solicitação origem do usuário pode ser atendida é colocando um servidor de Federação intermediária na organização do parceiro de recursos para ajudá-lo com reemissão um token de segurança que têm privilégios de acesso apropriados.  
  
## <a name="how-does-identity-delegation-work"></a>Como funciona a delegação de identidade?  
Aplicativos Web em arquiteturas de várias camadas de aplicativo geralmente chamam serviços Web para acessar dados ou funcionalidade comum. É importante para esses serviços Web saber a identidade do usuário original, para que o serviço possa tomar decisões de autorização e facilitar a auditoria. Nesse caso, o aplicativo da Web front\-end representa o usuário para o serviço Web como um representante. AD FS facilita nesse cenário, permitindo que contas do Active Directory atuar como um usuário para outra parte confiante. Um cenário de delegação de identidade é mostrado na ilustração a seguir.  
  
![delegação de identidade](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tenta acessar o histórico de pedidos part\ de um aplicativo da Web em outra organização. Seu computador cliente solicita e recebe um token do AD FS do aplicativo da Web de ordenação part\ front\-end.  
  
2.  O computador cliente envia uma solicitação ao aplicativo da Web, incluindo o token obtido na etapa 1, provar a identidade do cliente.  
  
3.  O aplicativo Web precisa se comunicar com o serviço Web para concluir sua transação para o cliente. O aplicativo Web contata o AD FS para obter um token de delegação para interagir com o serviço Web. Delegação tokens são tokens de segurança que são emitidas para um representante para atuar como um usuário. AD FS retorna um token de delegação com declarações sobre o cliente, direcionado para o serviço Web.  
  
4.  O aplicativo Web usa o token que foi obtido do AD FS na etapa 3 para acessar o serviço Web que está atuando como o cliente. Examinar o token de delegação, o serviço Web pode determinar que o aplicativo Web está atuando como o cliente. O serviço Web executa sua política de autorização, registra a solicitação e fornece as partes necessárias dados de histórico que foi originalmente solicitados pelo Frank para o aplicativo Web e, portanto, a Francisco.  
  
Para um representante específico, o AD FS pode limitar os serviços Web para o qual o aplicativo Web pode solicitar um token de delegação. O computador cliente não ter uma conta do Active Directory para esta operação ser bem-sucedida. Por fim, conforme observado anteriormente, o serviço Web pode determinar facilmente a identidade do delegado que está atuando como o usuário. Isso permite que os serviços Web para demonstrar um comportamento diferente com base no que eles estão falando diretamente para o computador cliente ou por meio de um representante.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurando o AD FS para delegação de identidade  
Você pode usar o AD FS snap\-in Gerenciamento para configurar o AD FS para delegação de identidade sempre que você precisa para facilitar o processo de recuperação de dados. Depois que você configurá-la, o AD FS pode gerar novos tokens de segurança que incluirão o contexto de autorização que o serviço back\-end pode exigir antes que ele pode fornecer acesso aos dados protegidos.  
  
AD FS não restringe quais usuários podem ser representados. Depois de configurar o AD FS para delegação de identidade, faz o seguinte:  
  
-   Ela determina quais servidores podem ser recebidos a autoridade para solicitar tokens para representar um usuário.  
  
-   Ele estabelece e mantém separado o contexto de identidade da conta do cliente que é recebida e o servidor de que age como um representante.  
  
Você pode configurar delegação de identidade, adicionando as regras de autorização de delegação para uma terceira confiança de terceiros no AD FS gerenciamento snap\-in. Para saber mais sobre como fazer isso, consulte [lista de verificação: criação de regras de declaração para confiar terceiros uma dependência](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configurando o aplicativo da Web front\-end para delegação de identidade  
Os desenvolvedores têm várias opções que eles podem usar para programar adequadamente o aplicativo de front\-end da Web ou serviço para redirecionar solicitações de delegação para um computador do AD FS. Para obter mais informações sobre como personalizar um aplicativo da Web para trabalhar com a delegação de identidade, consulte o [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
