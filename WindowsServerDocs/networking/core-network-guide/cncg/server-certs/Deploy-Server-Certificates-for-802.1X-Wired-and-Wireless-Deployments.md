---
title: Implantar certificados de servidor para implantações com e sem fio do 802.1X
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 769a4165cfd82056a904c79c41e96fb666d05e43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842267"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implantar certificados de servidor para implantações com e sem fio do 802.1X

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este guia para implantar certificados de servidor em seus servidores de infraestrutura do acesso remoto e o servidor de diretivas de rede (NPS).   

Este guia contém as seções a seguir.  

-   [Pré-requisitos para usar este guia](#bkmk_pre)  

-   [O que este guia não contém](#bkmk_not)  

-   [Visões gerais de tecnologia](#bkmk_tech)  

-   [Visão geral da implantação de certificado do servidor](Server-Certificate-Deployment-Overview.md)  

-   [Planejar a implantação de certificado do servidor](Server-Certificate-Deployment-Planning.md)  

-   [Implantação de certificado do servidor](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**Certificados de servidor digital**  
Este guia fornece instruções para usar os serviços de certificados do Active Directory (AD CS) para registrar automaticamente certificados para acesso remoto e servidores de infraestrutura do NPS. AD CS permite que você criar uma infraestrutura de chave pública (PKI) e fornecer recursos de assinatura digital, certificados digitais e criptografia de chave pública para sua organização.  

Quando você usa certificados digitais de servidor para autenticação entre computadores na sua rede, os certificados fornecem:   

1. Confidencialidade por meio da criptografia.  
2. Integridade através de assinaturas digitais.  
3. Autenticação pela associação de chaves de certificado com as contas de computador, usuário ou dispositivo em uma rede do computador.  

### <a name="server-types"></a>**Tipos de servidor**  
Usando este guia, você pode implantar certificados de servidor para os seguintes tipos de servidores.  
- Servidores que estão executando o serviço de acesso remoto, que são servidores de padrão de rede virtual privada (VPN) ou DirectAccess, e que são membros do **servidores RAS e IAS** grupo.  
- Servidores que estão executando o serviço servidor de diretivas de rede (NPS) que são membros do **servidores RAS e IAS** grupo.  

### <a name="advantages-of-certificate-autoenrollment"></a>**Vantagens do registro automático de certificados**  
O registro automático de certificados do servidor, também chamado de registro automático, fornece as seguintes vantagens.  

- A autoridade de certificação (CA) do AD CS inscreve automaticamente um certificado de servidor para todos os seus servidores NPS e acesso remoto.  
- Todos os computadores no domínio recebem automaticamente o certificado de autoridade de certificação, que é instalado em autoridades de certificação raiz confiáveis armazenam em cada computador membro do domínio. Por isso, todos os computadores no domínio confiam em certificados emitidos pela autoridade de certificação. Essa relação de confiança permite que os servidores de autenticação para comprovar suas identidades uns aos outros e se envolva em comunicações seguras.  
- Diferente de atualização de diretiva de grupo, reconfiguração manual de todos os servidores não é necessária.  
- Todos os certificados de servidor inclui a finalidade de autenticação de servidor e a finalidade de autenticação de cliente nas extensões de uso avançado de chave (EKU).  
- Escalabilidade. Depois de implantar sua autoridade de certificação raiz corporativa com este guia, você pode expandir sua infraestrutura de chave pública (PKI) com a adição de autoridades de certificação subordinada corporativas.  
- Capacidade de gerenciamento. Você pode gerenciar o AD CS usando o console do AD CS ou usando scripts e comandos do Windows PowerShell.  
- Simplicidade. Você especifica os servidores que registram certificados de servidor usando contas de grupo do Active Directory e a associação de grupo.   
- Quando você implantar certificados de servidor, os certificados são com base em um modelo que você configura com as instruções neste guia. Isso significa que você pode personalizar modelos de certificado diferente para tipos específicos de servidores, ou você pode usar o mesmo modelo para todos os certificados de servidor que você deseja emitir.  

## <a name="bkmk_pre"></a>Pré-requisitos para usar este guia  

Este guia fornece instruções sobre como implantar certificados de servidor usando o AD CS e a função de servidor servidor Web (IIS) no Windows Server 2016. A seguir estão os pré-requisitos para executar os procedimentos neste guia.  

- Você deve implantar uma rede principal usando o guia de rede do Windows Server 2016 Core, ou você já deve ter as tecnologias fornecidas no guia da rede principal instaladas e funcionando corretamente em sua rede. Essas tecnologias incluem o TCP/IP v4, DHCP, Active Directory Domain Services (AD DS), DNS e NPS.  
>[!NOTE]
>Guia de rede do Windows Server 2016 Core está disponível na biblioteca técnica do Windows Server 2016. Para obter mais informações, consulte [guia da rede principal](../../../core-network-guide/Core-Network-Guide.md).

- Você deve ler a seção de planejamento deste guia para garantir que você esteja preparado para essa implantação antes de executar a implantação.  
- Você deve executar as etapas neste guia, na ordem em que eles são apresentados. Não vá em frente e implantar sua autoridade de certificação sem executando as etapas que levam ao implantar o servidor ou a implantação falhará.  
- Você deve estar preparado para implantar dois novos servidores na sua rede, um servidor no qual você instalará o AD CS como uma AC raiz corporativa e um servidor no qual você instalará servidor Web (IIS) para que sua autoridade de certificação pode publicar a lista de certificados revogados (CRL) para o Web se servido.   

>[!NOTE]  
>Você está preparado para atribuir um endereço IP estático para os servidores Web e do AD CS que você implanta com este guia, bem como para nomear computadores de acordo com suas convenções de nomenclatura da organização. Além disso, você deve unir computadores ao seu domínio.  

## <a name="bkmk_not"></a>O que este guia não fornece  
Este guia não fornece instruções abrangentes para projetar e implantar uma infraestrutura de chave pública (PKI) usando o AD CS. É recomendável que você examine a documentação do AD CS e documentação de design PKI antes de implantar as tecnologias neste guia.   

## <a name="bkmk_tech"></a>Visões gerais de tecnologia  
A seguir estão as visões gerais de tecnologia para AD CS e o servidor Web (IIS).  

### <a name="active-directory-certificate-services"></a>Serviços de Certificados do Active Directory  
O AD CS no Windows Server 2016 fornece serviços personalizáveis para criar e gerenciar os certificados x. 509 usados em sistemas de segurança de software que empregam tecnologias de chave pública. As organizações podem usar o AD CS para aumentar a segurança associando a identidade de uma pessoa, dispositivo ou serviço a uma chave pública correspondente. AD CS também inclui recursos que permitem que você gerencie o registro de certificado e a revogação em uma variedade de ambientes escalonáveis.  

Para obter mais informações, consulte [visão geral serviços de certificados do Active Directory](https://technet.microsoft.com/library/hh831740.aspx) e [diretrizes de Design de infraestrutura de chave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Servidor Web (IIS)  

A função de servidor Web (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para a hospedagem confiável de sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, intranet ou extranet. O IIS é uma plataforma web unificada que integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).  

Para obter mais informações, consulte [visão geral do servidor Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
