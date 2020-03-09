---
title: Visão geral sobre malha protegida e VMs blindadas
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ace6eb30ae6df2dc29aacc05eb7852e03145df4f
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370650"
---
# <a name="guarded-fabric-and-shielded-vms-overview"></a>Visão geral sobre malha protegida e VMs blindadas

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

## <a name="overview-of-the-guarded-fabric"></a>Visão geral da malha protegida

A segurança de virtualização é uma grande área de investimento no Hyper-V. Além de proteger hosts ou outras máquinas virtuais de uma máquina virtual que está executando software mal-intencionado, também é necessário proteger as máquinas virtuais de um host comprometido. Esse é um perigo fundamental para cada plataforma de virtualização hoje, seja ele Hyper-V, VMware ou qualquer outro. De maneira simples, se uma máquina virtual fica fora de uma organização (seja por más intenções ou acidentalmente), essa máquina virtual pode ser executada em qualquer outro sistema. Proteger os ativos de alto valor na sua organização, como controladores de domínio, servidores de arquivos confidenciais e sistemas de RH, é uma prioridade.

Para ajudar a proteger contra a malha de virtualização comprometida, o Hyper-V do Windows Server 2016 introduziu VMs blindadas. Uma VM blindada é uma VM de geração 2 (com suporte no Windows Server 2012 e posterior) que tem um TPM virtual, é criptografada usando o BitLocker e pode ser executada somente em hosts íntegros e aprovados na malha. As VMs blindadas e as malhas protegidas permitem que os provedores de serviço de nuvem ou administradores de nuvens privadas empresariais forneçam um ambiente mais seguro para VMs locatárias. 

Uma malha protegida consiste em:

- 1 Serviço Guardião de Host (HGS) (normalmente, um cluster de 3 nós)
- 1 ou mais hosts protegidos
- Um conjunto de máquinas virtuais blindadas. O diagrama a seguir mostra como o Serviço Guardião de Host usa atestado para garantir que somente hosts válidos e conhecidos possam iniciar as VMs blindadas e a proteção de chave para liberar com segurança as chaves para as VMs blindadas.

Quando um locatário cria VMs blindadas que são executadas em uma malha protegida, os hosts Hyper-V e as VMs blindadas em si são protegidas pelo HGS. O HGS fornece dois serviços distintos: atestado e proteção de chave. O serviço de Atestado garante que somente os hosts Hyper-V confiáveis possam executar VMs blindadas enquanto o Serviço de Proteção de Chave fornece as chaves necessárias para ativá-las e para migrá-las ao vivo para outros hosts protegidos.

![Malha de host protegido](../media/Guarded-Fabric-Shielded-VM/Guarded-Host-Overview-Diagram.png)

## <a name="video-introduction-to-shielded-virtual-machines"></a>Vídeo: introdução às máquinas virtuais blindadas 

<iframe src="https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016/player" width="650" height="440" allowFullScreen frameBorder="0"></iframe>

## <a name="attestation-modes-in-the-guarded-fabric-solution"></a>Modos de Atestado na solução de malha protegida

O HGS dá suporte a diferentes modos de atestado para uma malha protegida:

- Atestado confiável de TPM (baseado em hardware)
- Atestado de chave de host (com base em pares de chaves assimétricas)

O Atestado de TPM confiável é recomendado porque oferece garantias mais fortes, conforme explicado na tabela a seguir, mas exige que os hosts do Hyper-V tenham TPM 2.0. Se atualmente você não tiver o TPM 2,0 ou qualquer TPM, poderá usar o atestado de chave de host. Se decidir mover de Atestado de TPM confiável ao adquirir novo hardware, você poderá alternar o modo de atestado do Serviço Guardião de Host com pouca ou nenhuma interrupção para sua malha.

