---
title: Malha protegida e guia de planejamento de VM blindada para hosters
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.author: nirb
ms.date: 08/29/2018
ms.openlocfilehash: c603f946132e3c8811279f94c585de4ec9eb0509
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989142"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>Malha protegida e guia de planejamento de VM blindada para hosters

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico aborda as decisões de planejamento que deverão ser feitas para permitir que máquinas virtuais blindadas sejam executadas em sua malha. Se você atualizar uma malha do Hyper-V existente ou criar uma nova malha, a execução de VMs blindadas consiste em dois componentes principais:

- O serviço guardião de host (HGS) fornece atestado e proteção de chave para que você possa garantir que VMs blindadas sejam executadas somente em hosts Hyper-V aprovados e íntegros. 
- Os hosts Hyper-V aprovados e íntegros nos quais VMs blindadas (e VMs regulares) podem ser executados — eles são conhecidos como hosts protegidos.

![HGS e um host protegido](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>#1 de decisão: nível de confiança na malha

A forma como você implementa o serviço guardião de host e hosts Hyper-V protegidos dependerá principalmente da força de confiança que você pretende atingir em sua malha. A força da confiança é regida pelo modo de atestado. Há duas opções mutuamente exclusivas:

1. TPM-atestado confiável

    Se seu objetivo for ajudar a proteger as máquinas virtuais contra administradores mal-intencionados ou uma malha comprometida, você usará o atestado confiável de TPM. Essa opção funciona bem para cenários de hospedagem multilocatário, bem como para ativos de alto valor em ambientes corporativos, como controladores de domínio ou servidores de conteúdo como SQL ou SharePoint.
    As políticas de política HVAC (integridade de código protegida por hipervisor) são medidas e sua validade imposta pelo HGS antes de o host ter permissão para executar VMs blindadas.

2. Atestado de chave de host

    Se seus requisitos forem controlados principalmente pela conformidade que exige que as máquinas virtuais sejam criptografadas em repouso, bem como em trânsito, você usará o atestado de chave de host. Essa opção funciona bem para data centers de uso geral, em que você está familiarizado com os administradores do Hyper-V e de malhas que têm acesso aos sistemas operacionais convidados de máquinas virtuais para manutenção e operações do dia a dia.

    Nesse modo, o administrador da malha é exclusivamente responsável por garantir a integridade dos hosts Hyper-V.
    Como o HGS não exerce nenhuma parte na decisão de o que é ou não tem permissão para ser executado, malware e depuradores funcionarão conforme projetado.

    No entanto, os depuradores que tentam se conectar diretamente a um processo (como WinDbg.exe) são bloqueados para VMs blindadas porque o processo de trabalho da VM (VMWP.exe) é uma PPL (sinal de processo protegido).
    Técnicas de depuração alternativas, como as usadas pelo LiveKd.exe, não são bloqueadas.
    Ao contrário das VMs blindadas, o processo de trabalho para as VMs com suporte para criptografia não é executado como uma PPL, de forma que os depuradores tradicionais, como WinDbg.exe, continuarão a funcionar normalmente.

    Um modo de atestado semelhante chamado admin – atestado confiável (baseado em Active Directory) é preterido a partir do Windows Server 2019.

O nível de confiança escolhido determinará os requisitos de hardware para seus hosts Hyper-V, bem como as políticas que você aplicará na malha. Se necessário, você pode implantar sua malha protegida usando o hardware existente e o atestado confiável de administrador e, em seguida, convertê-lo no atestado confiável do TPM quando o hardware foi atualizado e você precisa reforçar a segurança da malha.

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>#2 de decisão: malha Hyper-V existente versus uma nova malha Hyper-V separada

Se você tiver uma malha existente (Hyper-V ou não), é muito provável que você possa usá-la para executar VMs blindadas junto com VMs regulares. Alguns clientes optam por integrar VMs blindadas em suas ferramentas e malhas existentes, enquanto outras separam a malha por motivos comerciais.

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>Planejamento de administrador HGS para o serviço guardião de host

Implante o serviço guardião de host (HGS) em um ambiente altamente seguro, esteja ele em um servidor físico dedicado, uma VM blindada, uma VM em um host Hyper-V isolado (separado da malha que está protegendo) ou um logicamente separado usando uma assinatura diferente do Azure.

| Área | Detalhes |
|------|---------|
| Requisitos de instalação | <ul><li>Um servidor (cluster de três nós recomendado para alta disponibilidade)</li><li>Para fallback, são necessários pelo menos dois servidores HGS</li><li>Os servidores podem ser virtuais ou físicos (o servidor físico com TPM 2,0 recomendado; Também há suporte para o TPM 1,2)</li><li>Instalação Server Core do Windows Server 2016 ou posterior</li><li>Linha de visão de rede para a malha que permite a configuração de HTTP ou [fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>Certificado HTTPS recomendado para validação de acesso</li></ul> |
| Dimensionamento | Cada nó de servidor HGS de tamanho médio (8 núcleos/4 GB) pode lidar com hosts Hyper-V 1.000. |
| Gerenciamento | Designe pessoas específicas que irão gerenciar o HGS. Eles devem ser separados dos administradores de malha. Para comparação, os clusters HGS podem ser considerados da mesma maneira que uma autoridade de certificação (CA) em termos de isolamento administrativo, implantação física e nível geral de sensibilidade de segurança. |
| Active Directory do serviço guardião de host | Por padrão, o HGS instala seu próprio Active Directory interno para gerenciamento. Esta é uma floresta autocontida e autogerenciada e é a configuração recomendada para ajudar a isolar o HGS da sua malha.<br><br>Se você já tiver uma floresta Active Directory altamente privilegiada que usa para isolamento, poderá usar essa floresta em vez da floresta padrão do HGS. É importante que o HGS não seja associado a um domínio na mesma floresta que os hosts Hyper-V ou suas ferramentas de gerenciamento de malha. Isso pode permitir que um administrador de malha assuma o controle do HGS. |
| Recuperação de desastre | Há três opções:<br><ol><li>Instale um cluster HGS separado em cada datacenter e autorize as VMs blindadas a serem executadas nos datacenters primário e de backup. Isso evita a necessidade de ampliar o cluster em uma WAN e permite que você Isole as máquinas virtuais, de modo que elas sejam executadas somente em seu site designado.</li><li>Instale o HGS em um cluster de ampliação entre dois (ou mais) datacenters. Isso fornece resiliência se a WAN ficar inativa, mas envia os limites de clustering de failover. Você não pode isolar cargas de trabalho para um site; uma VM autorizada a ser executada em um site pode ser executada em qualquer outra.</li><li>Registre seu host Hyper-V com outro HGS como failover.</li></ol>Você também deve fazer backup de todos os HGS exportando sua configuração para que sempre possa recuperar localmente. Para obter mais informações, consulte [Export-HgsServerState](/powershell/module/hgsserver/export-hgsserverstate) e [Import-HgsServerState](/powershell/module/hgsserver/import-hgsserverstate). |
| Chaves de serviço guardião de host | Um serviço guardião de host usa dois pares de chaves assimétricas — uma chave de criptografia e uma chave de assinatura, cada uma representada por um certificado SSL. Há duas opções para gerar essas chaves:<br><ol><li>Autoridade de certificação interna – você pode gerar essas chaves usando sua infraestrutura de PKI interna. Isso é adequado para um ambiente de datacenter.</li><li>Autoridades de certificação publicamente confiáveis – use um conjunto de chaves obtidas de uma autoridade de certificação publicamente confiável. Essa é a opção que os hosters devem usar.</li></ol>Observe que embora seja possível usar certificados autoassinados, isso não é recomendado para cenários de implantação diferentes de laboratórios de prova de conceito.<br><br>Além de ter chaves HGS, um hoster pode usar "Traga sua própria chave", onde os locatários podem fornecer suas próprias chaves para que alguns (ou todos) locatários possam ter sua própria chave HGS específica. Essa opção é adequada para os hosters que podem fornecer um processo fora de banda para que os locatários Carreguem suas chaves. |
| Armazenamento de chave do serviço guardião de host | Para obter a segurança mais forte possível, recomendamos que as chaves HGS sejam criadas e armazenadas exclusivamente em um HSM (módulo de segurança de hardware). Se você não estiver usando HSMs, é altamente recomendável aplicar o BitLocker nos servidores HGS. |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>Planejamento de administração de malha para hosts protegidos

| Área | Detalhes |
|------|---------|
| Hardware | <ul><li>Atestado de chave de host: você pode usar qualquer hardware existente como seu host protegido. Há algumas exceções (para certificar-se de que seu host pode usar novos mecanismos de segurança a partir do Windows Server 2016, consulte [hardware compatível com a proteção baseada em virtualização do Windows server 2016 de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).</li><li>Atestado confiável de TPM: você pode usar qualquer hardware que tenha a [qualificação adicional de hardware Assurance](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) , desde que esteja configurado adequadamente (consulte Configurações de [servidor que são compatíveis com VMs blindadas e proteção baseada em virtualização de integridade de código](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) para a configuração específica). Isso inclui TPM 2,0 e a versão de UEFI 2.3.1 c e posterior.</li></ul> |
| Sistema operacional | É recomendável usar a opção Server Core para o sistema operacional do host Hyper-V. |
| Implicações de desempenho | Com base no teste de desempenho, antecipamos uma diferença de densidade de aproximadamente 5% entre a execução de VMs blindadas e VMs não blindadas. Isso significa que, se um determinado host Hyper-V puder executar 20 VMs não blindadas, esperamos que ele possa executar 19 VMs blindadas.<br><br>Certifique-se de verificar o dimensionamento com suas cargas de trabalho típicas. Por exemplo, pode haver algumas exceções com cargas de trabalho de e/s com orientação intensa e que afetarão ainda mais a diferença de densidade. |
| Considerações das filiais | A partir do Windows Server versão 1709, você pode especificar uma URL de fallback para um servidor HGS virtualizado em execução localmente como uma VM blindada na filial. A URL de fallback pode ser usada quando a filial perde a conectividade com os servidores HGS no datacenter. Nas versões anteriores do Windows Server, um host Hyper-V em execução em uma filial precisa de conectividade com o serviço guardião de host para ativar ou migrar VMs blindadas ao vivo. Para obter mais informações, consulte [Considerações sobre filiais](guarded-fabric-manage-branch-office.md). |