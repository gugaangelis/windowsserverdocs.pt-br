---
title: Repositório de software do Linux para produtos da Microsoft
description: Este documento descreve como usar e instalar pacotes de software do Linux para produtos da Microsoft.
ms.custom: na
ms.prod: windows-server
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: 0627c38f15966948dd4bea91b66a96ee59ec89e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370446"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Repositório de software do Linux para produtos da Microsoft

## <a name="overview"></a>Visão geral
A Microsoft cria e dá suporte a uma variedade de produtos de software para sistemas Linux e os torna disponíveis por meio dos repositórios de pacote APT e YUM padrão. Este documento descreve como configurar o repositório em seu sistema Linux, para que você possa instalar/atualizar o software Linux da Microsoft usando as ferramentas de gerenciamento de pacotes padrão de sua distribuição.

O repositório de software do Linux da Microsoft é composto por vários repositórios:

 - prod – o subrepositório de produção é designado para pacotes destinados para uso na produção. Esses pacotes têm suporte comercial da Microsoft sob os termos do contrato de suporte ou do programa aplicável que você tem com a Microsoft.

 - MSSQL-Server-esses repositórios contêm pacotes para Microsoft SQL Server em Linux-consulte também: [SQL Server em Linux](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux).

> [!Note]
> Os pacotes nos repositórios de software do Linux estão sujeitos aos termos de licença localizados nos pacotes. Leia os termos de licença antes de usar o pacote. A instalação e o uso do pacote constituem sua aceitação desses termos. Se você não concordar com os termos de licença, não use o pacote.


## <a name="configuring-the-repositories"></a>Configurando os repositórios
Os repositórios podem ser configurados automaticamente instalando o pacote do Linux que se aplica à sua distribuição e versão do Linux. O pacote instalará a configuração do repositório junto com a chave pública GPG usada por ferramentas como apt/yum/zypper para validar os pacotes assinados e/ou os metadados do repositório.

### <a name="enterprise-linux-rhel-and-variants"></a>Linux corporativo (RHEL e variantes)

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14, 4 (confiança)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16, 4 (Xenial)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18, 4 (Bionic)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18,10 (raios cósmicos)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19, 4 (disco)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>Configuração manual
Os arquivos de configuração do repositório estão disponíveis em [Packages.Microsoft.com/config](https://packages.microsoft.com/config/). O nome e o local desses arquivos podem ser localizados usando a seguinte convenção de nomenclatura de URI:

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**Chave de assinatura de pacote e repositório**

 - A chave pública do GPG da Microsoft pode ser baixada aqui:[https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - ID da chave pública: Microsoft (assinatura de versão)<gpgsecurity@microsoft.com>
 - Impressão digital da chave pública:`BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

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



