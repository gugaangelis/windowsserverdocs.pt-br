---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Farm de servidores de federação usando WID
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4cfdfbda3a1c7df19ded887cc748e7f69f2fad08
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864058"
---
# <a name="legacy-ad-fs-federation-server-farm-using-wid"></a>Farm de servidores de Federação AD FS herdados usando WID

A topologia padrão para Serviços de Federação do Active Directory (AD FS) \( AD FS \) é um farm de servidores de Federação, usando o wid do banco de dados interno do Windows \( \) . Nessa topologia, AD FS usa WID como o repositório para o banco de dados de configuração de AD FS para todos os servidores de Federação que ingressaram nesse farm. O farm replica e mantém os dados do Serviço de Federação no banco de dados de configuração em cada servidor no farm. AD FS no Windows Server 2012 R2 permite que organizações com 100 ou menos confianças de terceira parte confiável configurem farms de servidores de Federação usando o WID com até 30 servidores.

O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação. Quando você usa o WID para o banco de dados de configuração do AD FS, o primeiro servidor de Federação que você cria no farm é chamado de *servidor de Federação primário*. Isso significa que esse computador está configurado com uma \/ cópia de leitura/gravação do banco de dados de configuração do AD FS.

Todos os outros servidores de Federação configurados para esse farm são chamados de *servidores de Federação secundários* porque eles devem replicar quaisquer alterações feitas no servidor de Federação primário para as \- cópias somente leitura do banco de dados de configuração de AD FS que eles armazenam localmente.

> [!IMPORTANT]
> Recomendamos o uso de pelo menos dois servidores de Federação em uma \- configuração de balanceamento de carga.

## <a name="deployment-considerations"></a>Considerações de implantação
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.

### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?

- Organizações com 100 ou menos relações de confiança configuradas que precisam fornecer seus usuários internos conectados \( a computadores que estão conectados fisicamente à rede corporativa \) com acesso SSO de logon único \- \( \) a aplicativos ou serviços federados

- Organizações que desejam fornecer aos usuários internos acesso de SSO ao Microsoft Online Services ou Microsoft Office 365

- Organizações menores que exigem serviços redundantes e escalonáveis

> [!NOTE]
> Organizações com bancos de dados maiores devem considerar o uso do [farm de servidores de Federação usando SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologia de implantação. As organizações com usuários que fazem logon de fora da rede devem considerar o uso do [farm de servidores de Federação usando a topologia wid e proxies](Federation-Server-Farm-Using-WID-and-Proxies.md) ou o farm de servidores de [Federação usando](Federation-Server-Farm-Using-SQL-Server.md) a topologia SQL Server.

### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?

- Fornece acesso de SSO a usuários internos

- Redundância de dados e Serviço de Federação \( cada servidor de Federação Replica as alterações para outros servidores de Federação no mesmo farm\)

- O WID está incluído no Windows; Portanto, não é necessário comprar SQL Server

### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?

- Um farm WID tem um limite de 30 servidores de Federação se você tiver 100 ou menos confianças de terceira parte confiável.

- Um farm WID não dá suporte à detecção de reprodução de token ou \( parte da resolução de artefatos do \( protocolo SAML Security Assertion Markup Language \) \) .

A tabela a seguir fornece um resumo para usar um farm WID. Use-o para planejar sua implementação.

| 1 a 100 relações de confiança de RP | Mais de 100 relações de confiança de RP |
|--|--|
| **1 a 30 nós do AD FS:** Suporte ao WID | **1 a 30 nós do AD FS:** Sem suporte para uso do WID – O SQL é exigido |
| **Mais de 30 Nós do AD FS:** Sem suporte para uso do WID – O SQL é exigido | **Mais de 30 Nós do AD FS:** Sem suporte para uso do WID – O SQL é exigido |


## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor
Quando estiver pronto para iniciar a implantação dessa topologia em sua rede, você deve planejar colocar todos os servidores de Federação em sua rede corporativa atrás de um host NLB de balanceamento de carga de rede \( \) que possa ser configurado para um cluster NLB com um nome DNS do sistema de nome de domínio do cluster dedicado \( \) e endereço IP do cluster.

> [!NOTE]
> Esse nome DNS do cluster deve corresponder ao nome do Serviço de Federação, por exemplo, fs.fabrikam.com.

O host NLB pode usar as configurações definidas nesse cluster NLB para alocar solicitações de cliente para os servidores de Federação individuais. A ilustração a seguir mostra como a empresa fictícia Fabrikam, Inc., configura a primeira fase de sua implantação usando um farm de servidores de Federação de dois \- computadores \( FS1 e FS2 \) com wid e o posicionamento de um servidor DNS e um único host NLB com conexão com a rede corporativa.

![farm de servidores usando WID](media/FarmWID.gif)

> [!NOTE]
> Se houver uma falha neste host NLB único, os usuários não poderão acessar aplicativos ou serviços federados. Adicione mais hosts NLB se seus requisitos comerciais não permitirem a existência de um único ponto de falha.

Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação, consulte a seção requisitos de resolução de nomes em [requisitos de AD FS](AD-FS-Requirements.md).

## <a name="see-also"></a>Consulte Também
[Planejar sua topologia](Plan-Your-AD-FS-Deployment-Topology.md) 
 de implantação do AD FS [Guia de design de AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)
