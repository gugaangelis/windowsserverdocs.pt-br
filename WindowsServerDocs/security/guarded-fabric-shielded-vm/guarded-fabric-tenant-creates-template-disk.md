---
title: VMs blindadas para locatários – criar um disco de modelo - opcional
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874627"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>VMs blindadas para locatários – criar um disco de modelo (opcional)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Para criar uma nova VM blindada, você precisará usar um disco de modelo assinado, especialmente preparado. Metadados de discos de modelo assinado ajuda a garantir que os discos não são modificados após eles terem sido criados e permite que você como um locatário para restringir os discos pode ser usado para criar suas VMs blindadas. É uma maneira de fornecer esse disco para você, o locatário, criá-lo, conforme descrito neste tópico. 

> [!IMPORTANT]
> Se você preferir, você pode usar um disco de modelo fornecido pelo seu provedor de serviços de hospedagem. Se você fizer isso, é importante implantar um teste de VM usando esse disco de modelo e executar suas próprias ferramentas (antivírus, scanners de vulnerabilidade e assim por diante) para validar o disco está, na verdade, em um estado que você confia.

## <a name="prepare-an-operating-system-vhdx"></a>Preparar um VHDX do sistema operacional

Para criar um disco de modelo blindado, você precisa primeiro preparar um disco do sistema operacional que executará o Assistente de disco de modelo. Esse disco será usado como o disco do sistema operacional em VMs blindadas. Você pode usar as ferramentas existentes para criar esse disco, como o Microsoft Desktop imagem Service Manager (DISM), ou configurar uma VM com um VHDX em branco e instalar manualmente o sistema operacional nesse disco. Ao configurar o disco, ele deve cumprir os seguintes requisitos que são específicos para a geração 2 e/ou VMs blindadas: 

| Requisito para VHDX | Motivo |
|-----------|----|
|Deve ser um disco de tabela de partição GUID (GPT) | Necessário para máquinas virtuais de geração 2 deem suporte à UEFI|
|Tipo de disco deve ser **básicas** em vez de **dinâmico**. <br>Observação: Isso se refere ao tipo de disco lógico, não o "expansão dinâmica" recurso VHDX suportado pelo Hyper-V. | O BitLocker não oferece suporte a discos dinâmicos.|
|O disco tem pelo menos duas partições. Uma partição deve incluir a unidade em que o Windows está instalado. Esta é a unidade que o BitLocker criptografará. A outra partição é a partição ativa, que contém o carregador de inicialização e permanecerá não criptografada para que o computador pode ser iniciado.|Necessário para o BitLocker|
|É o sistema de arquivos NTFS | Necessário para o BitLocker|
|O sistema operacional instalado no VHDX é um dos seguintes:<br>– Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>- Windows 10, Windows 8.1, Windows 8| Necessário para dar suporte a máquinas virtuais de geração 2 e o modelo de inicialização segura do Microsoft|
|Sistema operacional deve ser generalizado (execução sysprep.exe) | Modelo de provisionamento envolve especializado VMs para carga de trabalho de um locatário específico| 

> [!NOTE]
> Não copie o disco de modelo para a biblioteca do VMM neste estágio. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Pacote para criar um disco de modelo do Nano Server necessário

Se você estiver planejando executar o Nano Server como seu sistema operacional convidado em VMs blindadas, você deve garantir que sua imagem do Nano Server inclui os seguintes pacotes:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Execute o Windows Update no sistema operacional do modelo

No disco do modelo, verifique se o sistema operacional tem todas as atualizações mais recentes do Windows instaladas. Recentemente, atualizações lançadas melhoram a confiabilidade do processo de blindagem de ponta a ponta - um processo que pode falhar ao se completa, o sistema operacional do modelo não está atualizado.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Assinar e proteger o VHDX com o Assistente de disco de modelo

Para usar um disco de modelo com VMs blindadas, o disco deve ser assinado e criptografado com BitLocker. Para fazer isso, você usará o Assistente de criação de disco modelo blindado. Este assistente irá gerar um hash para o disco e adicioná-lo a um catálogo de assinatura de volume (VSC). O VSC é assinado usando um certificado que você especificar e é usado durante o processo de provisionamento para garantir que o disco que está sendo implantado para um locatário não foi alterado ou substituído por um disco que o locatário não confia. Por fim, o BitLocker é instalado no sistema de operacional do disco (se ele não ainda estiver lá) para preparar o disco para a criptografia durante o provisionamento da VM.

> [!NOTE]
> O Assistente de disco de modelo irá modificar o disco de modelo que você especificar no local. Você talvez queira fazer uma cópia de VHDX desprotegido antes de executar o Assistente para fazer atualizações para o disco em um momento posterior. Você não poderá modificar um disco que foi protegido com o Assistente de disco de modelo.

Execute as seguintes etapas em um computador executando o Windows Server 2016 (não precisa ser um host protegido ou para o servidor do VMM):

1. Copie o VHDX generalizado criado na [preparar um sistema operacional VHDX](#prepare-an-operating-system-vhdx) para o servidor, se ele não ainda estiver lá.

2. Instalar o **ferramentas de VM Blindada** recurso de **ferramentas de administração de servidor remoto** na máquina.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Obter ou criar um certificado para assinar o VHDX que se tornará o disco de modelo para novas VMs blindadas. Detalhes sobre o certificado serão incorporados a um arquivo de dados de blindagem, que autoriza o disco como um disco confiável. Portanto, é importante obter este certificado de autoridade de certificação que você e a hospedagem do serviço a confiança do provedor. Cenários de negócios em que você está o hoster e o locatário, você pode considerar esse certificado de emissão de sua PKI.

    Se você estiver configurando um ambiente de teste e quiser usar um certificado autoassinado para assinar seu disco de modelo, execute um comando semelhante ao seguinte em seu computador:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Iniciar o **Assistente de modelo de disco** da **ferramentas administrativas** pasta no menu Iniciar ou digitando **TemplateDiskWizard.exe** em um prompt de comando.

5. Sobre o **certificado** , clique em **procurar** para exibir uma lista de certificados. Selecione o certificado para assinar o modelo de disco. Clique em **OK** e clique em **Avançar**.

6. Na página de disco Virtual, clique em **navegue** para selecionar o VHDX que você preparou, em seguida, clique em **próxima**.

7. Na página Catálogo de assinatura, forneça um amigável **nome do disco** e **versão.** Esses campos estão presentes para ajudá-lo a identificar o disco depois que ele foi assinado.

    Por exemplo, para **nome do disco** você pode digitar _WS2016_ e para **versão**, _1.0.0.0_

8. Examine suas seleções na página de configurações de análise do assistente. Quando você clica em **gerar**, o Assistente para habilitar o BitLocker no disco do modelo, computar o hash do disco e criar o catálogo de assinatura de Volume, que é armazenado nos metadados do VHDX.

    Aguarde até que o processo de assinatura terminou antes de tentar montar ou mover o disco de modelo. Esse processo pode demorar um pouco para ser concluído, dependendo do tamanho do disco. 

9. Sobre o **resumo** página informações sobre o modelo de disco, o certificado usado para assinar o modelo, e o emissor do certificado é mostrado. Clique em **Fechar** para sair do assistente.


Forneça o modelo blindado de disco para o provedor de serviços de hospedagem, junto com um arquivo de dados de blindagem que você cria, conforme descrito na [criação de dados para definir uma VM blindada de blindagem](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Consulte também

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
