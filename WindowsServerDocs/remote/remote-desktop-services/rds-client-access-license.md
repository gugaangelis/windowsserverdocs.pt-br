---
title: Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)
description: Visão geral do cliente de licenciamento nos Serviços de Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 02/12/2020
manager: dongill
ms.openlocfilehash: 295536afc77d0559fd7d2d4a22f555231a1aab75
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858069"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Cada usuário e dispositivo que conecta a um Host da Sessão da Área de Trabalho Remota precisa de uma CAL (Licença de Acesso para Cliente). Use o Licenciamento de Área de Trabalho Remota para instalar, emitir e rastrear as CALs para Serviços de Área de Trabalho Remota.  

Quando um usuário ou um dispositivo se conecta a um servidor Host da Sessão da Área de Trabalho Remota, ele determina se uma CAL para Serviços de Área de Trabalho Remota é necessária. Em seguida, o servidor Host da Sessão da Área de Trabalho Remota solicita uma CAL para Serviços de Área de Trabalho Remota do servidor de licenças de Área de Trabalho Remota. Se uma CAL para Serviços de Área de Trabalho Remota apropriada estiver disponível em um servidor de licença, uma CAL para Serviços de Área de Trabalho Remota será emitida para o cliente e, a partir de lá, para a área de trabalho ou aplicativos que estão tentando usar.

Embora exista um período de cortesia do licenciamento, durante o qual não é necessário nenhum servidor de licença, ao final deste período, os clientes precisarão receber uma CAL para Serviços de Área de Trabalho Remota válida emitida por um servidor de licença para poderem fazer o logon em um servidor Host da Sessão da Área de Trabalho Remota.

Use as informações a seguir para saber mais sobre como funciona o licenciamento de acesso para cliente nos Serviços de Área de Trabalho Remota e como implantar e gerenciar suas licenças:

