---
title: Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)
description: Visão geral do cliente de licenciamento nos Serviços de Área de Trabalho Remota.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 252ee776946ba0c387d7a6cdf3dc97ffdc55a591
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387581"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Cada usuário e dispositivo que conecta a um Host da Sessão da Área de Trabalho Remota precisa de uma CAL (Licença de Acesso para Cliente). Use o Licenciamento de Área de Trabalho Remota para instalar, emitir e rastrear as CALs para Serviços de Área de Trabalho Remota.  

Quando um usuário ou um dispositivo se conecta a um servidor Host da Sessão da Área de Trabalho Remota, ele determina se uma CAL para Serviços de Área de Trabalho Remota é necessária. Em seguida, o servidor Host da Sessão da Área de Trabalho Remota solicita uma CAL para Serviços de Área de Trabalho Remota do servidor de licenças de Área de Trabalho Remota. Se uma CAL para Serviços de Área de Trabalho Remota apropriada estiver disponível em um servidor de licença, uma CAL para Serviços de Área de Trabalho Remota será emitida para o cliente e, a partir de lá, para a área de trabalho ou aplicativos que estão tentando usar.

Embora exista um período de cortesia do licenciamento, durante o qual não é necessário nenhum servidor de licença, ao final deste período, os clientes precisarão receber uma CAL para Serviços de Área de Trabalho Remota válida emitida por um servidor de licença para poderem fazer o logon em um servidor Host da Sessão da Área de Trabalho Remota.

Use as informações a seguir para saber mais sobre como funciona o licenciamento de acesso para cliente nos Serviços de Área de Trabalho Remota e como implantar e gerenciar suas licenças:

- [Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Noções básicas sobre o modelo de CALs](#understanding-the-cals-model)
  - [Nota sobre as versões de CAL](#note-about-cal-versions)

## <a name="understanding-the-cals-model"></a>Noções básicas sobre o modelo de CALs

Existem dois tipos de CALs:

- CALs de RDS por dispositivo
- CALs de RDS por usuário

A tabela a seguir descreve as diferenças entre os dois tipos de CALs:

| Por Dispositivo                                                     | Por Usuário                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| CALs são fisicamente atribuídas a cada dispositivo.                   | CALs são atribuídas a um usuário no Active Directory.                                 |
| CALs são rastreadas pelo servidor de licença.                        | CALs são rastreadas pelo servidor de licença.                                          |
| CALs podem ser rastreadas, independentemente da associação no Active Directory. | Não é possível rastrear CALs dentro de um grupo de trabalho.                                       |
| É possível revogar até 20% das CALs.                              | Não é possível revogar CALs.                                                      |
| CALs temporárias são válidas por 52 a 89 dias.                       | CALs temporárias não estão disponíveis.                                                |
| CALs não podem ser superalocadas.                                  | CALs podem ser superalocadas (violando o contrato de licenciamento da Área de Trabalho Remota). |

Quando você usa o modelo por dispositivo, uma licença temporária é emitida na primeira vez que um dispositivo conecta ao Host da Sessão RD. Na segunda vez que o dispositivo se conecta, contanto que o servidor de licença esteja ativado e existam CALs disponíveis, o servidor de licença emite uma CAL de RDS por dispositivo permanente.

Quando você usa o modelo por usuário, licenciamento não é imposto e cada usuário recebe uma licença para conectar a um Host da Sessão RD em qualquer número de dispositivos. O servidor de licença emite licenças de pool de CAL disponível ou do pool de CAL Superutilizada. É sua responsabilidade verificar se todos os seus usuários têm uma licença válida e nenhuma CAL Superutilizada; caso contrário, você estará violando os termos de licença dos Serviços de Área de Trabalho Remota.

Para garantir a conformidade com os termos de licença dos Serviços de Área de Trabalho Remota, rastreie o número de CALs de RDS por usuário usadas na sua organização e certifique-se de ter CALs por usuário suficientes instaladas no servidor de licença para todos os usuários.

Você pode usar o Gerenciador de Licenciamento de Área de Trabalho Remota para rastrear e gerar relatórios sobre CALs de RDS por usuário.

## <a name="note-about-cal-versions"></a>Nota sobre as versões de CAL

A CAL usada por usuários ou dispositivos deve corresponder à versão do Windows Server ao qual o usuário ou dispositivo está conectando. Não é possível usar CALs mais antigas para acessar as versões mais recentes do Windows Server, mas é possível usar CALs mais recentes para acessar versões anteriores do Windows Server.

A tabela a seguir mostra as CALs que são compatíveis em Hosts da Sessão RD e Hosts de Virtualização de Área de Trabalho Remota.

|                  |CAL 2008 R2 e anterior|CAL 2012|CAL 2016|CAL 2019|
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licença 2008, 2008 R2**| Sim    | Não     | Não     | Não     |
| **Servidor de licença 2012**         | Sim    | Sim    | Não     | Não     |
| **Servidor de licença 2012 R2**      | Sim    | Sim    | Não     | Não     |
| **Servidor de licença 2016**         | Sim    | Sim    | Sim    | Não     |
| **Servidor de licença 2019**         | Sim    | Sim    | Sim    | Sim    |

Qualquer servidor de licenças de RDS pode hospedar licenças de todas as versões anteriores dos serviços dos Serviços de Área de Trabalho Remota e a versão atual dos Serviços de Área de Trabalho Remota. Por exemplo, um servidor de licenças de RDS do Windows Server 2016 pode hospedar licenças de todas as versões anteriores do RDS, enquanto um servidor de licenças de RDS do Windows Server 2012 R2 só pode hospedar licenças até o Windows Server 2012 R2.
