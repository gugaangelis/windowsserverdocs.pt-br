---
title: Malha protegida e VM Blindada, guia de planejamento para Hosters
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f280fbe682ebf706ce6ea5b53ea8af5e6f39d75d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857807"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Malha protegida e VM Blindada, guia de planejamento para Hosters

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico aborda as decisões de planejamento que precisam ser feitas habilite blindado máquinas virtuais para executar em sua malha. Se você atualizar uma malha de Hyper-V existente ou cria uma malha de novo, executando blindado VMs consiste em dois componentes principais:

- O serviço de guardião de Host (HGS) fornece proteção de Atestado e chave para que você possa fazer-se de que as VMs blindadas serão executados somente em hosts do Hyper-V íntegros e aprovados. 
- Hosts do Hyper-V íntegros e aprovados na qual as VMs blindadas (e VMs comuns) podem ser executados, eles são conhecidos como hosts protegidos.

![HGS e um host protegido](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>Decisão #1: Nível na malha de confiança

Como você implementa o serviço guardião de Host e hosts protegidos do Hyper-V depende principalmente da segurança de confiança que você está procurando para atingir em sua malha. A intensidade da relação de confiança é regida pelo modo de Atestado. Há duas opções mutuamente exclusivas:

1. Atestado de TPM confiável

    Se seu objetivo é ajudar a proteger as máquinas virtuais contra administradores mal-intencionados ou uma malha comprometida, você usará o atestado de TPM confiável. Essa opção funciona bem para vários locatários cenários de hospedagem, bem como para ativos de alto valor em ambientes corporativos, como controladores de domínio ou servidores de conteúdo, como SQL ou SharePoint.
    Políticas de integridade (HVAC) de código protegido por hipervisor são medidas e sua validade imposta pelo HGS antes que o host tenha permissão de execução de VMs blindadas. 

2. Atestado de chaves do host

    Se seus requisitos são orientados basicamente pela conformidade que requer virtuais máquinas criptografados tanto em repouso, bem como em andamento, em seguida, você usará o atestado de chaves do host. Essa opção funciona bem para data centers de finalidade geral em que estiver familiarizado com os administradores de malha e host do Hyper-V tendo acesso a sistemas operacionais convidados de máquinas virtuais para operações e manutenção diária. 

    Nesse modo, o administrador da malha é exclusivamente responsável por garantir a integridade dos hosts do Hyper-V. 
    Uma vez que o HGS não desempenha nenhuma parte decidir o que é ou não tem permissão para executar, malware e depuradores funcionará como projetado. 
    
    No entanto, os depuradores que tentam conectar-se diretamente a um processo (como WinDbg.exe) são bloqueadas para VMs blindadas porque o processo de trabalho da VM (VMWP.exe) é uma luz de processo protegido (PPL). 
    Técnicas de depuração alternativas, como os usados pelo LiveKd.exe, não são bloqueadas. 
    Ao contrário de VMs blindadas, o processo de trabalho para VMs com suporte de criptografia não é executado como um PPL depuradores tradicionais como WinDbg.exe continuarão a funcionar normalmente.

    Um modo de Atestado semelhante chamada atestado de Admin confiável (baseado no Active Directory) é substituído, começando com o Windows Server 2019. 

O nível de confiança que você escolher determinarão os requisitos de hardware para seus hosts Hyper-V, bem como as políticas aplicadas na malha. Se necessário, você pode implantar sua malha protegida usando o hardware existente e o atestado de admin confiável e, em seguida, convertê-lo para atestado de TPM confiável quando o hardware foi atualizado e você precisa reforçar a segurança de malha.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>Decisão #2: Malha de Hyper-V existente em comparação com uma nova malha separada do Hyper-V

Se você tiver uma malha existente (Hyper-V ou não), é muito provável que você pode usá-lo a executar VMs blindadas, juntamente com VMs comuns. Alguns clientes optam por integrar VMs blindadas em suas ferramentas existentes e malhas, enquanto outros separam da malha por motivos comerciais.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Administrador do HGS planejamento para o serviço guardião de Host

Implantar o Host HGS (serviço guardião) em um ambiente altamente seguro, seja em um servidor físico dedicado, uma VM blindada, uma VM em um host Hyper-V isolado (separados da malha do qual que ele está protegendo) ou um logicamente separados, usando um Azure diferente assinatura.   

| Área | Detalhes |
|------|---------|
| Requisitos de instalação | <ul><li>Um servidor (cluster de três nós recomendado para alta disponibilidade)</li><li>Para fallback, são necessários pelo menos dois servidores HGS</li><li>Servidores podem ser físico ou virtual (servidor físico com o TPM 2.0 recomendada; TPM 1.2 também têm suportada)</li><li>Instalação do Server Core do Windows Server 2016 ou posterior</li><li>Rede linha de visão para a malha, permitindo que o HTTP ou [configuração de fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificado HTTPS recomendado para validação de acesso</li></ul> |
| Dimensionamento | Cada nó de servidor HGS médio porte (8 núcleos e 4, GB) pode manipular 1.000 hosts do Hyper-V. |
| Management | Designe pessoas específicas que irão gerenciar HGS. Eles devem ser separados dos administradores da malha. Para comparação, clusters HGS podem ser considerados da mesma maneira como uma autoridade de certificação (CA) em termos de isolamento administrativo, implantação física e nível geral de diferenciação de segurança. |
| Guardião de host serviço do Active Directory | Por padrão, o HGS instala seu próprio interno do Active Directory para gerenciamento. Essa é uma floresta independente, autogerenciada e é a configuração recomendada para ajudar a isolar o HGS da sua malha.<br><br>Se você já tiver uma floresta do Active Directory altamente privilegiada que você usa para isolamento, você pode usar essa floresta, em vez da floresta de padrão HGS. É importante que HGS não ingressou em um domínio na mesma floresta que os hosts Hyper-V ou as ferramentas de gerenciamento de malha. Isso pode permitir que um administrador de malha obter controle sobre o HGS. |
| Recuperação de desastres | Há três opções:<br><ol><li>Instalar um cluster HGS separado em cada datacenter e autorizar VMs blindadas para ser executado no primário e os datacenters de backup. Isso evita a necessidade do cluster estendido em uma WAN e lhe permite isolar as máquinas virtuais, de modo que eles são executados apenas em seu site designado.</li><li>Instale o HGS em um cluster estendido entre data centers de dois (ou mais). Isso fornece resiliência se WAN fica inativo, mas supera os limites de cluster de failover. Não é possível isolar as cargas de trabalho a um site; uma VM autorizada a executar em um site pode executar em qualquer outro.</li><li>Registre seu host do Hyper-V com o HGS outro como failover.</li></ol>Você também deve fazer backup todos HGS Exportando sua configuração para que você sempre pode recuperar localmente. Para obter mais informações, consulte [exportação HgsServerState](https://technet.microsoft.com/library/mt652164.aspx) e [HgsServerState importação](https://technet.microsoft.com/library/mt652168.aspx). |
| Chaves de serviço guardião de host | Um serviço guardião de Host usa dois pares de chaves assimétricas — uma chave de criptografia e uma chave de assinatura — cada um representado por um certificado SSL. Há duas opções para gerar essas chaves:<br><ol><li>Autoridade de certificação interna – você pode gerar essas chaves usando sua infraestrutura PKI interna. Isso é adequado para um ambiente de datacenter.</li><li>Autoridades de certificação confiável publicamente – use um conjunto de chaves obtidos de uma autoridade de certificação confiável publicamente. Essa é a opção que hosters devem usar.</li></ol>Observe que embora seja possível usar certificados autoassinados, não é recomendável para cenários de implantação que não seja de laboratórios de prova de conceito.<br><br>Além de ter as chaves HGS, um hoster pode usar "Traga sua própria chave" onde locatários podem fornecer suas próprias chaves para que os locatários algumas (ou todos) podem ter sua própria chave HGS específico. Esta opção é adequada para hosters que podem fornecer um processo de out-of-band para locatários carregar suas chaves. |
| Armazenamento de chaves de serviço guardião de host | Para a maior segurança possível, recomendamos que as chaves HGS são criadas e armazenadas exclusivamente em um módulo HSM (Hardware Security). Se você não estiver usando HSMs, aplicar o BitLocker em servidores do HGS é recomendável. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planejamento de administrador de malha para hosts protegidos

| Área | Detalhes |
|------|---------|
| Hardware | <ul><li>Atestado de chaves do host: Você pode usar qualquer hardware existente como seu host protegido. Há algumas exceções (para certificar-se de que o host pode usar os novos mecanismos de segurança, começando com o Windows Server 2016, consulte [hardware compatível com a proteção baseada em virtualização do Windows Server 2016 de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Atestado de TPM confiável: Você pode usar qualquer hardware que tem o [qualificação adicional de garantia de Hardware](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) desde que ele está configurado corretamente (consulte [configurações de servidores que estão em conformidade com VMs Blindadas e baseada em virtualização proteção de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) para a configuração específica). Isso inclui o TPM 2.0 e a versão UEFI 2.3.1c e acima.</li></ul> |
| SO | É recomendável usar a opção Server Core para o sistema operacional do host do Hyper-V. |
| Implicações de desempenho | Com base nos testes de desempenho, prevemos uma aproximadamente 5% densidade-diferença entre execução blindado VMs e VMs não blindadas. Isso significa que, se um determinado host do Hyper-V pode executar 20 VMs não blindadas, devemos esperar que ele pode executar VMs blindadas 19.<br><br>Certifique-se de verificar se o dimensionamento com suas cargas de trabalho típicos. Por exemplo, pode haver algumas exceções com e orientada a gravação e/s cargas de trabalho intensas que mais afetará a diferença de densidade. |
| Considerações das filiais | Começando com o Windows Server versão 1709, você pode especificar uma URL de fallback para um servidor HGS virtualizado em execução localmente como uma VM blindada na filial. A URL de fallback pode ser usada quando o escritório da filial perde conectividade com servidores HGS no data center. Em versões anteriores do Windows Server, um host Hyper-V em execução em uma conectividade de necessidades do branch office para o serviço guardião de Host para ligar ou migrar ao vivo VMs blindadas. Para obter mais informações, consulte [ramificar considerações office](guarded-fabric-manage-branch-office.md). |
