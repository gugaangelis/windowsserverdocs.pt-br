---
title: O que há de novo no Windows Server 2016 Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bc83686f76c49773203d63a88894841f65ffd1d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433765"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>O que há de novo no Windows Server 2016 Essentials

> Aplica-se a: Windows Server 2016 Essentials

A seguir é nova e aprimorados recursos no Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-servicesazure-site-recovery-services-integrationmd"></a>[Integração com serviços de recuperação de Site do Azure](azure-site-recovery-services-integration.md)

**O que ele faz** – quando uma máquina virtual que é protegido falhar ou o servidor de host que a máquina virtual protegida é executado falhar, o failover com serviços do Azure Site Recovery mantém a continuidade dos negócios até que a máquina de virtual no local ou servidor de host é reparado e está disponível. 

**Como ele funciona** – serviços do Azure Site Recovery, oferecido no Microsoft Azure, habilita a replicação em tempo real de suas máquinas virtuais (VM) em um cofre de backup no Azure. No caso em que seu servidor ou site fica inativo devido a uma falha de hardware ou outra, você pode fazer o failover com serviços do Azure Site Recovery para que a imagem VM armazenada no seu Cofre de backup será provisionada como uma VM em execução no Azure. Combinado com uma rede Virtual do Azure, computadores conectados anteriormente ao servidor local de cliente irá conectar de forma transparente para o servidor em execução no Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-networkazure-virtual-network-integrationmd"></a>[Integração com a rede Virtual do Azure](azure-virtual-network-integration.md)

**O que ele faz**- - como as organizações cheguem à computação em nuvem, eles raramente mover todos os seus recursos ao mesmo tempo. Em vez disso, eles se movam alguns recursos para a nuvem e manter alguns no local. Dessa forma, é fácil mover de uma organização para a nuvem em fases ao longo do tempo. Integração da rede virtual do Azure fornece a infraestrutura de rede que faz com que o processo contínuo e gerenciável.

**Como ele funciona** – rede Virtual do Azure é um serviço oferecido no Microsoft Azure que permite às organizações criar uma ponto a ponto (P2P) ou site a site (S2S) rede virtual privada que faz com que os recursos que estão em execução no Azure (como máquinas virtuais e armazenamento) parecem como se eles estão na rede local para o aplicativo de conexão remota e acesso a recursos.



## <a name="support-for-larger-deploymentssupport-for-larger-deploymentsmd"></a>[Suporte para implantações maiores](support-for-larger-deployments.md) 

Algumas pequenas empresas maiores precisam de mais funcionalidades e capacidades para implementar o Windows Server Essentials com eficiência. Windows Server 2016 Essentials oferece maior capacidade de gerenciamento de domínios, usuários e dispositivos, adicionando suporte para implantações maiores com:                                                                                                                                                                                                 

 - vários domínios
 - vários controladores de domínio                                                                                                                                                                                                                                        
 - capacidade de especificar um controlador de domínio designado                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Consulte também
--------

[Introdução ao Windows Server Essentials](get-started.md)
