---
title: Benefício Híbrido do Azure para Windows Server
description: Use suas licenças do Windows Server local para salvar em VMs do Azure
ms.prod: windows-server
ms.date: 11/10/2017
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.openlocfilehash: 187abe06b469abe511d4bbbfb0aac9237d3c650a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868515"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Benefício Híbrido do Azure para Windows Server

>Aplica-se a: Windows Server

## <a name="benefit-description-rules-and-use-cases"></a>Descrição, regras e casos de uso do benefício

O Benefício Híbrido do Azure para Windows Server permite que você economize até 40% em VMs do Windows Server no Azure utilizando suas licenças do Windows Server local com o Software Assurance.  Com esse benefício, os clientes precisam apenas pagar os custos de infraestrutura da máquina virtual porque o licenciamento para o Windows Server está coberto pelos benefícios do Software Assurance.  O benefício é aplicável às edições Standard e Datacenter do Windows Server para as versões 2008R2, 2012, 2012R2 e 2016.  Esse benefício está disponível em todas as regiões e nuvens soberanas.


![imagem 1](media/ahb01.png)

Tudo o que você precisa para se qualificar ao benefício é de um Software Assurance ou de uma Licença de Assinatura ativa, como a assinatura EAS, SCE ou a Open Value Subscription nas licenças do Windows Server.  

Cada licença de 2 processadores do Windows Server com um SA/Assinatura ativa e cada conjunto de 16 licenças de núcleo do Windows Server com SA/Assinatura, qualifica o cliente a usar o Windows Server no Microsoft Azure em até 16 núcleos virtuais alocados em duas ou menos Instâncias Base do Azure (máquinas virtuais). Cada conjunto adicional de oito licenças de núcleo com SA/Assinatura qualifica para o uso em até oito núcleos virtuais e uma Instância Base (VM).

| Licença com SA/Assinatura            | VMs e núcleos concedidos            | Como eles podem ser usados                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| WS Datacenter (16 núcleos ou um L de 2 processadores)  | Até duas VMs e até 16 núcleos | Executar máquinas virtuais localmente e no Azure  |
| WS Standard (16 núcleos ou um L de 2 processadores)    | Até duas VMs e até 16 núcleos | Executar máquinas virtuais localmente ou no Azure |

As VMs que utilizam o Benefício Híbrido do Azure só podem ser executadas durante o termo do SA/Assinatura. Quando a hora de expiração do SA/Assinatura estiver se aproximando, o cliente terá a opção de renovar o SA/Assinatura, desativar a funcionalidade de benefício híbrido dessa VM ou desprovisionar a VM usando o benefício híbrido. 

### <a name="savings-examples"></a>Exemplos de economia 

![imagem 2](media/ahb02.png)
 
Abaixo, você encontra uma tabela de referência para ajudá-lo a compreender as regras do benefício com maior granularidade. A coluna verde mostra a quantidade de VMs do mesmo tipo e a linha azul mostra a densidade de núcleo de cada VM. As células amarelas mostram o número de licenças de dois processadores (ou conjuntos de 16 núcleos) que você deve ter para implantar um determinado número de VMs de uma determinada densidade de núcleo. 

Windows Server com tabela de referência de requisitos de SA:

![imagem 3](media/ahb03.png)
 
O Benefício Híbrido do Azure para Windows Server também permite a flexibilidade necessária para executar configurações de acordo com suas necessidades, bem como combinar VMs de tipos diferentes.

Configurações de exemplo para várias posições de licenciamento:

![imagem 4](media/ahb04.png)
![imagem 5](media/ahb05.png)

 
Se você quiser saber mais sobre o Benefício Híbrido do Azure para o Windows Server, vá para o site do Benefício Híbrido do Azure.

## <a name="how-to-maintain-compliance"></a>Como manter a conformidade

