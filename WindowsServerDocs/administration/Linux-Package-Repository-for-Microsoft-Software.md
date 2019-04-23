---
title: Repositório de Software do Linux para produtos da Microsoft
description: Este documento descreve como usar e instalar pacotes de software do Linux para produtos da Microsoft.
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: dbdbd0f436645f7e19c07e4f3278c5073636a547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831857"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repositório de Software do Linux para produtos da Microsoft

## <a name="overview"></a>Visão geral
Microsoft compila e dá suporte a uma variedade de produtos de software para os sistemas Linux e disponibiliza-os por meio de repositórios de pacote padrão APT e YUM. Este documento descreve como configurar o repositório em seu sistema Linux, para que você pode, em seguida, instalar/atualizar usando ferramentas de gerenciamento de pacote padrão da distribuição de software do Linux da Microsoft.

Repositório de Software do Linux da Microsoft é composto de vários repositórios de subpropriedades:

 - Prod – produção o repositório sub é designado para os pacotes destinados a uso em produção. Esses pacotes comercialmente são suportados pela Microsoft sob os termos do contrato de suporte aplicável ou programa que você tem com a Microsoft.

 - MSSQL-server - esses repositórios contêm pacotes para o Microsoft SQL Server no Linux – consulte também: [SQL Server no Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

>[!Note]
Os pacotes nos repositórios de software Linux são sujeito aos termos de licença localizados nos pacotes. Leia os termos de licença antes de usar o pacote. Sua instalação e uso do pacote constitui a sua aceitação destes termos. Se você não concordar com os termos de licença, não use o pacote.


## <a name="configuring-the-repositories"></a>Configurando os repositórios
Repositórios podem ser configurados automaticamente ao instalar o pacote do Linux que se aplica à sua distribuição do Linux e versão. O pacote instalará a configuração do repositório, juntamente com a chave pública do GPG usada por ferramentas como o apt/yum/zypper para validar os pacotes assinados e/ou metadados do repositório.

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux (RHEL e variações)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 (confiável)

        wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update

 - Ubuntu 16.10 (Yakkety)

        wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb
        sudo dpkg -i packages-microsoft-prod.deb
        sudo apt-get update


### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configuração manual
Os arquivos de configuração de repositório estão disponíveis no [packages.microsoft.com/config](https://packages.microsoft.com/config/). O nome e o local desses arquivos podem ser localizados usando a seguinte convenção de nomenclatura da URI:

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Pacote e assinatura do repositório de chave**

 - Chave pública da Microsoft GPG pode ser baixado aqui: [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID da chave pública: Microsoft (assinatura de versão) <gpgsecurity@microsoft.com>
 - Impressão digital da chave pública: `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>Exemplos:

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



