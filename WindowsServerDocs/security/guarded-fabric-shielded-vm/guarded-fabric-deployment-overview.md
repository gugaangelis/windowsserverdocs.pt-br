---
title: Início rápido para a implantação de malha protegida
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d63726e7046896c9ef7aa0c3b3d85237bc3f788d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879057"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Início rápido para a implantação de malha protegida

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico explica o que é uma malha protegida, seus requisitos e um resumo do processo de implantação. Para obter etapas detalhadas de implantação, consulte [implantar o serviço guardião de Host para hosts protegidos e VMs blindadas](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

Prefere vídeo? Consulte o curso do Microsoft Virtual Academy [implantar VMs Blindadas e a uma malha protegida com o Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>O que é uma malha protegida

Um _protegidos do fabric_ é uma malha do Windows Server 2016 Hyper-V capazes de proteger as cargas de trabalho do locatário contra inspeção, roubo e violação contra malware em execução no host, bem como dos administradores de sistema. Essas cargas de trabalho de locatário de virtualizadas — protegidos em repouso e em trânsito — são chamados _VMs blindadas_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>Quais são os requisitos para uma malha protegida

Os requisitos para uma malha protegida incluem:

- **Um lugar para executar blindado VMs que esteja livre de softwares mal-intencionados.**

    Eles são chamados _hosts protegidos_. 
    Hosts protegidos são hosts Hyper-V do Windows Server 2016 Datacenter edition que podem executar VMs blindadas apenas se eles podem provar que estão sendo executados em um estado conhecido e confiável para uma autoridade externa chamada serviço de guardião de Host (HGS). 
    O HGS é uma nova função de servidor no Windows Server 2016 e normalmente é implantado como um cluster de três nós. 

- **Uma maneira de verificar se um host está em um estado íntegro.**

    Executa o HGS _Atestado_, onde ela mede a integridade de hosts protegidos.

- **Um processo para liberar com segurança as chaves para hosts íntegros.**

    Executa o HGS _proteção e a versão de chave da chave_, onde ele a liberará as chaves de volta para os hosts íntegros.

- **Ferramentas de gerenciamento para automatizar o provisionamento seguro e hospedagem de VMs blindadas.**

    Opcionalmente, você pode adicionar essas ferramentas de gerenciamento para uma malha protegida:

    - System Center 2016 Virtual Machine Manager (VMM). O VMM é recomendado porque fornece ferramentas além de usar apenas os cmdlets do PowerShell que vêm com o Hyper-V e as cargas de trabalho de malha protegida, você obtém de gerenciamento adicional).
    - System Center 2016 Service Provider Foundation (SPF). Isso é uma camada de API entre o Windows Azure Pack e o VMM e um pré-requisito para usar o Windows Azure Pack.
    - Pacote do Windows Azure fornece uma interface boa web gráfica para gerenciar uma malha protegida e VMs blindadas. 

Na prática, uma decisão deve ser feita com antecedência: o [modo de Atestado](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) usado pela malha protegida. Há dois meios — dois modos mutuamente exclusivos — por qual HGS pode medir que um host Hyper-V está íntegro. Quando você inicializa o HGS, você precisa escolher o modo:  

- Atestado de chaves do host ou o modo de chave, é menos seguro, mas o mais fácil adotar  
- Atestado de TPM ou modo TPM, é mais seguro, mas requer mais configuração e hardware específico

Se necessário, você pode implantar em modo de chave usando hosts Hyper-V existentes que foram atualizados para o Windows Server 2019 Datacenter edition e, em seguida, converter para o modo mais seguro do TPM quando que dão suporte a hardware de servidor (incluindo TPM 2.0) está disponível. 

Agora que você sabe quais são as partes, vamos examinar um exemplo de modelo de implantação.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Como obter de uma malha de Hyper-V atual para uma malha protegida

Vamos imaginar que esse cenário, você tem a estrutura do Hyper-V existente, como Contoso.com na imagem a seguir, e você deseja criar uma malha protegida do Windows Server 2016.

![Malha de Hyper-V existente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Etapa 1: Implante os hosts do Hyper-V executando o Windows Server 2016 

Os hosts do Hyper-V precisam executar o Windows Server 2016 Datacenter edition ou posterior. Se você estiver atualizando os hosts, você poderá [atualizar](https://technet.microsoft.com/windowsserver/dn527667.aspx) do Standard edition para o Datacenter edition.

![Atualizar hosts Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Etapa 2: Implantar o serviço guardião de Host (HGS)

Em seguida, instalar a função de servidor HGS e implantá-lo como um cluster de três nós, semelhante ao exemplo relecloud.com na imagem a seguir. Isso exige três cmdlets do PowerShell:

- Para adicionar a função HGS, use `Install-WindowsFeature` 
- Para instalar o HGS, use `Install-HgsServer` 
- Para inicializar o HGS com seu modo escolhido de Atestado, use `Initialize-HgsServer` 

Se seus servidores existentes do Hyper-V não cumprir os pré-requisitos para o modo TPM (por exemplo, eles não tiverem TPM 2.0), você pode inicializar o HGS usando o administrador com base em Atestado (modo AD), que requer uma relação de confiança do Active Directory com o domínio de malha. 

Em nosso exemplo, digamos que a Contoso inicialmente implantado em um modo AD para atender a requisitos de conformidade imediatamente e planos para converter para o atestado de mais seguro e baseado em TPM depois que o hardware de servidor adequado pode ser adquirido. 

![Instalar o HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Etapa 3: Extrair identidades, linhas de base de hardware e as políticas de integridade de código

O processo para extrair as identidades de hosts Hyper-V depende do modo de Atestado que está sendo usado.

Para o modo do AD, a ID do host é sua conta de computador ingressado no domínio, o que deve ser um membro de um grupo de segurança designado no domínio do fabric.
A associação no grupo designado é a determinação somente se o host está íntegro ou não. 

Nesse modo, o administrador da malha é exclusivamente responsável por garantir a integridade dos hosts do Hyper-V. Uma vez que o HGS não desempenha nenhuma parte decidir o que é ou não tem permissão para executar, malware e depuradores funcionará como projetado. 

No entanto, os depuradores que tentam conectar-se diretamente a um processo (como WinDbg.exe) são bloqueadas para VMs blindadas porque o processo de trabalho da VM (VMWP.exe) é uma luz de processo protegido (PPL).
Técnicas de depuração alternativas, como os usados pelo LiveKd.exe, não são bloqueadas. Ao contrário de VMs blindadas, o processo de trabalho para VMs com suporte de criptografia não é executado como um PPL depuradores tradicionais como WinDbg.exe continuarão a funcionar normalmente.

Indicado de outra forma, as etapas de validação rigoroso usadas para o modo TPM não são usadas para o modo de AD de nenhuma forma.

Para o modo TPM, são necessárias três coisas: 

1.  Um _chave de endosso público_ (ou _EKpub_) do que o TPM 2.0 em cada host do Hyper-V. Para capturar o EKpub, use `Get-PlatformIdentifier`. 
2.  Um _linha de base de hardware_. Se cada um dos seus hosts Hyper-V forem idêntica, uma única linha de base é tudo o que você precisa. Se não forem, será necessário um para cada classe de hardware. A linha de base está na forma de um arquivo de log do grupo de computação confiável, ou TCGlog. O TCGlog contém tudo o que fez o host, do firmware UEFI, por meio do kernel, até onde o host está totalmente inicializado. Para capturar a linha de base de hardware, instale a função Hyper-V e o recurso de suporte de Hyper-V de guardião de Host e usar `Get-HgsAttestationBaselinePolicy`. 
3.  Um _política de integridade de código_. Se cada um dos seus hosts Hyper-V forem idêntica, uma única política de CI é tudo o que você precisa. Se não forem, será necessário um para cada classe de hardware. Windows Server 2016 e Windows 10 tem um novo formulário de imposição de políticas de CI chamado _integridade de código de imposto de hipervisor (HVAC)_. HVAC fornece imposição de alta segurança e garante que um host só tem permissão para executar binários que um administrador confiável permitiu que ele seja executado. Essas instruções são encapsuladas em uma política de CI é adicionada ao HGS. HGS mede a política de CI do cada host antes de eles estiverem permissão para executar VMs blindadas. Para capturar uma política de CI, use `New-CIPolicy`. A política deve ser convertida em seu formato binário usando `ConvertFrom-CIPolicy`.

![Extrair identidades, a linha de base e a política de CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Isso é tudo — a malha protegida baseia-se, em termos de infraestrutura para executá-lo.  
Agora você pode criar um disco de modelo VM blindado e um arquivo de dados de blindagem então blindado VMs podem ser provisionadas com segurança e simplicidade. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Etapa 4: Criar um modelo para as VMs blindadas

Um modelo VM blindado protege os discos de modelo Criando uma assinatura do disco em um ponto confiável conhecido no tempo. 
Se o disco de modelo mais tarde for infectado por malware, sua assinatura será diferente de modelo original, que será detectado pela processo de provisionamento da VM blindada segura. 
Discos de modelo Blindadas são criados executando o **Assistente de criação de disco de modelo blindado** ou `Protect-TemplateDisk` em relação a um disco de modelo regular. 

Cada um está incluído com o **ferramentas de VM Blindada** recurso nas [Remote Server Administration ferramentas para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Depois de baixar o RSAT, execute este comando para instalar o **ferramentas de VM Blindada** recurso:

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Um administrador confiável, como o administrador de malha ou o proprietário da VM, será necessário um certificado (geralmente é fornecido por um provedor de serviços de hospedagem) para assinar o disco de VHDX do modelo. 

![Assistente de disco modelo blindado](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

A assinatura do disco é calculada sobre a partição do sistema operacional do disco virtual.
Se houver alguma alteração na partição do sistema operacional, a assinatura também serão alterados.
Isso permite que os usuários altamente identificar quais discos que eles confiam, especificando a assinatura apropriada.

Examine os [requisitos de disco de modelo](guarded-fabric-create-a-shielded-vm-template.md) antes de começar. 

## <a name="step-5-create-a-shielding-data-file"></a>Etapa 5: Criar um arquivo de dados de blindagem 

Um arquivo de dados blindagem, também conhecido como um arquivo. PDK, captura informações confidenciais sobre a máquina virtual, como a senha de administrador. 

![Os dados de blindagem](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

O arquivo de dados de blindagem também inclui a configuração de política de segurança para a VM blindada. Você deve escolher uma das duas políticas de segurança quando você cria um arquivo de dados de blindagem:

- Blindado
   
    A opção mais segura, que elimina muitos vetores de ataque administrativo.

- Criptografia com suporte

    Um menor nível de proteção que ainda fornece os benefícios de conformidade de ser capaz de criptografar uma VM, mas permite que os administradores do Hyper-V fazer coisas como usar a conexão de console da VM e o PowerShell Direct. 

    ![Nova criptografia de VM com suporte](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Você pode adicionar partes de opcional para gerenciamento como o VMM ou o Windows Azure Pack. Se você gostaria de criar uma VM sem instalar essas peças, consulte [passo a passo – criação de VMs Blindadas sem o VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Etapa 6: Criar uma VM blindada

Criação de máquinas virtuais blindadas difere muito pouco de máquinas de virtuais regulares. No Windows Azure Pack, a experiência é ainda mais fácil de criar uma VM comum, pois você só precisa fornecer um nome de arquivo de dados (que contém o restante das informações de especialização) e a rede VM de blindagem. 

![Nova VM blindada no Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Pré-requisitos do HGS](guarded-fabric-prepare-for-hgs.md)