| **Modo de atestado escolhido para hosts**                                            | **Garantias de host** |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Atestado de TPM confiável:** oferece as proteções mais fortes de possíveis, mas também exigem mais etapas de configuração. O hardware e o firmware do host devem incluir o TPM 2,0 e a UEFI 2.3.1 com a inicialização segura habilitada. | Os hosts protegidos são aprovados com base na identidade do TPM, na sequência de inicialização medida e nas políticas de integridade de código para garantir que eles só executem o código aprovado.| 
| **Atestado de chave de host:** Destina-se a oferecer suporte a hardware de host existente onde o TPM 2,0 não está disponível. Requer menos etapas de configuração e é compatível com o hardware de servidor comum. | Os hosts protegidos são aprovados com base na posse da chave. | 

Outro modo chamado **admin – atestado confiável** foi preterido a partir do Windows Server 2019. Esse modo foi baseado na associação de host protegido em um grupo de segurança de Active Directory Domain Services designado (AD DS). O atestado de chave de host fornece identificação de host semelhante e é mais fácil de configurar. 

## <a name="assurances-provided-by-the-host-guardian-service"></a>As garantias fornecidas pelo Serviço Guardião de Host

HGS, junto com os métodos para a criação de VMs blindadas, ajudam a fornecer as seguintes garantias.

| **Tipo de garantia para VMs**                         | **Garantias de VM blindadas, do serviço de proteção de chave e de métodos de criação para VMs blindadas** |
|----------------------------|--------------------------------------------------|
| **Discos criptografados do BitLocker (discos do so e discos de dados)**   | VMs blindadas usam o BitLocker para proteger seus discos. As chaves do BitLocker necessárias para inicializar a VM e descriptografar os discos são protegidas pelo TPM virtual da VM blindada usando tecnologias comprovadas pelo setor, como a inicialização medida segura. Embora as VMs blindadas apenas criptografem e protejam automaticamente o disco do sistema operacional, você também poderá [criptografar unidades de dados](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-overview) anexadas à VM blindada. |
| **Implantação de novas VMs blindadas de imagens/discos de modelo "confiáveis"** | Ao implantar novas VMs blindadas, os locatários são capazes de especificar em quais discos modelo confiam. Os discos modelos blindados têm assinaturas que são calculadas em um ponto no tempo quando seu conteúdo é considerado confiável. As assinaturas do disco são armazenadas em um catálogo de assinatura, que locatários fornecem com segurança à malha ao criar VMs blindadas. Durante o provisionamento das VMs blindadas, a assinatura do disco é computada novamente e comparada com as assinaturas confiáveis no catálogo. Se as assinaturas forem correspondentes, a máquina virtual blindada estará implantada. Se as assinaturas não corresponderem, o disco de modelo blindado será considerado não confiável e a implantação falha. |
| **Proteção de senhas e outros segredos quando uma VM blindada é criada** | Ao criar VMs, é necessário garantir que os segredos da VM, como as assinaturas de disco confiável, os certificados RDP e a senha da conta de administrador local da VM, não sejam divulgados para a malha. Esses segredos são armazenados em um arquivo criptografado chamado de arquivo de dados de blindagem (um arquivo .PDK), que é protegido por chaves de locatário e carregado para a malha pelo locatário. Quando uma máquina virtual blindada é criada, o locatário seleciona os dados de blindagem para usar que fornece com segurança esses segredos somente para os componentes confiáveis na malha protegida. |
| **Controle de locatário de onde a VM pode ser iniciada** | Os dados de blindagem também contêm uma lista das malhas protegidas nas quais uma VM blindada específica tem permissão para executar. Isso é útil, por exemplo, em casos em que uma máquina virtual blindada geralmente reside em uma nuvem privada local, mas talvez precise ser migrada para outra nuvem (pública ou privada) para fins de recuperação de desastres. A malha ou nuvem de destino deve suportar VMs blindadas e a VM blindada deve permitir que a malha a execute. |

## <a name="what-is-shielding-data-and-why-is-it-necessary"></a>O que são dados de blindagem e por que isso é necessário?

