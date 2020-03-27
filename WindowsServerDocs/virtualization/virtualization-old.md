---
title: Virtualização
description: Fornece uma visão geral das tecnologias de virtualização, como Contêineres, o Hyper-V e o Comutador Virtual do Hyper-V, além de links para conteúdo adicional para o Windows Server 2016 e versões posteriores do sistema operacional.
ms.prod: windows-server
manager: dougkim
ms.technology: compute
ms.topic: article
author: eross-msft
ms.author: lizross
ms.localizationpriority: medium
ms.date: 03/16/2018
ms.openlocfilehash: d5a3390f1a072e5d155f19a97fe90ef481436f33
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307913"
---
# <a name="virtualization"></a>Virtualização

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016 

>[!TIP]
> Está procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar informações específicas neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions).

<img src="../media/landing-icons/virtualization.png" style='float:left; padding:.5em;' alt="Icon showing a box with spokes"> Virtualização no Windows Server 2016 é uma das tecnologias fundamentais necessárias para criar sua infraestrutura definida por software. Juntamente com as funcionalidades de rede e armazenamento, os recursos de virtualização oferecem a flexibilidade de que você precisa para potencializar cargas de trabalho para seus clientes.

As tecnologias de virtualização do Windows Server incluem atualizações para Hyper-V, comutador virtual Hyper-V e malha protegida e máquinas virtuais blindadas \(VMs\), que melhoram a segurança, a escalabilidade e a confiabilidade. As atualizações de cluster de failover, rede e armazenamento facilitam ainda mais a implantação e o gerenciamento dessas tecnologias quando usadas com o Hyper-V. 


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
                        <p>Como um provedor de serviços de nuvem ou um administrador de nuvem privada corporativa, você pode usar uma malha protegida para fornecer um ambiente mais seguro para as VMs. Uma malha protegida consiste em um HGS (Serviço Guardião de Host) – em geral, um cluster de três nós – além de um ou mais hosts protegidos e um conjunto de máquinas virtuais protegidas.</p>
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
                        <h3>Windows 10 para a empresa: maneiras de usar dispositivos para trabalhar</h3>
                        <p>A tecnologia do Hyper-V fornece recursos de computação por meio da virtualização de hardware. O Hyper-V cria uma versão de software de um computador, chamada de máquina virtual, usada para executar um sistema operacional e aplicativos. Você pode executar várias máquinas virtuais ao mesmo tempo e pode criá-las e excluí-las conforme necessário. </p>
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
                        <p>A tecnologia do Hyper-V fornece recursos de computação por meio da virtualização de hardware. O Hyper-V cria uma versão de software de um computador, chamada de máquina virtual, usada para executar um sistema operacional e aplicativos. Você pode executar várias máquinas virtuais ao mesmo tempo e pode criá-las e excluí-las conforme necessário. </p>
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
                        <p>O Comutador Virtual do Hyper-V é um comutador de rede Ethernet baseado em software de camada 2 que está incluído em todas as versões do Hyper-V.</p>

                        <p>O Comutador Virtual Hyper-V está disponível no Gerenciador do Hyper-V, após instalar a função de servidor do Hyper-V.</p>

                        <p>Funcionalidades programaticamente gerenciadas e extensíveis que permitem conectar máquinas virtuais tanto a redes físicas quanto virtuais estão incluídas no Comutador Virtual do Hyper-V.</p> 

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
                        <p>Os Contêineres do Windows fornecem uma virtualização no nível do sistema operacional que permite que vários aplicativos isolados sejam executados em um único sistema. Dois tipos diferentes de runtimes do contêiner são incluídos com o recurso, cada um com um grau diferente de isolamento do aplicativo.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>




## <a name="related"></a>Relacionado

O Hyper-V requer hardware específico para criar o ambiente de virtualização. Para obter mais detalhes, consulte [Requisitos do sistema para o Hyper-V no Windows Server 2016](./hyper-v/system-requirements-for-hyper-v-on-windows.md). 

Para obter mais informações, consulte [Hyper-V no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows).

