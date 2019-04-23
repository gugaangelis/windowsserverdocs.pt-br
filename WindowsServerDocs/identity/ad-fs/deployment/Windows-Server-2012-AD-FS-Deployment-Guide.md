---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guia de Implantação do AD FS do Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882437"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guia de Implantação do AD FS do Windows Server 2012

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os serviços de Federação do Active Directory® \(do AD FS\) com o sistema de operacional Windows Server® 2012 para criar uma solução de gerenciamento de identidade federada que estenda identificação distribuída, autenticação, e Serviços de autorização para Web\-aplicativos baseados em toda a organização e limites de plataforma. Ao implantar o AD FS, você pode estender recursos de gerenciamento de identidade existentes da sua organização com a Internet.  
  
Você pode implantar o AD FS para:  
  
-   Fornecer aos funcionários ou clientes com uma Web\-único com base,\-sinal\-na \(SSO\) experiência quando eles precisarem de acesso remoto para internamente hospedado sites ou serviços.  
  
-   Fornecer aos funcionários ou clientes com uma Web\-com base, experiência de logon único quando eles acessarem cruzada\-organizacional de sites ou serviços a partir dos firewalls da sua rede.  
  
-   Fornecer aos funcionários ou clientes acesso contínuo à Web\-com base em recursos em qualquer organização de parceiro de federação na Internet sem a necessidade de funcionários ou clientes façam logon mais de uma vez.  
  
-   Manter o controle total sobre as identidades de seus funcionários ou clientes sem o uso de outro sinal\-nos provedores \(Windows Live ID, Liberty Alliance e outros\).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia destina-se para uso por administradores e engenheiros de sistema. Ele fornece orientações detalhadas para implantar um design do AD FS que tenha sido pré-selecionado por você ou um arquiteto de sistema ou especialista em infraestrutura em sua organização.  
  
Se um design ainda não tiver sido selecionado, recomendamos que você aguarde para seguir as instruções deste guia até depois de revisar as opções de design na [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) e você tiver selecionado o máximo design apropriado para sua organização. Para obter mais informações sobre como usar este guia com um design que já foi selecionado, consulte [implementando seu plano de Design do AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Depois de selecionar o design da guia de design e coletar as informações necessárias sobre declarações, tipos de token, repositórios de atributos e outros itens, você pode usar este guia para implantar seu design do AD FS no seu ambiente de produção. Este guia fornece etapas para implantar qualquer um dos seguintes designs do AD FS primários:  
  
-   SSO da Web  
  
-   SSO da Web federado  
  
Use as listas de verificação [implementando seu plano de Design do AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar a melhor maneira de usar as instruções neste guia para implantar seu design específico. Para obter informações sobre os requisitos de hardware e software para implantar o AD FS, consulte o [apêndice a: Revisar os requisitos do AD FS](https://technet.microsoft.com/library/ff678034.aspx) no AD FS Design Guide.  
  
### <a name="what-this-guide-does-not-provide"></a>O que este guia não contém  
O que este guia não contém:  
  
-   Orientações sobre quando e onde colocar servidores de federação, proxies de servidor de Federação ou servidores Web em sua infraestrutura de rede existente. Para obter essas informações, consulte [planejamento de posicionamento do servidor de Federação](https://technet.microsoft.com/library/dd807069.aspx) e [posicionamento de Proxy do servidor de Federação planejamento](https://technet.microsoft.com/library/dd807130.aspx) no guia de Design do AD FS.  
  
-   Diretrizes para usar autoridades de certificação \(CAs\) para configurar o AD FS  
  
-   Diretrizes para instalação ou configuração da Web específico\-com base em aplicativos  
  
-   Instruções de configuração específicas para configurar um ambiente de laboratório de teste.  
  
-   Informações sobre como personalizar telas de logon federadas, arquivos web.config ou o banco de dados de configuração.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Planejando a implantação do AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Plano de Design de implementação do AD FS](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Lista de verificação: Implementando um Design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de verificação: Implementando um Design SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configurando a organizações de parceiros](Configuring-Partner-Organizations.md)  
  
-   [Configurando regras de declaração](Configuring-Claim-Rules.md)  
  
-   [Implantando servidores de Federação](Deploying-Federation-Servers.md)  
  
-   [Implantação de Proxies de servidor de Federação](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperação com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
