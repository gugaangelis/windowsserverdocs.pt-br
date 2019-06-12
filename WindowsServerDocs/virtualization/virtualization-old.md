---
title: Virtualização
description: Fornece uma visão geral das tecnologias de virtualização, como Contêineres, o Hyper-V e o Comutador Virtual do Hyper-V, além de links para conteúdo adicional para o Windows Server 2016 e versões posteriores do sistema operacional.
ms.prod: windows-server-threshold
manager: dougkim
ms.technology: compute
ms.topic: article
author: shortpatti
ms.author: pashort
ms.localizationpriority: medium
ms.date: 03/16/2018
ms.openlocfilehash: e6dbb5be6d836462c9a24078dbec3700b09b08fc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446054"
---
# <a name="virtualization"></a>Virtualização

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016 

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

<img src="../media/landing-icons/virtualization.png" style='float:left; padding:.5em;' alt="Icon showing a box with spokes"> Virtualização no Windows Server 2016 é uma das tecnologias fundamentais necessárias para criar sua infraestrutura definida por software. Juntamente com a rede e o armazenamento, os recursos de virtualização oferecem a flexibilidade de que você precisa para acionar cargas de trabalho para seus clientes.

Tecnologias de virtualização do Windows Server incluem atualizações do Hyper-V, o comutador Virtual do Hyper-V e malha protegida e VMs Blindadas \(VMs\), que melhoram a segurança, escalabilidade e confiabilidade. As atualizações de cluster de failover, rede e armazenamento facilitam ainda a implantação e o gerenciamento dessas tecnologias quando usadas com o Hyper-V. 


<ul class="cardsI panelContent">
<li>
        <a href="../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Malha protegida e VMs blindadas</h3>
                        <p>Como um provedor de serviços em nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um Host de HGS (serviço guardião) - normalmente, um cluster de três nós - mais de um ou mais protegidos hosts e um conjunto de VMs blindadas.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
    </li>
<li>
        <a href="/hyper-v/Hyper-V-on-Windows-Server.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Windows 10 para a empresa: Maneiras de usar dispositivos para trabalho</h3>
                        <p>A tecnologia do Hyper-V fornece recursos de computação por meio da virtualização de hardware. Hyper-V cria uma versão de software de um computador, chamada de máquina virtual, que você usa para executar um sistema operacional e aplicativos. Você pode executar várias máquinas virtuais ao mesmo tempo e pode criá-las e excluí-las conforme necessário. </p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>

<li>
        <a href="https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-server-2016">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Microsoft Hyper-V Server</h3>
                        <p>A tecnologia do Hyper-V fornece recursos de computação por meio da virtualização de hardware. Hyper-V cria uma versão de software de um computador, chamada de máquina virtual, que você usa para executar um sistema operacional e aplicativos. Você pode executar várias máquinas virtuais ao mesmo tempo e pode criá-las e excluí-las conforme necessário. </p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>


<li>
        <a href="hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Comutador Virtual Hyper-V</h3>
                        <p>O comutador Virtual do Hyper-V é um baseada em software camada 2 comutador de rede Ethernet está incluído em todas as versões do Hyper-V.</p>

                        <p>Comutador Virtual Hyper-V está disponível no Gerenciador do Hyper-V depois de instalar a função de servidor do Hyper-V.</p>

                        <p>Funcionalidades programaticamente gerenciadas e extensíveis que permitem que você conectar máquinas virtuais à rede física e redes virtuais estão incluídos no comutador Virtual do Hyper-V.</p> 

                        <p>Além disso, o Comutador Virtual Hyper-V fornece imposição de política para níveis de segurança, de isolamento e de serviço.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>


<li>
       <a href="https://docs.microsoft.com/virtualization/windowscontainers">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Contêineres do Windows</h3>
                        <p>Contêineres do Windows fornecem uma virtualização no nível do sistema operacional que permite que vários aplicativos isolados sejam executados em um único sistema. Dois tipos diferentes de tempos de execução do contêiner são incluídos no recurso, cada um com um grau de isolamento do aplicativo diferente.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>




## <a name="related"></a>Relacionados

O Hyper-V exige hardware específico para criar o ambiente de virtualização. Para obter mais detalhes, consulte [Requisitos do sistema para o Hyper-V no Windows Server 2016](./hyper-v/system-requirements-for-hyper-v-on-windows.md). 

Para obter mais informações, consulte [Hyper-V no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows).