Um arquivo de dados de blindagem (também chamado de um arquivo de dados de provisionamento ou arquivo PDK) é um arquivo criptografado que um locatário ou proprietário de VM cria para proteger as informações de configuração de VM importantes, como a senha do administrador, RDP e outros certificados relacionados à identidade, credenciais de associação de domínio e assim por diante. Um administrador de malha usa o arquivo de dados de blindagem ao criar uma máquina virtual blindada, mas não consegue exibir ou usar as informações contidas no arquivo.

Entre outros, um arquivo de dados de blindagem contém segredos como:

- Credenciais do administrador
- Um arquivo de resposta (unattend.xml)
- Uma política de segurança que determina se as VMs criadas usando esses dados de blindagem estão configuradas como blindadas ou com suporte para criptografia
    - Lembre-se de que máquinas virtuais configuradas como blindadas são protegidas contra administradores de malha enquanto que as VMs com suporte de criptografia não são
- Um certificado RDP para proteger a comunicação da área de trabalho remota com a máquina virtual
- Um catálogo de assinatura de volume que contém uma lista de assinaturas de disco modelo confiável e assinada a partir da qual uma nova VM tem permissão para ser criada
- Um protetor de chave (ou KP) que define em quais malhas protegidas uma máquina virtual blindada está autorizada a executar

O arquivo de dados de blindagem (arquivo PDK) fornece garantias de que a VM será criada da maneira que locatário desejava. Por exemplo, quando o locatário coloca um arquivo de resposta (unattend.xml) no arquivo de dados de blindagem e o entrega para o provedor de hospedagem, o provedor de hospedagem não pode exibir ou fazer alterações nesse arquivo de resposta. Da mesma forma, o provedor de hospedagem não pode substituir um VHDX diferente ao criar a VM blindada, porque o arquivo de dados de blindagem contém as assinaturas dos discos confiáveis a partir das quais as VMs blindadas podem ser criadas.

A figura a seguir mostra o arquivo de dados de blindagem e os elementos de configuração relacionados.

![Arquivo de dados de blindagem](../media/Guarded-Fabric-Shielded-VM/shielded-vms-shielding-data-file.png)

## <a name="what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run"></a>Quais são os tipos de máquinas virtuais que podem ser executados por uma malha protegida?

Malhas protegidas são capazes de executar VMs de três maneiras:

1.  Uma VM normal que não oferece proteções além de versões anteriores do Hyper-V
2.  Uma VM com suporte para criptografia cujas proteções podem ser configuradas por um administrador de malha
3.  Uma VM blindada cujas proteções estão todas ligadas e não podem ser desabilitadas por um administrador de malha

VMs com suporte para criptografia destinam-se ao uso onde os administradores da malha são totalmente confiáveis.  Por exemplo, uma empresa pode implantar uma malha protegida para garantir que os discos de VM estejam criptografados em repouso para fins de conformidade. Os administradores da malha podem continuar a usar os recursos de gerenciamento convenientes, por exemplo, conexões de console de VM, PowerShell Direct e outras ferramentas de gerenciamento e solução de problemas diários.

As VMs blindadas destinam-se ao uso em malhas onde os dados e o estado da máquina virtual devem ser protegidas dos administradores de malha e de softwares não confiáveis que possam estar em execução nos hosts do Hyper-V. Por exemplo, VMs blindadas nunca permitirão uma conexão de console da máquina virtual, enquanto que um administrador de malha pode ativar ou desativar essa proteção para VMs de criptografia compatíveis.

A tabela a seguir resume as diferenças entre as VMs blindadas e com suporte de criptografia.

