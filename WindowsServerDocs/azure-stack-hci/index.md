---
title: Visão geral de pilha HCI Azure
description: 'Azure HCI pilha é um cluster do Windows Server 2019 hiperconvergente usa validados hardware para executar cargas de trabalho virtualizadas locais, opcionalmente, conectando-se aos serviços do Azure para backup baseado em nuvem, site recovery e muito mais. Soluções de pilha HCI Azure usam hardware Microsoft validada para garantir a confiabilidade e o desempenho ideal e incluem suporte para tecnologias como unidades NVMe, memória persistente e a rede de acesso (RDMA) de memória remoto direto.'
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Visão geral de pilha HCI Azure

>Aplica-se a: Windows Server 2019

Azure HCI pilha é um cluster do Windows Server 2019 hiperconvergente usa validados hardware para executar cargas de trabalho virtualizadas locais, opcionalmente, conectando-se aos serviços do Azure para backup baseado em nuvem, site recovery e muito mais. Soluções de pilha HCI Azure usam hardware Microsoft validada para garantir a confiabilidade e o desempenho ideal e incluem suporte para tecnologias como unidades NVMe, memória persistente e a rede de acesso (RDMA) de memória remoto direto.

## A família de pilha do Azure

Azure Stack HCI faz parte do Azure e família de pilha do Azure, usando o mesmo definido pelo software de computação, armazenamento e rede software como Azure Stack. Aqui está um resumo rápido das soluções diferentes:

- [Azure](https://azure.microsoft.com) - serviços de nuvem pública de uso
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - operam no local de serviços de nuvem
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - execução virtualizado aplicativos locais, com conexões opcionais para o Azure

![Azure e o Azure Stack executar serviços em nuvem, enquanto executa o Azure Stack HCI virtualizados aplicativos locais](media/azure-and-azure-stack-family.png)

Para saber mais:

- Registre-se para nosso [evento Virtual de nuvem híbrida](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html) no 28 de março de 2019.
- Saiba mais em nosso site de soluções [HCI de pilha do Azure](https://azure.microsoft.com/overview/azure-stack/hci) .
- Assista Jeff Woolsey especialistas da Microsoft e Vijay Tewari [falar sobre as novas soluções HCI de pilha do Azure](https://aka.ms/AzureStackOverviewVideo).

## Parceiros de hardware

Você pode comprar soluções Azure Stack HCI validadas que executam o Windows Server 2019 de 15 de parceiros. Seu parceiro Microsoft preferencial entrar em execução sem design demorada e tempo de compilação e oferece um único ponto de contato para serviços de implementação e suporte.

Visite o [site do Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) para exibir nossas 70 + Azure Stack HCI soluções disponíveis atualmente esses de parceiros da Microsoft: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, soluções primeLine, QCT, SecureGUARD e Supermicro.

## Perguntas frequentes

### O que as soluções de pilha do Azure e o Azure Stack HCI têm em comum? 
Soluções de pilha HCI Azure apresentam a mesma Hyper-V com base definido pelo software computação, armazenamento e tecnologias de rede como Azure Stack. Ambas as ofertas atendem aos critérios rigorosos de teste e validação para garantir a confiabilidade e compatibilidade com a plataforma de hardware subjacente.

### Como eles são diferentes?
Com a pilha do Azure, você executar nuvem serviços locais. Você pode executar o IaaS do Azure e PaaS serviços locais para compilar e executar aplicativos de nuvem em qualquer lugar, gerenciados com o Portal do Azure no local de forma consistente.

Com o Azure Stack HCI, executar cargas de trabalho virtualizadas locais, gerenciado com o Windows Admin Center e familiar do Windows Server ferramentas. Opcionalmente, você pode conectar-se ao Azure para cenários de híbrido como recuperação de site baseado em nuvem, monitoramento e outros.

### Por que a Microsoft é trazer seu HCI oferecendo para a família de pilha do Azure? 
A tecnologia da Microsoft hiperconvergido; já é a base da pilha do Azure. 

Muitos clientes da Microsoft tem ambientes de TI complexos e nosso objetivo é fornecer soluções que cumpri-los onde eles estão com a tecnologia adequada para a direita empresas precisa. Azure HCI pilha é uma evolução das soluções de construção baseada no Windows Server 2016 (WSSD) anteriormente disponíveis de nossos parceiros de hardware. Vamos colocá-lo para a família do Azure Stack porque começamos a oferecer novas opções para se conectar com o Azure perfeitamente para serviços de gerenciamento da infraestrutura. 

### Será capaz de fazer upgrade do Azure Stack HCI para Azure Stack? 
Não, mas os clientes podem migrar suas cargas de trabalho do Azure Stack HCI Azure Stack ou do Azure.

### Quais serviços do Azure pode conectar ao Azure Stack HCI?

Para obter uma lista atualizada de serviços do Azure que você pode conectar HCI de pilha do Azure para, consulte [Conectando-se o Windows Server para os serviços do Azure híbrido](../azure-hybrid-services/index.md).

### Como posso comprar soluções HCI de pilha do Azure?
Siga estas etapas: 

1. Compre um sistema de hardware Microsoft validou com seu parceiro de hardware preferencial.
1. Instalar o Windows Server 2019 Datacenter edition e o Windows Admin Center para o gerenciamento e a capacidade de se conectar ao Azure para serviços de nuvem
1. Opcionalmente, use sua conta do Azure para anexar serviços de gerenciamento e segurança baseada em nuvem para suas cargas de trabalho.