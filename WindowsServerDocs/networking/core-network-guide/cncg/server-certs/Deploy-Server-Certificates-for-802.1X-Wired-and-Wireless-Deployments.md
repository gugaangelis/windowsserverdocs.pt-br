---
title: Implantar certificados de servidor para implantações com e sem fio do 802.1X
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fd5c9ea9954053fd21f6ab46ff0b6d2f8da5245f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995570"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implantar certificados de servidor para implantações com e sem fio do 802.1X

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este guia para implantar certificados de servidor em seus servidores de infraestrutura de acesso remoto e servidor de políticas de rede (NPS).

Este guia contém as seções a seguir.

-   [Pré-requisitos para usar este guia](#bkmk_pre)

-   [O que este guia não contém](#bkmk_not)

-   [Visões gerais da tecnologia](#bkmk_tech)

-   [Visão geral da implantação de certificado do servidor](Server-Certificate-Deployment-Overview.md)

-   [Planejamento de implantação de certificado do servidor](Server-Certificate-Deployment-Planning.md)

-   [Implantação de certificado do servidor](Server-Certificate-Deployment.md)

### <a name="digital-server-certificates"></a>**Certificados do servidor digital**
Este guia fornece instruções para usar Active Directory serviços de certificados (AD CS) para registrar automaticamente certificados para acesso remoto e servidores de infraestrutura do NPS. O AD CS permite criar uma PKI (infraestrutura de chave pública) e fornecer criptografia de chave pública, certificados digitais e recursos de assinatura digital para sua organização.

Quando você usa certificados de servidor digital para autenticação entre computadores em sua rede, os certificados fornecem:

1. Confidencialidade por meio de criptografia.
2. Integridade por meio de assinaturas digitais.
3. Autenticação associando chaves de certificado a contas de computador, usuário ou dispositivo em uma rede de computadores.

### <a name="server-types"></a>**Tipos de servidores**
Usando este guia, você pode implantar certificados de servidor nos seguintes tipos de servidores.
- Servidores que executam o serviço de acesso remoto, que são servidores de rede virtual privada (VPN) do DirectAccess ou padrão, e que são membros do grupo de **Servidores RAS e ias** .
- Servidores que executam o serviço de servidor de diretivas de rede (NPS) que são membros do grupo de **Servidores RAS e ias** .

### <a name="advantages-of-certificate-autoenrollment"></a>**Vantagens do registro automático de certificado**
O registro automático de certificados de servidor, também chamado de inscrição automática, oferece as seguintes vantagens.

- A AC (autoridade de certificação) do AD CS registra automaticamente um certificado de servidor para todos os seus servidores de acesso remoto e NPS.
- Todos os computadores no domínio recebem automaticamente seu certificado de autoridade de certificação, que é instalado no repositório de autoridades de certificado raiz confiáveis em cada computador membro do domínio. Por isso, todos os computadores no domínio confiam nos certificados emitidos pela sua autoridade de certificação. Essa confiança permite que seus servidores de autenticação comprovem suas identidades entre si e participem de comunicações seguras.
- Além de atualizar Política de Grupo, a reconfiguração manual de todos os servidores não é necessária.
- Cada certificado de servidor inclui a finalidade de autenticação de servidor e a finalidade de autenticação de cliente em extensões EKU (uso avançado de chave).
- Escalabilidade. Depois de implantar sua autoridade de certificação raiz corporativa com este guia, você pode expandir sua PKI (infraestrutura de chave pública) adicionando CAs de empresa subordinada.
- Capacidade de gerenciamento. Você pode gerenciar o AD CS usando o console do AD CS ou usando comandos e scripts do Windows PowerShell.
- Simplicidade. Você especifica os servidores que registram certificados de servidor usando Active Directory contas de grupo e a associação de grupo.
- Quando você implanta certificados de servidor, os certificados são baseados em um modelo que você configura com as instruções neste guia. Isso significa que você pode personalizar modelos de certificado diferentes para tipos de servidor específicos ou pode usar o mesmo modelo para todos os certificados de servidor que você deseja emitir.

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>Pré-requisitos para usar este guia

Este guia fornece instruções sobre como implantar certificados de servidor usando o AD CS e a função de servidor do servidor Web (IIS) no Windows Server 2016. Veja a seguir os pré-requisitos para executar os procedimentos deste guia.

- Você deve implantar uma rede principal usando o guia de rede do Windows Server 2016 core ou já deve ter as tecnologias fornecidas no guia de rede principal instalado e funcionando corretamente na rede. Essas tecnologias incluem TCP/IP V4, DHCP, Active Directory Domain Services (AD DS), DNS e NPS.
  >[!NOTE]
  >O guia de rede do Windows Server 2016 Core está disponível na biblioteca técnica do Windows Server 2016. Para obter mais informações, consulte [Guia de rede de núcleo](../../../core-network-guide/Core-Network-Guide.md).

- Você deve ler a seção de planejamento deste guia para garantir que você está preparado para essa implantação antes de executar a implantação.
- Você deve executar as etapas neste guia na ordem em que elas são apresentadas. Não vá em frente e implante sua autoridade de certificação sem executar as etapas que levam à implantação do servidor ou a sua implantação falhará.
- Você deve estar preparado para implantar dois novos servidores em sua rede-um servidor no qual você instalará o AD CS como uma AC raiz corporativa e um servidor no qual você instalará o servidor Web (IIS) para que sua autoridade de certificação possa publicar a CRL (lista de certificados revogados) no servidor Web.

>[!NOTE]
>Você está preparado para atribuir um endereço IP estático aos servidores Web e do AD CS que você implanta com este guia, bem como para nomear os computadores de acordo com as convenções de nomenclatura da organização. Além disso, você deve unir os computadores ao seu domínio.

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>O que este guia não contém
Este guia não fornece instruções abrangentes para projetar e implantar uma PKI (infraestrutura de chave pública) usando o AD CS. É recomendável que você examine a documentação do AD CS e a documentação de design PKI antes de implantar as tecnologias neste guia.

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>Visões gerais da tecnologia
A seguir estão as visões gerais de tecnologia para o AD CS e o servidor Web (IIS).

### <a name="active-directory-certificate-services"></a>Serviços de Certificados do Active Directory
O AD CS no Windows Server 2016 fornece serviços personalizáveis para criar e gerenciar os certificados X. 509 que são usados em sistemas de segurança de software que empregam tecnologias de chave pública. As organizações podem usar o AD CS para aumentar a segurança ligando a identidade de uma pessoa, dispositivo ou serviço a uma chave pública correspondente. O AD CS também inclui recursos que permitem que você gerencie o registro e a revogação de certificados em uma variedade de ambientes escalonáveis.

Para obter mais informações, consulte [visão geral dos serviços de certificados Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831740(v=ws.11)) e [diretrizes de design de infraestrutura de chave pública](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/designing-and-implementing-a-pki-part-i-design-and-planning/ba-p/396953).

### <a name="web-server-iis"></a>Servidor Web (IIS)

A função do servidor Web (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para hospedagem confiável de sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, em uma intranet ou em uma extranet. O IIS é uma plataforma Web unificada que integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).

Para obter mais informações, consulte [visão geral do servidor Web (IIS)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831725(v=ws.11)).