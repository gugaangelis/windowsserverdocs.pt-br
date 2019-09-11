---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: Quando usar a delegação de identidade
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d69c92209a2998cf2c249b865ad8d16424735ca
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867575"
---
# <a name="when-to-use-identity-delegation"></a>Quando usar a delegação de identidade
  
## <a name="what-is-identity-delegation"></a>O que é a delegação de identidade?  
A delegação de identidade é um recurso \(do\) serviços de Federação do Active Directory (AD FS) AD FS que\-permite que contas especificadas pelo administrador representem usuários. A conta que representa o usuário é chamada de *Representante*. Essa capacidade de delegação é essencial para muitos aplicativos distribuídos para os quais há uma série de verificações de controle de acesso que devem ser feitas em sequência para cada aplicativo, banco de dados ou serviço que está na cadeia de autorização da solicitação de origem. Muitos cenários\-do mundo real existem nos quais um "front-end" de aplicativo Web deve recuperar dados de um "back-end" mais seguro, como um serviço Web conectado a um banco de dados Microsoft SQL Server.  
  
Por exemplo, um site de\-ordenação de partes existentes pode ser aprimorado programaticamente para permitir que as organizações parceiras exibam seu próprio histórico de compras e o status da conta. Por motivos de segurança, todos os dados financeiros do parceiro são armazenados em um banco de dado \(seguro\) em um SQL Server dedicado linguagem SQL. Nessa situação, o código no aplicativo de front\--end não sabe nada sobre os dados financeiros da organização parceira. Portanto, ele deve recuperar esses dados de outro computador em outra parte na rede que \(hospeda, nesse\) caso, o serviço Web para o \(banco de dados\)de partes do back-end.  
  
Para que esse\-processo de recuperação de dados seja bem-sucedido, é necessário que ocorra uma\-certa sucessão de autorização "incerto" entre o aplicativo Web e o serviço Web para o banco de dados de partes, conforme mostrado na ilustração a seguir.  
  
![Delegação de identidade](media/adfs2_identitydelegationconcept.gif)  
  
Já que a solicitação original foi feita para o próprio servidor Web, que pode estar localizado em uma organização completamente diferente da organização do usuário que está tentando acessar o servidor Web, o token de segurança que é enviado junto com a solicitação não atende aos critérios de autorização necessários para acessar qualquer outro computador além do servidor Web. Portanto, a única maneira pela qual a solicitação do usuário de origem pode ser atendida é colocando um servidor de federação intermediário na organização parceira de recursos para ajudar na reemissão de um token de segurança que tem privilégios de acesso adequados.  
  
## <a name="how-does-identity-delegation-work"></a>Como funciona a delegação de identidade?  
Muitas vezes, os aplicativos Web localizados em arquiteturas de aplicativos com várias camadas chamam serviços Web para acessar dados ou funcionalidades comuns. É importante que esses serviços Web saibam a identidade do usuário original para que o serviço possa tomar decisões de autorização e facilitar a auditoria. Nesse caso, o aplicativo Web\-de front-end representa o usuário para o serviço Web como um delegado. AD FS facilita esse cenário, permitindo que Active Directory contas atuem como um usuário para outra terceira parte confiável. Um cenário de delegação de identidade é exibido na ilustração a seguir.  
  
![Delegação de identidade](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tenta acessar o histórico\-de pedidos de parte de um aplicativo Web em outra organização. Seu computador cliente solicita e recebe um token de AD FS para o aplicativo\-Web de\-ordenação de peças de front-end.  
  
2.  O computador cliente envia uma solicitação para o aplicativo Web, incluindo o token obtido na etapa 1, para provar a identidade do cliente.  
  
3.  O aplicativo Web precisa se comunicar com o serviço Web para concluir a sua transação para o cliente. O aplicativo Web entra em contato AD FS para obter um token de delegação para interagir com o serviço Web. Os tokens de delegação são tokens de segurança que são emitidos para que um representante atue como um usuário. AD FS retorna um token de delegação com declarações sobre o cliente, direcionado para o serviço Web.  
  
4.  O aplicativo Web usa o token obtido de AD FS na etapa 3 para acessar o serviço Web que está agindo como o cliente. Examinando o token de delegação, o serviço Web pode determinar que o aplicativo Web está atuando como o cliente. O serviço Web executa sua política de autorização, registra a solicitação e fornece os dados necessários de histórico de peças que foram solicitados originalmente pelo Frank ao aplicativo Web e, portanto, ao Frank.  
  
Para um determinado delegado, AD FS pode limitar os serviços Web para os quais o aplicativo Web pode solicitar um token de delegação. O computador cliente não precisa ter uma conta do Active Directory para que essa operação seja bem-sucedida. Finalmente, conforme observado anteriormente, o serviço Web pode facilmente determinar a identidade do representante que está atuando como o usuário. Isso permite que os serviços Web exibam um comportamento diferente com base no fato de estarem se comunicando diretamente com o computador cliente ou por meio de um representante.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurando o AD FS para delegação de identidade  
Você pode usar o snap\--in Gerenciamento de AD FS para configurar AD FS para delegação de identidade sempre que precisar facilitar o processo de recuperação de dados. Depois de configurá-lo, AD FS pode gerar novos tokens de segurança que incluirão o contexto de\-autorização que o serviço de back-end pode exigir antes de poder fornecer acesso aos dados protegidos.  
  
AD FS não restringe quais usuários podem ser representados. Depois de configurar AD FS para delegação de identidade, ele faz o seguinte:  
  
-   Determina quais servidores podem ser delegados à autoridade de solicitar tokens para representar um usuário.  
  
-   Estabelece e separa o contexto de identidade da conta do cliente que é representada e o servidor que atua como um representante.  
  
Você pode configurar a delegação de identidade adicionando regras de autorização de delegação a uma terceira parte confiável no snap\--in de gerenciamento de AD FS. Para obter mais informações sobre como fazer isso, consulte [lista de verificação: Criando regras de declaração para uma relação de confiança](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)de terceira parte confiável.  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configurando\-o aplicativo Web de front-end para delegação de identidade  
Os desenvolvedores têm várias opções que podem usar para programar adequadamente o aplicativo ou\-serviço de front-end da Web para redirecionar solicitações de delegação para um computador AD FS. Para obter mais informações sobre como personalizar um aplicativo Web para trabalhar com a delegação de identidade, consulte o [SDK Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