Os clientes que procuram aplicar o Benefício Híbrido do Azure às suas VMs do Windows Server precisam verificar o número de licenças qualificadas e o respectivo período de cobertura de seu SA/Assinatura antes de qualquer ativação desse benefício e aplicar as diretrizes acima para implantar o número correto de VMs com o benefício. Se já tiver VMs com o Benefício Híbrido do Azure, você precisará executar um inventário de quantas unidades estão em execução e verificar em relação às licenças do SA ativas que você tem.  Entre em contato com a especialista em licenciamento do Contrato Enterprise da Microsoft para validar sua posição de licenciamento SA.
Para ver e contar todas as máquinas virtuais implantadas com o Benefício Híbrido do Azure para Windows Server em uma assinatura, você poderá executar um destes procedimentos:

1. Configure o Portal do Microsoft Azure para mostrar o Benefício Híbrido do Azure para utilização do Windows Server. Adicione a coluna "Benefício Híbrido do Azure" ao modo de exibição de lista da seção de máquinas virtuais no Portal do Microsoft Azure. 

    ![imagem 6](media/ahb06.png)

2.  Usar o PowerShell para listar o Benefício Híbrido do Azure para utilização do Windows Server

    ```
    $vms = Get-AzureRMVM 
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  Examine sua conta do Microsoft Azure para determinar quantas máquinas virtuais com o Benefício Híbrido do Azure para Windows Server em execução. As informações sobre o número de instâncias com o benefício são mostradas em "Informações adicionais":

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}" 
    ```

Observe que a cobrança não se aplica em tempo real, ou seja, haverá um atraso de algumas horas desde a hora em que você ativou a VM com o benefício híbrido antes que ele fosse mostrado na conta.
Em seguida, você pode preencher os resultados na **Ferramenta do SA de Contagem do Benefício Híbrido do Azure para Windows Server** abaixo para obter o número de licenças WS cobertas pelo SA ou pelas Assinaturas necessários.

Execute um inventário em cada assinatura que você tem para gerar uma visão abrangente da sua posição de licenciamento.

[Ferramenta do SA de Contagem de Benefício Híbrido do Azure para o WS](http://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

Se tiver realizado o que foi realizado acima e confirmado que você está totalmente licenciado para o número de instâncias do Benefício Híbrido do Azure que você está executando, não haverá necessidade de outras ações. Se você tiver descoberto que pode cobrir VMs incrementais com o benefício, convém otimizar seus custos ainda mais alternando para instâncias em execução com o benefício versus custo total.

Se você não tiver licenças do Windows Server suficientes qualificadas para o número de VMs já implantadas, será necessário comprar licenças locais do Windows Server adicionais cobertas com o Software Assurance por meio de um dos canais listados abaixo, comprar VMs do Windows Server a taxas por hora regulares ou desativar a funcionalidade Benefício Híbrido para algumas VMs. Observe que você pode comprar licenças de núcleo no incremento de oito núcleos para se qualificar para cada VM do Benefício Híbrido do Azure adicional. 

O Software Assurance e/ou as Assinaturas do Windows Server estão disponíveis para compra por meio de uma das combinações dos seguintes canais de licenciamento da Microsoft:

| Canal                      | Abrir     | OVS      | Select/ Select Plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| Tamanho típico (nº de dispositivos)  | 5-250    | 5-250    | >250                  | >250      | >500     |
| SA/Assinatura            | Opcional | Incluído | Opcional              | Opcional  | Incluído |

A Microsoft se reserva o direito de auditar o cliente final a qualquer momento para verificar a qualificação para a utilização do Benefício Híbrido do Azure. 

## <a name="deployment-guidance"></a>Diretrizes de implantação 

Habilitamos a disponibilidade de imagens da galeria pré-fabricadas para todos os nossos clientes com licenças qualificadas, independentemente de onde eles as compraram, bem como a parceiros habilitados, para que sejam capazes de executar as implantações em nome dos clientes. 

Veja as instruções para todas as opções de implantação disponíveis [aqui](https://azure.microsoft.com/pricing/hybrid-use-benefit/), incluindo: 
-   Vídeo detalhado realçando a nova experiência de implantação utilizando imagens pré-fabricadas da galeria
-   Instruções detalhadas sobre como carregar uma VM personalizada 
-   Instruções detalhadas sobre como migrar máquinas virtuais existentes usando o Azure Site Recovery com o PowerShell. 
