---
title: O que há de novo no SDN no Windows Server
description: Este tópico fornece informações sobre novos recursos de rede definida pelo Software para Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: aef2bc32f249550d4e8d33d4b871ca98010e2f49
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446293"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novidades na SDN para o Windows Server 2019

>Aplica-se a: Windows Server (canal semestral)


|                         **Recurso**                          |                                                                                                                                                                                         **Descrição**                                                                                                                                                                                         | **Novo/atualizado** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Redes criptografadas](vnet-encryption/sdn-vnet-encryption.md) | Criptografia de rede virtual permite a criptografia de tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como 'A criptografia habilitada.' Ele também utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar pacotes. O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física. |       Novo       |
|    [Auditoria de firewall](security/sdn-firewall-auditing.md)    |                                                                                            Auditoria de firewall é um novo recurso para o firewall do SDN no Windows Server 2019. Quando você habilita o firewall do SDN, qualquer fluxo processado pelas regras de firewall SDN (ACLs) que têm o registro em log habilitado obtém gravado.                                                                                            |       Novo       |
| [Emparelhamento de rede virtual](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      Emparelhamento de rede virtual permite que você conecte duas redes virtuais perfeitamente. Uma vez emparelhadas, para fins de conectividade, as redes virtuais aparecerão como um.                                                                                                                      |       Novo       |
|           [Saída de medição](manage/sdn-egress.md)            |                  Esse novo recurso no Windows Server 2019 permite SDN oferecer os medidores de uso para transferências de dados. Com esse recurso foi adicionado, mantém o controlador de rede, uma lista de permissões por rede Virtual de todos os intervalos IP usado em SDN e considere qualquer pacote associado para um destino que não está incluído em um desses intervalos de ser cobrado transferências de dados.                   |       Novo       |

---



