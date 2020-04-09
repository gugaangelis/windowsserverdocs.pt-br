---
title: Início rápido para implantação de malha protegida
ms.prod: windows-server
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: c0e29abf14ff1dded12e7e20a0c0a74f80a91d8e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856739"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Início rápido para implantação de malha protegida

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico explica o que é uma malha protegida, seus requisitos e um resumo do processo de implantação. Para obter etapas de implantação detalhadas, consulte [implantando o serviço guardião de host para hosts protegidos e VMs blindadas](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

Prefere vídeo? Confira o curso Microsoft Virtual Academy [implantando VMs blindadas e uma malha protegida com o Windows Server 2016](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>O que é uma malha protegida

Uma _malha protegida_ é uma malha do Hyper-V do Windows Server 2016 capaz de proteger cargas de trabalho de locatário contra inspeção, roubo e adulteração de malware em execução no host, bem como de administradores do sistema. Essas cargas de trabalho de locatário virtualizadas — protegidas em repouso e em trânsito — são chamadas de _VMs blindadas_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quais são os requisitos para uma malha protegida

Os requisitos para uma malha protegida incluem:

- **Um local para executar VMs blindadas gratuitas de softwares mal-intencionados.**

    Eles são chamados de _hosts protegidos_. 
    Os hosts protegidos são hosts do Hyper-V do Windows Server 2016 Datacenter Edition que podem executar VMs blindadas somente se puderem provar que estão em execução em um estado conhecido e confiável para uma autoridade externa chamada de serviço guardião de host (HGS). 
    O HGS é uma nova função de servidor no Windows Server 2016 e normalmente é implantado como um cluster de três nós. 

- **Uma maneira de verificar se um host está em um estado íntegro.**

    O HGS executa o _atestado_, onde mede a integridade dos hosts protegidos.

- **Um processo para liberar chaves com segurança para hosts íntegros.**

    O HGS executa a _proteção de chave e a versão de chave_, onde libera as chaves de volta para os hosts íntegros.

- **Ferramentas de gerenciamento para automatizar o provisionamento seguro e a hospedagem de VMs blindadas.**

    Opcionalmente, você pode adicionar essas ferramentas de gerenciamento a uma malha protegida:

    - System Center 2016 Virtual Machine Manager (VMM). O VMM é recomendado porque fornece ferramentas de gerenciamento adicionais além das que você obtém usando apenas os cmdlets do PowerShell que vêm com o Hyper-V e as cargas de trabalho de malha protegidas).
    - SPF (System Center 2016 Service Provider Foundation). Essa é uma camada de API entre Pacote do Microsoft Azure e VMM e um pré-requisito para usar Pacote do Microsoft Azure.
    - O Pacote do Microsoft Azure fornece uma boa interface gráfica da Web para gerenciar uma malha protegida e VMs blindadas. 

Na prática, uma decisão deve ser feita antecipadamente: o [modo de atestado](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) usado pela malha protegida. Há dois meios: dois modos mutuamente exclusivos, pelos quais o HGS pode medir que um host Hyper-V está íntegro. Ao inicializar o HGS, você precisa escolher o modo:  

- O atestado de chave de host, ou o modo de chave, é menos seguro, mas mais fácil de adotar  
- O atestado baseado em TPM, ou o modo TPM, é mais seguro, mas requer mais configuração e hardware específico

Se necessário, você pode implantar no modo de chave usando hosts Hyper-V existentes que foram atualizados para o Windows Server 2019 Datacenter Edition e, em seguida, converter para o modo TPM mais seguro ao oferecer suporte a hardware de servidor (incluindo TPM 2,0) está disponível. 

Agora que você sabe quais são as partes, vamos examinar um exemplo do modelo de implantação.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Como obter de uma malha atual do Hyper-V para uma malha protegida

Vamos imaginar esse cenário — você tem uma malha Hyper-V existente, como Contoso.com na imagem a seguir, e deseja criar uma malha protegida do Windows Server 2016.

![Malha do Hyper-V existente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Etapa 1: implantar os hosts Hyper-V que executam o Windows Server 2016 

Os hosts do Hyper-V precisam executar o Windows Server 2016 Datacenter Edition ou posterior. Se você estiver atualizando hosts, poderá [Atualizar](https://technet.microsoft.com/windowsserver/dn527667.aspx) do Standard Edition para o Datacenter Edition.

![Atualizar hosts do Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Etapa 2: implantar o serviço guardião de host (HGS)

Em seguida, instale a função de servidor HGS e implante-a como um cluster de três nós, como o exemplo de relecloud.com na imagem a seguir. Isso requer três cmdlets do PowerShell:

- Para adicionar a função HGS, use `Install-WindowsFeature` 
- Para instalar o HGS, use `Install-HgsServer` 
- Para inicializar o HGS com o modo escolhido de atestado, use `Initialize-HgsServer` 

Se os servidores Hyper-V existentes não atenderem aos pré-requisitos do modo TPM (por exemplo, se não tiverem o TPM 2,0), você poderá inicializar o HGS usando o atestado baseado em administrador (modo AD), que exige uma confiança Active Directory com o domínio de malha. 

Em nosso exemplo, vamos dizer que a contoso inicialmente implanta no modo AD para atender imediatamente aos requisitos de conformidade e planeja converter para o atestado mais seguro baseado em TPM depois que o hardware de servidor adequado puder ser adquirido. 

![Instalar o HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Etapa 3: extrair identidades, linhas de base de hardware e políticas de integridade de código

O processo para extrair identidades de hosts Hyper-V depende do modo de atestado usado.

Para o modo AD, a ID do host é sua conta de computador ingressada no domínio, que deve ser membro de um grupo de segurança designado no domínio da malha.
A associação no grupo designado é a única determinação de se o host está íntegro ou não. 

Nesse modo, o administrador da malha é exclusivamente responsável por garantir a integridade dos hosts Hyper-V. Como o HGS não exerce nenhuma parte na decisão de o que é ou não tem permissão para ser executado, malware e depuradores funcionarão conforme projetado. 

No entanto, os depuradores que tentam se conectar diretamente a um processo (como o WinDbg. exe) são bloqueados para VMs blindadas porque o processo de trabalho da VM (VMWP. exe) é uma PPL (sinal de processo protegido).
Técnicas de depuração alternativas, como as usadas pelo LiveKd. exe, não são bloqueadas. Ao contrário das VMs blindadas, o processo de trabalho para as VMs com suporte para criptografia não é executado como uma PPL, de forma que os depuradores tradicionais, como o WinDbg. exe, continuarão a funcionar normalmente.

Em outras palavras, as etapas rigorosas de validação usadas para o modo TPM não são usadas para o modo AD de forma alguma.

Para o modo TPM, são necessárias três coisas: 

1.    Uma _chave de endosso pública_ (ou _EKPUB_) do TPM 2,0 em cada e em cada host do Hyper-V. Para capturar o EKpub, use `Get-PlatformIdentifier`. 
2.    Uma _linha de base de hardware_. Se cada um dos seus hosts Hyper-V for idêntico, uma única linha de base será tudo de que você precisa. Se não forem, você precisará de uma para cada classe de hardware. A linha de base está na forma de um arquivo de log do grupo de computação confiável ou TCGlog. O TCGlog contém tudo o que o host fez, do firmware UEFI, por meio do kernel, até onde o host é totalmente inicializado. Para capturar a linha de base do hardware, instale a função Hyper-V e o recurso de suporte do Hyper-V do guardião de host e use `Get-HgsAttestationBaselinePolicy`. 
3.    Uma _política de integridade de código_. Se cada um dos hosts do Hyper-V for idêntico, uma única política de CI será tudo de que você precisa. Se não forem, você precisará de uma para cada classe de hardware. O Windows Server 2016 e o Windows 10 têm uma nova forma de imposição de políticas de CI, chamada _política HVAC (integridade de código imposta) do hipervisor_. O política HVAC fornece uma forte imposição e garante que um host só tenha permissão para executar binários que um administrador confiável permitiu a execução. Essas instruções são encapsuladas em uma política de CI que é adicionada ao HGS. O HGS mede a política de CI de cada host antes que tenha permissão para executar VMs blindadas. Para capturar uma política de CI, use `New-CIPolicy`. A política deve então ser convertida em seu formato binário usando `ConvertFrom-CIPolicy`.

![Extrair identidades, linha de base e política de CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Isso é tudo: a malha protegida é criada, em termos da infraestrutura para executá-la.  
Agora você pode criar um disco de modelo de VM blindada e um arquivo de dados de blindagem para que as VMs blindadas possam ser provisionadas de forma simples e segura. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Etapa 4: criar um modelo para VMs blindadas

Um modelo de VM blindada protege os discos de modelo criando uma assinatura do disco em um ponto de confiança conhecido no tempo. 
Se o disco de modelo for posteriormente infectado por malware, sua assinatura será diferente do modelo original que será detectado pelo processo de provisionamento de VM blindado seguro. 
Os discos de modelo blindado são criados pela execução do **Assistente de criação de disco de modelo blindado** ou `Protect-TemplateDisk` em um disco de modelo normal. 

Cada um está incluído com o recurso **ferramentas de VM blindada** no [ferramentas de administração de servidor remoto para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Depois de baixar o RSAT, execute este comando para instalar o recurso de **ferramentas de VM blindada** :

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Um administrador confiável, como o administrador de malha ou o proprietário da VM, precisará de um certificado (geralmente fornecido por um provedor de serviços de hospedagem) para assinar o disco do modelo VHDX. 

![Assistente de disco de modelo blindado](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

A assinatura do disco é computada na partição do sistema operacional do disco virtual.
Se algo for alterado na partição do sistema operacional, a assinatura também será alterada.
Isso permite que os usuários identifiquem fortemente em quais discos eles confiam especificando a assinatura apropriada.

Examine os [requisitos de disco do modelo](guarded-fabric-create-a-shielded-vm-template.md) antes de começar. 

## <a name="step-5-create-a-shielding-data-file"></a>Etapa 5: criar um arquivo de dados de blindagem 

Um arquivo de dados de blindagem, também conhecido como arquivo. PDK, captura informações confidenciais sobre a máquina virtual, como a senha do administrador. 

![Dados de blindagem](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

O arquivo de dados de blindagem também inclui a configuração de política de segurança para a VM blindada. Você deve escolher uma das duas políticas de segurança ao criar um arquivo de dados de blindagem:

- Blindadas
   
    A opção mais segura, que elimina muitos vetores de ataque administrativo.

- Criptografia com suporte

    Um nível inferior de proteção que ainda fornece os benefícios de conformidade de ser capaz de criptografar uma VM, mas permite que os administradores do Hyper-V façam coisas como usar a conexão do console da VM e o PowerShell Direct. 

    ![Nova VM com suporte de criptografia](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Você pode adicionar partes de gerenciamento opcionais, como VMM ou Pacote do Microsoft Azure. Se você quiser criar uma VM sem instalar essas partes, consulte [passo a passo – criando VMs blindadas sem VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Etapa 6: criar uma VM blindada

A criação de máquinas virtuais blindadas difere muito pouco das máquinas virtuais normais. Em Pacote do Microsoft Azure, a experiência é ainda mais fácil do que criar uma VM regular, pois você só precisa fornecer um nome, um arquivo de dados de blindagem (contendo o restante das informações de especialização) e a rede VM. 

![Nova VM blindada no Pacote do Microsoft Azure](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Pré-requisitos HGS](guarded-fabric-prepare-for-hgs.md)
