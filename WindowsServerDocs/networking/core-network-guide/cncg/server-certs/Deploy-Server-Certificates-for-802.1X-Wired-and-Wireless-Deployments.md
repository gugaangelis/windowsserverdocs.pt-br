---
title: Implantar certificados de servidor para 802.1 X implantações com e sem fio
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implantar certificados de servidor para 802.1 X implantações com e sem fio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este guia para implantar certificados de servidor para os servidores de infraestrutura de acesso remoto e o servidor de política de rede (NPS).   

Este guia contém as seguintes seções.  

-   [Pré-requisitos para usar este guia](#bkmk_pre)  

-   [O que este guia não fornece](#bkmk_not)  

-   [Visões gerais de tecnologia](#bkmk_tech)  

-   [Visão geral de implantação de certificado do servidor](Server-Certificate-Deployment-Overview.md)  

-   [Planejamento de implantação do certificado de servidor](Server-Certificate-Deployment-Planning.md)  

-   [Implantação do certificado de servidor](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>Certificados digitais de servidor**  
Este guia fornece instruções para usar os serviços de certificados do Active Directory (AD CS) para registrar certificados para acesso remoto e servidores de infraestrutura NPS automaticamente. AD CS permite que você crie uma infraestrutura de chave pública (PKI) e fornecer recursos de assinatura digital, certificados digitais e criptografia de chave pública para sua organização.  

Quando você usa certificados digitais de servidor de autenticação entre computadores em sua rede, os certificados fornecem:   

1. Confidencialidade por meio de criptografia.  
2. Integridade por meio de assinaturas digitais.  
3. Autenticação, associando as chaves de certificado a contas de computador, usuário ou dispositivo em uma rede do computador.  

### **<a name="server-types"></a>Tipos de servidor**  
Ao usar este guia, você pode implantar certificados de servidor para os seguintes tipos de servidores.  
- Servidores que executam o serviço de acesso remoto, que são o DirectAccess ou servidores de rede virtual privada (VPN) padrão e que são membros do **servidores RAS e IAS** grupo.  
- Servidores que executam o serviço de servidor de política de rede (NPS) que são membros do **servidores RAS e IAS** grupo.  

### **<a name="advantages-of-certificate-autoenrollment"></a>Vantagens de registro automático de certificado**  
Registro automático de certificados de servidor, também chamada de registro automático, oferece as seguintes vantagens.  

- A autoridade de certificação (CA) do AD CS registra automaticamente um certificado de servidor a todos os servidores NPS e acesso remoto.  
- Todos os computadores no domínio recebem automaticamente o certificado da CA, que é instalado em autoridades de certificação raiz confiáveis armazenem em cada computador membro do domínio. Por isso, todos os computadores no domínio confiam os certificados são emitidos pela autoridade de certificação. Esta relação de confiança permite que os servidores de autenticação provam suas identidades uns aos outros e se envolva em comunicações seguras.  
- A reconfiguração manual de todos os servidores que não sejam de atualizar a política de grupo, não é necessária.  
- Cada certificado do servidor inclui a finalidade de autenticação de servidor e a finalidade de autenticação de cliente em extensões de uso avançado de chave (EKU).  
- Escalabilidade. Depois de implantar a CA raiz da empresa com este guia, você pode expandir sua infraestrutura de chave pública (PKI) adicionando subordinadas empresarial.  
- Capacidade de gerenciamento. Você pode gerenciar o AD CS usando o console do AD CS ou usando scripts e comandos do Windows PowerShell.  
- Simplicidade. Especifique os servidores que certificados de servidor, usando contas de grupo do Active Directory e associação de grupo.   
- Quando você implanta certificados de servidor, os certificados são baseados em um modelo que você configurar com as instruções neste guia. Isso significa que você pode personalizar os modelos de certificado diferentes para tipos de servidor específico, ou você pode usar o mesmo modelo para todos os certificados de servidor que você deseja executar.  

## <a name="bkmk_pre"></a>Pré-requisitos para usar este guia  

Este guia fornece instruções sobre como implantar certificados de servidor usando o AD CS e a função de servidor de Web Server (IIS) no Windows Server 2016. A seguir estão os pré-requisitos para executar os procedimentos neste guia.  

- Você deve implantar uma rede principal usando o guia de rede do Windows Server 2016 Core ou você já deve ter as tecnologias fornecidas no guia de rede principal instalados e funcionando corretamente em sua rede. Essas tecnologias incluem TCP/IP v4, DHCP, serviços Active Directory domínio (AD DS), DNS e NPS.  
>[!NOTE]
>Guia de rede do Windows Server 2016 Core está disponível na biblioteca técnica do Windows Server 2016. Para obter mais informações, consulte [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).

- Você deve ler a seção de planejamento deste guia para garantir que você está preparado para essa implantação antes de executar a implantação.  
- Você deve executar as etapas neste guia na ordem em que eles são apresentados. Não vá direto e implantar a CA sem executar as etapas que iniciaram Implantando o servidor ou sua implantação falhará.  
- Você deve estar preparado para implantar dois novos servidores em sua rede, um servidor no qual você instalará o AD CS como uma CA raiz corporativa e um servidor no qual você instalará Web Server (IIS) para que a autoridade de certificação pode publicar a lista de certificados revogados (CRL) para o servidor Web.   

>[!NOTE]  
>Você está preparado para atribuir um endereço IP estático para os servidores Web e AD CS que você implante com este guia, bem como para nomear os computadores de acordo com seu convenções de nomenclatura de organização. Além disso, você deve associar os computadores ao domínio.  

## <a name="bkmk_not"></a>O que este guia não fornece  
Este guia não fornece instruções abrangentes para projetar e implantar uma infraestrutura de chave pública (PKI) usando o AD CS. É recomendável que você reveja a documentação do AD CS e documentação de design PKI antes de implantar as tecnologias neste guia.   

## <a name="bkmk_tech"></a>Visões gerais de tecnologia  
A seguir estão visões gerais de tecnologia para AD CS e Web Server (IIS).  

### <a name="active-directory-certificate-services"></a>Serviços de certificados do Active Directory  
AD CS no Windows Server 2016 fornece serviços personalizáveis para criar e gerenciar os certificados x. 509 que são usados em sistemas de segurança de software que utilizam tecnologias de chave pública. As organizações podem usar o AD CS para aprimorar a segurança, associando a identidade de uma pessoa, dispositivo ou serviço para uma chave pública correspondente. AD CS também inclui recursos que permitem que você gerencie o registro de certificado e revogação em uma variedade de ambientes escaláveis.  

Para obter mais informações, consulte [visão geral do Active Directory certificado serviços](https://technet.microsoft.com/library/hh831740.aspx) e [diretrizes de Design de infraestrutura de chave pública ](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Servidor Web (IIS)  

A função Web Server (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para hospedar confiavelmente sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, uma intranet ou uma extranet. IIS é uma plataforma web unificado que se integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).  

Para obter mais informações, consulte [visão geral da Web Server (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