- [Licenciar a implantação do RDS com CALs (Licenças de Acesso para Cliente)](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Noções básicas sobre o modelo de CAL para Serviços de Área de Trabalho Remota](#understanding-the-rds-cal-model)
  - [Compatibilidade entre versões da CAL para Serviços de Área de Trabalho Remota](#rds-cal-version-compatibility)

## <a name="understanding-the-rds-cal-model"></a>Noções básicas sobre o modelo de CAL para Serviços de Área de Trabalho Remota

Há dois tipos de CALs para Serviços de Área de Trabalho Remota:

- CALs de RDS por dispositivo
- CALs de RDS por usuário

A tabela a seguir descreve as diferenças entre os dois tipos de CALs:

| Por Dispositivo                                                     | Por Usuário                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| As CALs para Serviços de Área de Trabalho Remota são fisicamente atribuídas a cada dispositivo.                   | As CALs para Serviços de Área de Trabalho Remota são atribuídas a um usuário no Active Directory.                                 |
| As CALs para Serviços de Área de Trabalho Remota são rastreadas pelo servidor de licença.                        | As CALs para Serviços de Área de Trabalho Remota são rastreadas pelo servidor de licença.                                          |
| As CALs para Serviços de Área de Trabalho Remota podem ser rastreadas, independentemente da associação no Active Directory. | Não é possível rastrear CALs para Serviços de Área de Trabalho Remota dentro de um grupo de trabalho.                                       |
| É possível revogar até 20% das CALs para Serviços de Área de Trabalho Remota.                              | Não é possível revogar nenhuma CAL para Serviços de Área de Trabalho Remota.                                                      |
| As CALs para Serviços de Área de Trabalho Remota temporárias são válidas durante 52 a 89 dias.                       | As CALs para Serviços de Área de Trabalho Remota temporárias não estão disponíveis.                                                |
| Não é possível superalocar CALs para Serviços de Área de Trabalho Remota.                                  | As CALs para Serviços de Área de Trabalho Remota podem ser superalocadas (violando o contrato de licenciamento de Área de Trabalho Remota). |

Quando você usa o modelo por dispositivo, uma licença temporária é emitida na primeira vez que um dispositivo conecta ao Host da Sessão RD. Na segunda vez que o dispositivo se conecta, contanto que o servidor de licença esteja ativado e existam CALs para Serviços de Área de Trabalho Remota disponíveis, o servidor de licença emite uma CAL por Dispositivo aos Serviços de Área de Trabalho Remota permanente.

Quando você usa o modelo por usuário, licenciamento não é imposto e cada usuário recebe uma licença para conectar a um Host da Sessão RD em qualquer número de dispositivos. O servidor de licença emite licenças do pool da CAL para Serviços de Área de Trabalho Remota disponível ou para o pool da CAL para Serviços de Área de Trabalho Remota superutilizada. É sua responsabilidade verificar se todos os seus usuários têm uma licença válida e nenhuma CAL Superutilizada; caso contrário, você estará violando os termos de licença dos Serviços de Área de Trabalho Remota.

Para verificar se você está em conformidade com os termos de licença dos Serviços de Área de Trabalho Remota, rastreie o número de CALs por Usuário aos Serviços de Área de Trabalho Remota usadas na sua organização e verifique se você tem CALs por Usuário aos Serviços de Área de Trabalho Remota instaladas no servidor de licença de todos os seus usuários.

Você pode usar o Gerenciador de Licenciamento de Área de Trabalho Remota para rastrear e gerar relatórios sobre CALs de RDS por usuário.

## <a name="rds-cal-version-compatibility"></a>Compatibilidade entre versões da CAL para Serviços de Área de Trabalho Remota

A CAL para Serviços de Área de Trabalho Remota dos seus usuários ou dispositivos deve ser compatível com a versão do Windows Server à qual o usuário ou o dispositivo está se conectando. Você não pode usar CALs para Serviços de Área de Trabalho Remota para versões anteriores a fim de acessar versões posteriores do Windows Server, mas pode usar versões posteriores de CALs para Serviços de Área de Trabalho Remota a fim de acessar versões anteriores do Windows Server. Por exemplo, uma CAL para Serviços de Área de Trabalho Remota 2016 ou posterior é necessária para se conectar a um Host da Sessão RD do Windows Server 2016, enquanto uma CAL para Serviços de Área de Trabalho Remota 2012 ou posterior é necessária para se conectar a um Host da Sessão RD do Windows Server 2012 R2.

A tabela a seguir mostra quais versões da CAL para Serviços de Área de Trabalho Remota e do Host da Sessão RD são compatíveis entre si.

|                  | CAL para Serviços de Área Trabalho Remota 2008 R2 e versões anteriores | CAL para Serviços de Área de Trabalho Remota 2012 | CAL para Serviços de Área de Trabalho Remota 2016 | CAL para Serviços de Área de Trabalho Remota 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Host da sessão 2008, 2008 R2** | Sim    | Sim    | Sim    | Sim     |
| **Host da sessão 2012**         | Não     | Sim    | Sim    | Sim    |
| **Host da sessão 2012 R2**      | Não     | Sim    | Sim    | Sim    |
| **Host da sessão 2016**         | Não     | Não     | Sim    | Sim    |
| **Host da sessão 2019**         | Não     | Não     | Não     | Sim    |

Você deve instalar a CAL para Serviços de Área de Trabalho Remota em um servidor de licença de RD compatível. Qualquer servidor de licenças de RDS pode hospedar licenças de todas as versões anteriores dos serviços dos Serviços de Área de Trabalho Remota e a versão atual dos Serviços de Área de Trabalho Remota. Por exemplo, um servidor de licenças de RDS do Windows Server 2016 pode hospedar licenças de todas as versões anteriores do RDS, enquanto um servidor de licenças de RDS do Windows Server 2012 R2 só pode hospedar licenças até o Windows Server 2012 R2.

A tabela a seguir mostra quais versões da CAL para Serviços de Área de Trabalho Remota e do servidor de licença são compatíveis entre si.

|                  | CAL para Serviços de Área Trabalho Remota 2008 R2 e versões anteriores | CAL para Serviços de Área de Trabalho Remota 2012 | CAL para Serviços de Área de Trabalho Remota 2016 | CAL para Serviços de Área de Trabalho Remota 2019 |
|---------------------------------|--------|--------|--------|--------|
| **Servidor de licença 2008, 2008 R2** | Sim    | Não   | Não   | Não    |
| **Servidor de licença 2012**         | Sim     | Sim    | Não   | Não    |
| **Servidor de licença 2012 R2**      | Sim     | Sim    | Não   | Não    |
| **Servidor de licença 2016**         | Sim     | Sim    | Sim   | Não    |
| **Servidor de licença 2019**         | Sim     | Sim    | Sim  | Sim   |
