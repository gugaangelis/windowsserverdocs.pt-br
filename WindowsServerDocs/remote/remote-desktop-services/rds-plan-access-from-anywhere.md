---
title: Serviços de área de trabalho remota - acesso em qualquer lugar
description: Informações de planejamento para um Gateway de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845827"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Serviços de área de trabalho remota - acesso em qualquer lugar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Os usuários finais podem se conectar aos recursos de rede interna segura de fora do firewall corporativo por meio do Gateway de área de trabalho remota.

Independentemente de como você configura as áreas de trabalho para os usuários finais, você pode conectar facilmente o Gateway de área de trabalho remota no fluxo de conexão para uma conexão rápida e segura. Para usuários finais se conectam por meio de feeds de publicado, você pode configurar a propriedade de Gateway de área de trabalho remota ao configurar as propriedades de implantação geral. Para usuários finais se conectam por meio de suas áreas de trabalho sem um feed, eles podem adicionar facilmente o nome do Gateway de área de trabalho remota da organização como uma propriedade de conexão, independentemente de qual aplicativo de cliente de área de trabalho remota, eles usam.

As três principais finalidades do Gateway de área de trabalho remota, na ordem de sequência de conexão, são:
1. **Estabelecer um túnel SSL criptografado entre o dispositivo do usuário final e o servidor de Gateway de área de trabalho remota**: Para se conectar por meio de qualquer servidor de Gateway de área de trabalho remota, o servidor de Gateway de área de trabalho remota deve ter um certificado instalado que reconhece de dispositivo do usuário final. Em testes e provas de conceitos, os certificados autoassinados podem ser usados, mas somente certificados publicamente confiáveis de uma autoridade de certificação devem ser usados em qualquer ambiente de produção.
2. **Autenticar o usuário para o ambiente**: O Gateway de área de trabalho remota usa a caixa de entrada de serviço para realizar a autenticação IIS e pode até mesmo utilizar o protocolo RADIUS para aproveitar [a autenticação multifator](rds-plan-mfa.md) soluções como o Azure MFA. Além das diretivas padrão criadas, você pode criar políticas de autorização de recurso de área de trabalho remota (RD RAPs) e diretivas de autorização de Conexão de área de trabalho remota (RD CAPs) adicionais para definir, mais especificamente, quais usuários devem ter acesso a quais recursos dentro de seguro ambiente.
3. **Passagem de tráfego entre o dispositivo do usuário final e o recurso especificado**: O Gateway de área de trabalho remota continua a executar essa tarefa para desde que a conexão é estabelecida. Você pode especificar propriedades de tempo limite diferente nos servidores de Gateway de área de trabalho remota para manter a segurança do ambiente, caso os usuários se deslocam para longe do dispositivo.

Você pode encontrar detalhes adicionais sobre a arquitetura geral de uma implantação de serviços de área de trabalho remota [na área de trabalho de arquitetura de referência de hospedagem](desktop-hosting-reference-architecture.md).