| Capability        | Suporte à criptografia de geração 2     | Blindado de geração 2         |
|----------|--------------------|----------------|
|Inicialização Segura        | Sim, obrigatória mas configurável        | Sim, obrigatória e imposta    |
|Vtpm               | Sim, obrigatória mas configurável        | Sim, obrigatória e imposta    |
|Criptografar o estado da VM e o tráfego da migração ao vivo | Sim, obrigatória mas configurável |  Sim, obrigatória e imposta  |
|Componentes de integração | Configurável pelo administrador da malha      | Alguns componentes de integração bloqueados (por exemplo, troca de dados, PowerShell Direct) |
|Conexão de máquina virtual (Console), dispositivos HID (por exemplo, teclado, mouse) | Ligados, não podem ser desabilitados | Habilitado nos hosts que começam com o Windows Server versão 1803; Desabilitado em hosts anteriores |
|Portas seriais/COM   | Com suporte                             | Desabilitados (não podem ser habilitados) |
|Anexar um depurador (ao processo da VM)<sup>1</sup>| Com suporte          | Desabilitados (não podem ser habilitados) |

<sup>1</sup> os depuradores tradicionais que são anexados diretamente a um processo, como o WinDbg. exe, são bloqueados para VMs blindadas porque o processo de trabalho da VM (VMWP. exe) é uma ppl (sinal de processo protegido). Técnicas de depuração alternativas, como as usadas pelo LiveKd. exe, não são bloqueadas. Ao contrário das VMs blindadas, o processo de trabalho para as VMs com suporte para criptografia não é executado como uma PPL, de forma que os depuradores tradicionais, como o WinDbg. exe, continuarão a funcionar normalmente. 

As VMs blindadas e as VMs com suporte à criptografia continuam a oferecer suporte a recursos de gerenciamento de malha comuns, como migração ao vivo, réplica do Hyper-V, pontos de verificação de máquina virtual e assim por diante.

## <a name="the-host-guardian-service-in-action-how-a-shielded-vm-is-powered-on"></a>O Serviço Guardião de Host em ação: como uma máquina virtual blindada é ligada

![Arquivo de dados de blindagem](../media/Guarded-Fabric-Shielded-VM/shielded-vms-how-a-shielded-vm-is-powered-on.png)

1. VM01 está ligado.

    Antes de um host protegido possa ligar uma VM blindada, ele deverá primeiro ser atestado afirmativamente de que esteja íntegro. Para provar que está íntegro, ele deverá apresentar um certificado de integridade para o Serviço de Proteção de Chave (KPS). O certificado de integridade é obtido através do processo de atestado.

2. Hosts exigem atestado.

    O host protegido exige atestado. O modo de atestado é determinado pelo Serviço Guardião de Host:

    **Atestado confiável de TPM**: o host Hyper-V envia informações que incluem:

       - Informações de identificação do TPM (sua chave de endosso)
       - Informações sobre os processos que foram iniciados durante a sequência de inicialização mais recente (o log do TCG)
       - Informações sobre a política de integridade de código (CI) que foi aplicada no host. 

       Attestation happens when the host starts and every 8 hours thereafter. If for some reason a host doesn't have an attestation certificate when a VM tries to start, this also triggers attestation.

    **Atestado de chave de host**: o host Hyper-V envia a metade pública do par de chaves. O HGS valida que a chave de host está registrada. 
    
    **Atestado de admin confiável**: o host do Hyper-V envia um tíquete Kerberos, que identifica os grupos de segurança em que o host está. O HGS valida que o host pertence a um grupo de segurança que foi configurado anteriormente pelo administrador de HGS confiável.

3. O atestado tem êxito (ou falha).

    O modo de atestado determina quais verificações são necessárias para atestar com êxito que o host está íntegro. Com o atestado confiável do TPM, a identidade do TPM do host, as medidas de inicialização e a política de integridade de código são validadas. Com o atestado de chave do host, somente o registro da chave do host é validado. 

4. Certificado de atestado enviado ao host.

    Supondo que o atestado foi bem-sucedido, um certificado de integridade é enviado ao host e o host é considerado "protegido" (autorizado a executar VMs blindadas). O host usa o certificado de integridade para autorizar o Serviço de Proteção de Chave para liberar com segurança as chaves necessárias para trabalhar com VMs blindadas

