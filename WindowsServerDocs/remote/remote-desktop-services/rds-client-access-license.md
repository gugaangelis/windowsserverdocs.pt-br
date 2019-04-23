---
title: Licença de sua implantação do RDS com licenças de acesso para cliente (CALs)
description: Visão geral do cliente de licenciamento nos serviços de área de trabalho remota.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853437"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licença de sua implantação do RDS com licenças de acesso para cliente (CALs)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Cada usuário e dispositivo se conecta a um host de sessão de área de trabalho remota precisam um licenças de acesso para cliente (CAL). Usar o licenciamento de área de trabalho remota para instalar, emitir e acompanhar as RDS CALs.  

Quando um usuário ou um dispositivo se conecta a um servidor de Host de sessão de área de trabalho remota, o servidor de Host de sessão de área de trabalho remota determina se uma RDS CAL é necessária. O servidor de Host de sessão de área de trabalho remota, em seguida, solicita uma RDS CAL do servidor de licenças de área de trabalho remota. Se uma RDS CAL apropriada está disponível de um servidor de licença, o CAL RDS é emitido para o cliente e o cliente é capaz de se conectar ao servidor de Host de sessão de área de trabalho remota e daí para a área de trabalho ou aplicativos que eles estão tentando usar.

Embora não haja um período de cortesia de licenciamento durante o qual nenhuma licença de servidor é necessário, após o período de cortesia termina, os clientes deve ter uma RDS CAL válida emitida por um servidor de licença antes de eles podem fazer logon em um servidor de Host de sessão de área de trabalho remota.

Use as informações a seguir para saber mais sobre como funciona o licenciamento de acesso de cliente nos serviços de área de trabalho remota e para implantar e gerenciar suas licenças:

- [Entender o modelo de CALs](#understanding-the-cals-model)
- [Ativar o servidor de licença](rds-activate-license-server.md)
- [Instalar RDS CALs no servidor de licença](rds-install-cals.md)
- [Acompanhar as CALs usadas em sua implantação](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>Noções básicas sobre o modelo de CALs

Há dois tipos de CALs:

- RDS por CALs de dispositivo
- RDS por CALs de usuário

A tabela a seguir descreve as diferenças entre os dois tipos de CALs:

| Por Dispositivo                                                     | Por Usuário                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| CALs fisicamente são atribuídas a cada dispositivo.                   | CALs são atribuídas a um usuário no Active Directory.                                 |
| CALs são controladas pelo servidor de licenças.                        | CALs são controladas pelo servidor de licenças.                                          |
| CALs podem ser controladas, independentemente da associação do Active Directory. | Não não possível controlar CALs dentro de um grupo de trabalho.                                       |
| Você pode revogar até 20% de CALs.                              | Não é possível revogar CALs.                                                      |
| CALs temporárias são válidas por dias de 52 – 89.                       | CALs temporárias não estão disponíveis.                                                |
| CALs não podem ser superalocadas.                                  | Podem ser superalocadas CALs (em violação da área de trabalho remota contrato de licenciamento). |

Quando você usa o modelo por dispositivo, uma licença temporária é emitida na primeira vez que um dispositivo se conecta ao Host da sessão de área de trabalho remota. Na segunda vez que o dispositivo se conecta, desde que o servidor de licença está ativado e há CALs disponíveis, os problemas de servidor de licença uma CAL por dispositivo permanente.

Quando você usa o modelo por usuário, licenciamento não é imposto e cada usuário recebe uma licença para se conectar a um Host de sessão de área de trabalho remota de qualquer número de dispositivos. O servidor de licença emite licenças de pool de CALs disponível ou o pool Over-Used CAL. Ele é sua responsabilidade garantir que todos os seus usuários tenham uma licença válida e zero Over-Used CALs — caso contrário, você está em violação dos termos de licença dos serviços de área de trabalho remota.

Para garantir a conformidade com os termos de licença dos serviços da área de trabalho remota, acompanhar o número de RDS por CALs de usuário usada na sua organização e certifique-se de ter um suficiente por CALs de usuário instalado no servidor de licenças para todos os seus usuários.

Você pode usar o Gerenciador de licenciamento de área de trabalho remota para rastrear e gerar relatórios sobre RDS por CALs de usuário.

## <a name="note-about-cal-versions"></a>Observação sobre as versões de CAL

O CAL usado por usuários ou dispositivos deve corresponder à versão do Windows Server que o usuário ou dispositivo está se conectando ao. É possível usar CALs mais antigas para acessar as versões mais recentes do Windows Server, mas você pode usar CALs mais recentes para acessar versões anteriores do Windows Server.

A tabela a seguir mostra as CALs que são compatíveis em Hosts de sessão de área de trabalho remota e Hosts de virtualização de área de trabalho remota.

|                  |2008 R2 e anterior CAL|2012 CAL|2016 CAL|2019 CAL|
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licença do 2008, 2008 R2**| Sim    | Não     | Não     | Não     |
| **2012 license server**         | Sim    | Sim    | Não     | Não     |
| **2012 R2 license server**      | Sim    | Sim    | Não     | Não     |
| **servidor de licença de 2016**         | Sim    | Sim    | Sim    | Não     |
| **servidor de licença de 2019**         | Sim    | Sim    | Sim    | Sim    |

Qualquer servidor de licença RDS pode hospedar licenças de todas as versões anteriores dos serviços de área de trabalho remota e a versão atual dos serviços de área de trabalho remota. Por exemplo, um servidor de licença de RDS do Windows Server 2016 pode hospedar licenças de todas as versões anteriores do RDS, enquanto um servidor de licença de RDS do Windows Server 2012 R2 só pode hospedar licenças até o Windows Server 2012 R2.
