---
title: O que há de novo no Windows Server 2016 Essentials
description: Informações sobre as novidades do Windows Server 2016 Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: geschuma
ms.openlocfilehash: ecce1760c4768d37936343604d9344a6d489c8cc
ms.sourcegitcommit: efc49ff9062f943abe36c5b6206096315723f56d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90571325"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>O que há de novo no Windows Server 2016 Essentials

> Aplica-se a: Windows Server 2016 Essentials

A seguir estão os recursos novos e aprimorados do Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-services"></a>[Integração com serviços de Azure Site Recovery](azure-site-recovery-services-integration.md)

**O que ele faz**  -- &reg; Quando uma máquina virtual que está protegida falhar, ou o servidor host em que a máquina virtual protegida falhar, o failover com Azure Site Recovery Services manterá a continuidade dos negócios até que a máquina virtual local ou o servidor host seja reparado e disponível.

**Como funciona** – Azure site Recovery serviços, oferecidos na Microsoft Azure, permite a replicação em tempo real de suas máquinas virtuais (VM) para um cofre de backup no Azure. Caso o servidor ou o site fique inativo devido a um hardware ou outra falha, você pode fazer failover com os serviços de Azure Site Recovery para que a imagem de VM armazenada em seu cofre de backup seja provisionada como uma VM em execução no Azure. Combinado com uma rede virtual do Azure, os PCs cliente que anteriormente conectados ao servidor local se conectarão de forma transparente ao servidor em execução no Azure.

## <a name="integration-with-azure-virtual-network"></a>[Integração com a rede virtual do Azure](azure-virtual-network-integration.md)

**O que ele faz**, à medida que as organizações passam para a computação em nuvem, eles raramente movem todos os seus recursos de uma só vez. Em vez disso, eles movem alguns recursos para a nuvem e mantêm algum local. Dessa forma, é fácil mover uma organização para a nuvem em estágios ao longo do tempo. A integração de rede virtual do Azure fornece a infraestrutura de rede que torna esse processo simples e gerenciável.

**Como funciona** – a rede virtual do Azure é um serviço oferecido no Microsoft Azure que permite que as organizações criem uma rede virtual privada de ponto a ponto (P2P) ou site a site (S2S) que faz com que os recursos que estão em execução no Azure (como máquinas virtuais e armazenamento) pareçam estar na rede local para acesso contínuo ao aplicativo e aos recursos.

## <a name="support-for-larger-deployments"></a>[Suporte para implantações maiores](support-for-larger-deployments.md)

Algumas pequenas empresas maiores precisam de mais funcionalidade e capacidade para implementar o Windows Server Essentials com eficiência. O Windows Server 2016 Essentials fornece maior capacidade de gerenciamento de domínios, usuários e dispositivos adicionando suporte para implantações maiores com:

- vários domínios
- vários controladores de domínio
- capacidade de especificar um controlador de domínio designado

Confira também

- [Introdução ao Windows Server Essentials](get-started.md)
