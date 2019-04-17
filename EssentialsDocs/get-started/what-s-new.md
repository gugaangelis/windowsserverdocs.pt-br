---
title: "Quais são as novidades no Windows Server 2016 Essentials"
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
ms.openlocfilehash: 7fed7e71f7ac163437fe5d32da7c867f93fbcf00
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
#<a name="whats-new-in-windows-server-2016-essentials"></a>Quais são as novidades no Windows Server 2016 Essentials

> Aplica-se a: Windows Server 2016 Essentials

A seguir é nova e aprimorados recursos no Windows Server 2016 Essentials.

##[<a name="integration-with-azure-site-recovery-services"></a>Integração com serviços de recuperação de Site Azure](azure-site-recovery-services-integration.md)

**O que ele faz** – quando uma máquina virtual que está protegido falhar, ou o servidor host que a máquina virtual protegida é executado em falhar, finalizado com falha com serviços de recuperação de Site do Azure mantém a continuidade de negócios até na máquina virtual local ou servidor host está reparado e disponível. 

**Como ele funciona** – serviços de recuperação de Site do Azure, oferecido no Microsoft Azure, permite a replicação em tempo real de suas máquinas virtuais (VM) para um backup cofre no Azure. Se seu site ou um servidor falhar devido a um hardware ou outra falha, você pode failover com serviços de recuperação de Site do Azure para que a imagem VM armazenada no seu backup cofre será configurada como uma VM em execução no Azure. Combinado com uma rede Virtual do Azure, cliente computadores conectados anteriormente para o servidor local transparente conectará ao servidor que executa no Azure.     
                                                                                                                                                                                                                                                                                                               

## [<a name="integration-with-azure-virtual-network"></a>Integração com a rede Virtual do Azure](azure-virtual-network-integration.md)

**O que ele faz**– conforme as organizações fazem sua maneira da computação em nuvem, eles raramente moverem todos os seus recursos de uma só vez. Em vez disso, eles mover alguns recursos na nuvem e manter alguns no local. Dessa forma, é fácil mover uma organização para a nuvem em estágios ao longo do tempo. A integração de rede virtual Azure fornece a infraestrutura de rede que torna esse processo contínuo e gerenciável.

**Como ele funciona** – redes virtuais do Azure é um serviço oferecido no Microsoft Azure que permite que as organizações criar uma ponto a ponto (P2P) ou -to-site (S2S) rede virtual privada que torna os recursos que estão em execução no Azure (como máquinas virtuais e armazenamento) aparência como se estivessem na rede local para acesso perfeito de recursos e dos aplicativos.



##[<a name="support-for-larger-deployments"></a>Suporte para implantações de maiores](support-for-larger-deployments.md) 

Algumas pequenas empresas maiores necessitam mais funcionalidade e a capacidade para implementar o Windows Server Essentials efetivamente. Windows Server 2016 Essentials oferece maior capacidade de gerenciamento de domínios, os usuários e dispositivos, adicionando suporte para implantações maiores com:                                                                                                                                                                                                 

 - Vários domínios
 - Vários controladores de domínio                                                                                                                                                                                                                                        
 - Capacidade para especificar um controlador de domínio designado                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Consulte também
--------

[Introdução ao Windows Server Essentials](get-started.md)
