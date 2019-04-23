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
ms.openlocfilehash: af227d9e87ddb73f194dd46c8ce45fcdf12a34cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872547"
---
# <a name="when-to-use-identity-delegation"></a>Quando usar a delegação de identidade

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="what-is-identity-delegation"></a>O que é a delegação de identidade?  
Delegação de identidade é um recurso dos serviços de Federação do Active Directory \(do AD FS\) que permite que o administrador\-especificado contas para representar usuários. A conta que representa o usuário é chamada de *Representante*. Essa capacidade de delegação é essencial para muitos aplicativos distribuídos para os quais há uma série de verificações de controle de acesso que devem ser feitas em sequência para cada aplicativo, banco de dados ou serviço que está na cadeia de autorização da solicitação de origem. Muitos real\-existem cenários do mundo em que um aplicativo Web "front-end" deve recuperar dados de um "back-end" mais seguro, como um serviço Web que está conectado a um banco de dados do Microsoft SQL Server.  
  
Por exemplo, um partes existentes\-ordenação no site da Web pode ser aprimorado por meio de programação para que ele permite que organizações parceiras exibir seu próprio status de histórico e a conta de compra. Por motivos de segurança, todos os dados financeiros do parceiro são armazenados em um banco de dados seguro em uma linguagem de consulta estruturada dedicado \(SQL\) server. Nessa situação, o código na parte frontal\-aplicativo end não sabe nada sobre dados financeiros da organização do parceiro. Portanto, ele deve recuperar esses dados de outro computador em outro lugar na rede que hospeda \(nesse caso\) o serviço Web para o banco de dados de partes \(back-end\).  
  
Para esses dados\-processo de recuperação tenha êxito, alguma sucessão de autorização "mão\-agitando" devem ser realizadas entre o aplicativo Web e o serviço Web para o banco de dados de partes, conforme mostrado na ilustração a seguir.  
  
![delegação de identidade](media/adfs2_identitydelegationconcept.gif)  
  
Já que a solicitação original foi feita para o próprio servidor Web, que pode estar localizado em uma organização completamente diferente da organização do usuário que está tentando acessar o servidor Web, o token de segurança que é enviado junto com a solicitação não atende aos critérios de autorização necessários para acessar qualquer outro computador além do servidor Web. Portanto, a única maneira pela qual a solicitação do usuário de origem pode ser atendida é colocando um servidor de federação intermediário na organização parceira de recursos para ajudar na reemissão de um token de segurança que tem privilégios de acesso adequados.  
  
## <a name="how-does-identity-delegation-work"></a>Como funciona a delegação de identidade?  
Muitas vezes, os aplicativos Web localizados em arquiteturas de aplicativos com várias camadas chamam serviços Web para acessar dados ou funcionalidades comuns. É importante que esses serviços Web saibam a identidade do usuário original para que o serviço possa tomar decisões de autorização e facilitar a auditoria. Nesse caso, o front\-end de aplicativo Web representa o usuário para o serviço Web como um delegado. O AD FS facilita esse cenário permitindo que as contas do Active Directory atuar como um usuário para outra terceira. Um cenário de delegação de identidade é exibido na ilustração a seguir.  
  
![delegação de identidade](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank tenta acessar a parte\-ordenação histórico de um aplicativo Web em outra organização. Seu computador cliente solicita e recebe um token do AD FS para a frente\-encerrar parte\-ordenação do aplicativo Web.  
  
2.  O computador cliente envia uma solicitação ao aplicativo Web, incluindo o token obtido na etapa 1, para provar a identidade do cliente.  
  
3.  O aplicativo Web precisa se comunicar com o serviço Web para concluir a sua transação para o cliente. O aplicativo Web entra em contato com o AD FS para obter um token de delegação para interagir com o serviço Web. Os tokens de delegação são tokens de segurança que são emitidos para que um representante atue como um usuário. O AD FS retorna um token de delegação com declarações sobre o cliente, direcionado para o serviço Web.  
  
4.  O aplicativo Web usa o token foi obtido do AD FS na etapa 3 para acessar o serviço Web que está atuando como o cliente. Examinando o token de delegação, o serviço Web pode determinar que o aplicativo Web está atuando como o cliente. O serviço Web executa sua política de autorização, registra a solicitação e fornece os dados necessários de histórico de peças que foram solicitados originalmente pelo Frank ao aplicativo Web e, portanto, ao Frank.  
  
Para um representante específico, o AD FS pode limitar os serviços da Web para o qual o aplicativo Web pode solicitar um token de delegação. O computador cliente não precisa ter uma conta do Active Directory para que essa operação seja bem-sucedida. Finalmente, conforme observado anteriormente, o serviço Web pode facilmente determinar a identidade do representante que está atuando como o usuário. Isso permite que os serviços Web exibam um comportamento diferente com base no fato de estarem se comunicando diretamente com o computador cliente ou por meio de um representante.  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>Configurando o AD FS para delegação de identidade  
Você pode usar o gerenciamento do AD FS snap\-para configurar o AD FS para delegação de identidade sempre que você precisa facilitar o processo de recuperação de dados. Depois de você configurá-lo, o AD FS pode gerar novos tokens de segurança que incluirão o contexto de autorização que o de volta\-serviço end pode exigir antes de poder fornecer acesso aos dados protegidos.  
  
O AD FS não restringe quais usuários podem ser representados. Depois de configurar o AD FS para delegação de identidade, faz o seguinte:  
  
-   Determina quais servidores podem ser delegados à autoridade de solicitar tokens para representar um usuário.  
  
-   Estabelece e separa o contexto de identidade da conta do cliente que é representada e o servidor que atua como um representante.  
  
Você pode configurar a delegação de identidade adicionando regras de autorização de delegação para uma terceira confiam no snap do gerenciamento do AD FS\-no. Para obter mais informações sobre como fazer isso, consulte [lista de verificação: Criando regras de declaração para a Relying Party Trust](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md).  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>Configurando a frente\-encerrar o aplicativo Web para delegação de identidade  
Os desenvolvedores têm várias opções que podem usar para programar devidamente frente Web\-encerrar o aplicativo ou serviço para redirecionar as solicitações de delegação para um computador do AD FS. Para obter mais informações sobre como personalizar um aplicativo Web para trabalhar com a delegação de identidade, consulte o [SDK Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
