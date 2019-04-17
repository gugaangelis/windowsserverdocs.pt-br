---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: "Guia de implantação do Windows Server 2012 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guia de implantação do Windows Server 2012 AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar serviços de Federação do Active Directory® \(AD FS\) com o sistema operacional Windows Server® 2012 para criar uma solução de gerenciamento de identidade federada que estende identificação distribuída, autenticação e autorização serviços para aplicativos baseados em Web\ organização e limites de plataforma. Implantando o AD FS, você pode estender a recursos de gerenciamento de identidade existentes da sua organização com a Internet.  
  
Você pode implantar o AD FS:  
  
-   Fornece seus funcionários ou clientes uma experiência \(SSO\) com base em Web\, sign\ single\ no quando precisar de acesso remoto para sites ou serviços hospedados internamente.  
  
-   Fornece seus funcionários ou clientes uma experiência SSO baseado em Web\ ao acessarem cross\ organizacional sites ou serviços de dentro dos firewalls da sua rede.  
  
-   Fornece seus funcionários ou clientes acesso perfeito a recursos baseados em Web\ em qualquer organização de parceiros de federação na Internet sem a necessidade de funcionários ou clientes fazer logon mais de uma vez.  
  
-   Manter controle total sobre suas identidades de funcionários ou clientes sem o uso de outros provedores de sign\ \ (Windows Live ID, Liberty Alliance e others\).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia destina-se ao uso por administradores do sistema e os engenheiros de sistema. Ele fornece orientações detalhadas para implantar um design AD FS que tenha sido pré-selecionado por você ou infraestrutura especialista ou sistema arquiteto em sua organização.  
  
Se um design ainda não tiver sido selecionado, é recomendável esperar para siga as instruções neste guia até depois de analisarmos as opções de design no [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) e você selecionou o design mais adequado para sua organização. Para obter mais informações sobre como usar este guia com um design que já foi selecionado, consulte [implementar seu plano de Design do AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Depois de selecionar o design do guia de design e coletar as informações necessárias sobre declarações, tipos de token, atributo lojas e outros itens, você pode usar este guia para implantar seu design do AD FS no ambiente de produção. Este guia apresenta etapas para implantar qualquer um dos seguintes designs do AD FS principais:  
  
-   SSO da Web  
  
-   SSO da Web federado  
  
Use as listas de verificação no [implementar seu plano de Design do AD FS](Implementing-Your-AD-FS-Design-Plan.md) para determinar a melhor maneira de usar as instruções neste guia para implantar seu design específico. Para obter informações sobre os requisitos de hardware e software para a implantação do AD FS, consulte o [apêndice a: examinando o AD FS requisitos](https://technet.microsoft.com/library/ff678034.aspx) no guia de Design do AD FS.  
  
### <a name="what-this-guide-does-not-provide"></a>O que este guia não fornece  
Este guia não fornece:  
  
-   Orientação sobre quando e onde colocar os servidores de federação, proxies do servidor de Federação ou servidores Web em sua infraestrutura de rede existente. Para essas informações, consulte [planejamento de posicionamento de servidor de Federação](https://technet.microsoft.com/library/dd807069.aspx) e [planejamento de posicionamento de Proxy de servidor de Federação](https://technet.microsoft.com/library/dd807130.aspx) no guia de Design do AD FS.  
  
-   Orientações para usar certificação autoridades \(CAs\) para configurar o AD FS  
  
-   Diretrizes para configurar ou configurar aplicativos específicos com base em Web\  
  
-   Instalação instruções que são específicas para configurar um ambiente de laboratório de teste.  
  
-   Informações sobre como personalizar telas de logon federado, arquivos da Web. config ou o banco de dados de configuração.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Planejar a implantação do AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implementando o AD FS elaborar o plano](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Lista de verificação: Implementando um Design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de verificação: Implementando um Design SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configurando organizações parceiras](Configuring-Partner-Organizations.md)  
  
-   [Configurando as regras de declaração](Configuring-Claim-Rules.md)  
  
-   [Implantando servidores de Federação](Deploying-Federation-Servers.md)  
  
-   [Implantando Proxies de servidor de Federação](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperando com o AD FS 1. x](Interoperating-with-AD-FS-1.x.md)  