5. O host solicita chave de máquina virtual.

    O host protegido não tem as chaves necessárias para ligar uma VM blindada (VM01 neste caso). Para obter as chaves necessárias, o host protegido deve fornecer o seguinte para o KPS:

    - O certificado de integridade atual
    - Um segredo criptografado (um Protetor de Chave ou KP) que contém as chaves necessárias para ligar a VM01. O segredo é criptografado usando outras chaves que somente o KPS conhece.

6. Versão da chave.

    O KPS examina o certificado de integridade para determinar sua validade. O certificado não deve ter expirado e o KPS deve confiar no serviço de atestado que o emitiu.

7. A chave é retornada ao host.

    Se o certificado de integridade é válido, o KPS tenta descriptografar o segredo e retornar com segurança as chaves necessárias para ligar a VM. Observe que as chaves são criptografadas para o VBS do host protegido.

8. O host liga a VM01.

## <a name="guarded-fabric-and-shielded-vm-glossary"></a>Glossário de malha protegida e VMs blindadas

| Termo              | Definição           |
|----------|------------|
| Serviço Guardião de Host (HGS) | Uma função do Windows Server que é instalada em um cluster protegido de servidores de bare-metal que é capaz de medir a integridade de um host do Hyper-V e liberar chaves para hosts do Hyper-V íntegros ao ligar ou migrar ao vivo VMs blindadas. Esses dois recursos são fundamentais para uma solução de máquina virtual blindada e são chamados de **Serviço de atestado** e **Serviço de Proteção de Chave** respectivamente. |
| host protegido | Um host do Hyper-V no qual as VMs blindadas podem ser executadas. Um host só pode ser considerado _protegido_ quando foi considerado Íntegro pelo serviço de atestado do HgS. As VMs blindadas não podem ser ligadas ou migradas ao vivo para um host do Hyper-V que ainda não tiver sido atestado ou que tiver atestado falho. |
| malha protegida    | Esse é o termo coletivo usado para descrever uma malha de hosts do Hyper-V e o Serviço Guardião de Host que tem a capacidade de gerenciar e executar VMs blindadas. |
| máquinas virtuais blindadas (VM) | Uma máquina virtual que pode ser executada somente em hosts protegidos e está protegida contra a inspeção, violação e roubo de administradores de malha mal-intencionados e malware de host. |
| administrador de malha | Um administrador de nuvem pública ou privada que pode gerenciar máquinas virtuais. No contexto de uma malha protegida, um administrador de malha não tem acesso a VMs blindadas ou às políticas que determinam em quais hosts as VMs blindadas podem ser executadas. |
| administrador de HGS | Um administrador confiável na nuvem pública ou privada que tem autoridade para gerenciar as políticas e o material criptográfico para hosts protegidos, ou seja, os hosts nos quais uma máquina virtual blindada pode ser executada.|
| arquivo de dados de provisionamento ou arquivo de dados de blindagem (arquivo PDK) | Um arquivo criptografado que um locatário ou usuário cria para guardar informações importantes de configuração de VM e proteger essas informações contra o acesso por outros usuários. Por exemplo, um arquivo de dados de blindagem pode conter a senha que será atribuída à conta de administrador local quando a VM é criada. |
| Segurança com Base em virtualização (VBS) | Um ambiente de armazenamento e processamento baseado em Hyper-V protegido por administradores. O Modo Seguro Virtual fornece ao sistema a capacidade de armazenar as chaves do sistema operacional que não são visíveis para um administrador do sistema operacional.|
| TPM virtual | Uma versão virtualizada de um Trusted Platform Module (TPM). Começando com o Hyper-V no Windows Server 2016, você pode fornecer um dispositivo virtual TPM 2,0 para que as máquinas virtuais possam ser criptografadas, assim como um TPM físico permite que um computador físico seja criptografado.|

## <a name="see-also"></a>Consulte também

- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
- Blog: [blog de datacenter e segurança de nuvem privada](https://blogs.technet.microsoft.com/datacentersecurity/)
- Vídeo: [introdução às máquinas virtuais blindadas](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vídeo: [Aprofunde-se em VMs blindadas com o Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
