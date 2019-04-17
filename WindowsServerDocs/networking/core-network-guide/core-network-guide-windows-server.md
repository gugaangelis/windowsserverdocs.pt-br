---
title: Guia da rede principal para o Windows Server
description: Este tópico fornece uma visão geral do guia de rede principal, que permite que você planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo domínio do Active Directory em uma nova floresta com o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Guia da rede principal para o Windows Server

>Aplica-se a: Windows Server, Windows Server 2016

Este tópico fornece uma visão geral do guia de rede principal para Windows Server&reg; de 2016 e contém as seguintes seções.  
  
-   [Introdução à rede do Windows Server Core](#bkmk_intro)  
  
-   [Guia da rede principal para o Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introdução à rede do Windows Server Core

Uma rede principal é uma coleção de hardware de rede, dispositivos, e precisa de um software que fornece os principais serviços de tecnologia da informação (TI) da sua organização.

Uma rede do Windows Server core fornece muitos benefícios, incluindo o seguinte.

- Protocolos de Core para conectividade de rede entre computadores e outros dispositivos compatíveis com o protocolo/Internet (TCP). TCP/IP é um conjunto de protocolos padrão para conectar computadores e criação de redes. TCP/IP é um software de protocolo de rede fornecida com a Microsoft&reg; Windows&reg; conjunto de protocolos de sistemas operacionais que implementa e dá suporte a TCP/IP.

- Dynamic Host Configuration Protocol (DHCP) servidor endereçamento IP automático. Configuração manual de endereços IP em todos os computadores em sua rede é demorado e menos flexível que fornecendo dinamicamente outros dispositivos e computadores com concessões de endereço IP de um servidor DHCP.

- Serviço de resolução de nomes do sistema de nome de domínio (DNS). DNS permite que os usuários, computadores, aplicativos e serviços encontrar os endereços IP dos computadores e dispositivos na rede usando o nome totalmente qualificado domínio do computador ou dispositivo.

- Uma floresta, que é um ou mais domínios do Active Directory que compartilham o mesmo definições de classe e atributo (esquema), informações de site e replicação (configuração) e recursos de pesquisa de toda a floresta (catálogo global).

- Um domínio raiz da floresta, que é o primeiro domínio criado em uma nova floresta. Os grupos de administradores de empresa e administradores de esquema, que são grupos administrativos toda a floresta, estão localizados no domínio raiz da floresta. Além disso, um domínio raiz da floresta, assim como acontece com outros domínios, é uma coleção de computador, usuários e objetos de grupo que são definidos pelo administrador nos serviços de domínio do Active Directory (AD DS). Esses objetos compartilham um políticas comuns de banco de dados e segurança do diretório. Eles também podem compartilhar relações de segurança com outros domínios se você adicionar domínios que sua organização cresce. O serviço de diretório também armazena dados de diretório e permite que computadores autorizados, aplicativos e os usuários acessem os dados.

- Um usuário e computador conta banco de dados. O serviço de diretório fornece um banco de dados de contas de usuário de maneira centralizada que permite que você crie contas de usuário e computador para pessoas e computadores que estão autorizados a se conectar à sua rede e acesso a rede, recursos, como aplicativos, bancos de dados, arquivos compartilhados e pastas e impressoras.

Uma rede principal também permite que você dimensione sua rede que sua organização cresce e mudam de requisitos de TI. Por exemplo, com uma rede principal, você pode adicionar domínios, sub-redes IP, serviços de acesso remoto, serviços sem fio e outros recursos e funções de servidor fornecidas pelo Windows Server 2016.

## <a name="bkmk_core"></a>Guia da rede principal para o Windows Server

Guia de rede do Windows Server 2016 Core fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo Active Directory&reg; domínio em uma nova floresta. Usando este guia, você pode implantar computadores configurados com os seguintes componentes de servidor do Windows:

- A função de servidor de serviços de domínio do Active Directory (AD DS)

- A função de servidor de sistema de nome de domínio (DNS)

- A função de servidor Dynamic Host Configuration Protocol (DHCP)

- O serviço de função de servidor de política de rede (NPS) da função de servidor Serviços de acesso e política de rede

- A função de servidor de Web Server (IIS)

- Transmissão controle protocolo/protocolo IP versão 4 (TCP/IP) conexões em servidores individuais

Este guia está disponível no seguinte local.

- O [Core guia rede](../core-network-guide/Core-Network-Guide.md) na biblioteca técnica do Windows Server 2016.
  


