---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guia de Implantação do AD FS do Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a995cf956a6416a01f40c468b4e5d339609a6fb7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940850"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guia de Implantação do AD FS do Windows Server 2012


Você pode usar Active Directory &reg; Serviços \( de Federação AD FS \) com o &reg; sistema operacional Windows Server 2012 para criar uma solução de gerenciamento de identidades federadas que estenda os serviços de identificação, autenticação e autorização distribuídos para aplicativos baseados na Web \- entre os limites da organização e da plataforma. Ao implantar AD FS, você pode estender os recursos de gerenciamento de identidade existentes da sua organização para a Internet.

Você pode implantar o AD FS para:

-   Forneça aos seus funcionários ou clientes uma \- experiência de SSO de logon único com base na Web \- \- \( \) quando eles precisarem de acesso remoto a sites ou serviços hospedados internamente.

-   Forneça aos seus funcionários ou clientes uma \- experiência de SSO baseada na Web quando eles acessarem \- sites ou serviços da organização cruzada de dentro dos firewalls da sua rede.

-   Forneça aos seus funcionários ou clientes acesso contínuo a \- recursos baseados na Web em qualquer organização de parceiro de Federação na Internet sem exigir que os funcionários ou clientes façam logon mais de uma vez.

-   Mantenha o controle completo sobre suas identidades de funcionário ou cliente sem usar outros \- provedores de logon do \( Windows Live ID, do Liberty Alliance e outros \) .

## <a name="about-this-guide"></a>Sobre este guia
Este guia destina-se para uso por administradores e engenheiros de sistema. Ele fornece diretrizes detalhadas para a implantação de um design de AD FS que foi previamente selecionado por você ou por um especialista em infraestrutura ou arquiteto de sistema em sua organização.

Se um design ainda não tiver sido selecionado, recomendamos que você aguarde para seguir as instruções neste guia até ter revisado as opções de design no [Guia de design de AD FS no Windows Server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md) e tiver selecionado o design mais apropriado para sua organização. Para obter mais informações sobre como usar este guia com um design que já foi selecionado, consulte [implementando seu plano de design de AD FS](Implementing-Your-AD-FS-Design-Plan.md).

Depois de selecionar o design no guia de design e reunir as informações necessárias sobre declarações, tipos de token, repositórios de atributos e outros itens, você pode usar este guia para implantar seu design de AD FS em seu ambiente de produção. Este guia fornece as etapas para implantar um dos seguintes designs de AD FS primários:

-   SSO da Web

-   SSO da Web federado

Use as listas de verificação em [implementando seu plano de design de AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar a melhor maneira de usar as instruções neste guia para implantar seu design específico. Para obter informações sobre os requisitos de hardware e software para a implantação de AD FS, consulte o [Apêndice a: revisar requisitos de AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11)) no guia de Design de AD FS.

### <a name="what-this-guide-does-not-provide"></a>O que este guia não contém
O que este guia não contém:

-   Orientação sobre quando e onde posicionar servidores de Federação, proxies de servidor de Federação ou servidores Web em sua infraestrutura de rede existente. Para obter essas informações, consulte planejando o posicionamento do [servidor de Federação](../design/planning-federation-server-placement.md) e planejando o posicionamento do proxy do servidor de [Federação](../design/planning-federation-server-proxy-placement.md) no guia de design do AD FS.

-   Diretrizes para usar CAS de autoridades de certificação \( \) para configurar AD FS

-   Diretrizes para configurar ou configurar aplicativos específicos baseados na Web \-

-   Instruções de configuração específicas para configurar um ambiente de laboratório de teste.

-   Informações sobre como personalizar telas de logon federadas, arquivos web.config ou o banco de dados de configuração.

## <a name="in-this-guide"></a>Neste guia

-   [Como planejar a implantação do AD FS](Planning-to-Deploy-AD-FS.md)

-   [Como implementar seu plano de design do AD FS](Implementing-Your-AD-FS-Design-Plan.md)

-   [Lista de verificação: Como implementar um design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)

-   [Lista de verificação: Como implementar um design de SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)

-   [Configurando organizações parceiras](Configuring-Partner-Organizations.md)

-   [Como configurar regras de declaração](Configuring-Claim-Rules.md)

-   [Como implantar servidores de federação](Deploying-Federation-Servers.md)

-   [Como implantar proxies de servidor de federação](Deploying-Federation-Server-Proxies.md)

-   [Como interoperar com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)
