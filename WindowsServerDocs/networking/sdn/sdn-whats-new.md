---
title: O que há de novo no SDN para Windows Server
description: Este tópico fornece informações sobre novos recursos de rede definidos pelo software para o Windows Server 1709
manager: grcusanz
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: bcf4923ec25aba66484ec384c8895d877aba0069
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962140"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novidades na SDN para o Windows Server 2019

>Aplica-se a: Windows Server (Canal semestral)


|                         **Recurso**                          |                                                                                                                                                                                         **Descrição**                                                                                                                                                                                         | **Novo/atualizado** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Redes criptografadas](vnet-encryption/sdn-vnet-encryption.md) | A criptografia de rede virtual permite a criptografia do tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como ' criptografia habilitada '. Ela também utiliza o DTLS (Datagrama do protocolo TLS) na sub-rede virtual para criptografar os pacotes. O DTLS protege contra interceptações, falsificação e adulteração por qualquer pessoa com acesso à rede física. |       Novo       |
|    [Auditoria de firewall](security/sdn-firewall-auditing.md)    |                                                                                            A auditoria de firewall é uma nova funcionalidade para o firewall de SDN no Windows Server 2019. Quando você habilita o Firewall do SDN, qualquer fluxo processado por ACLs (regras de firewall do SDN) que tenham o log habilitado é registrado.                                                                                            |       Novo       |
| [Emparelhamento de rede virtual](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      O emparelhamento de rede virtual permite que você conecte duas redes virtuais diretamente. Uma vez emparelhadas, para fins de conectividade, as redes virtuais aparecem como uma.                                                                                                                      |       Novo       |
|           [Medição de egresso](manage/sdn-egress.md)            |                  Esse novo recurso do Windows Server 2019 permite que o SDN ofereça medidores de uso para transferências de dados de saída. Com esse recurso adicionado, o controlador de rede mantém uma lista de permissões por rede virtual de todos os intervalos de IP usados dentro do SDN e considera qualquer pacote associado a um destino que não está incluído em um desses intervalos para serem cobradas transferências de dados de saída.                   |       Novo       |

---



