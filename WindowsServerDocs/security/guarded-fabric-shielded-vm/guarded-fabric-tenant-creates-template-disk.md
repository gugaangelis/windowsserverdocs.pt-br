---
title: VMs blindadas para locatários-criando um disco de modelo-opcional
ms.prod: windows-server
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d5cdaf915de94e73374459c41b090f197b8f56ef
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475073"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>VMs blindadas para locatários – criando um disco de modelo (opcional)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Para criar uma nova VM blindada, você precisará usar um disco de modelo assinado especialmente preparado. Os metadados de discos de modelo assinados ajudam a garantir que os discos não sejam modificados após terem sido criados e permite que você restrinja quais discos podem ser usados para criar suas VMs blindadas. Uma maneira de fornecer esse disco é para você, o locatário, para criá-lo, conforme descrito neste tópico.

> [!IMPORTANT]
> Se preferir, você pode usar um disco de modelo fornecido pelo provedor de serviços de hospedagem. Se você fizer isso, será importante implantar uma VM de teste usando esse disco de modelo e executar suas próprias ferramentas (antivírus, verificadores de vulnerabilidade e assim por diante) para validar se o disco é, na verdade, em um estado em que você confia.

## <a name="prepare-an-operating-system-vhdx"></a>Preparar um VHDX do sistema operacional

Para criar um disco de modelo blindado, primeiro você precisa preparar um disco do sistema operacional que será executado por meio do assistente de disco de modelo. Esse disco será usado como o disco do sistema operacional em VMs blindadas. Você pode usar qualquer ferramenta existente para criar esse disco, como o Microsoft Desktop Image Service Manager (DISM), ou configurar manualmente uma VM com um VHDX em branco e instalar o sistema operacional nesse disco. Ao configurar o disco, ele deve aderir aos seguintes requisitos específicos para VMs de geração 2 e/ou blindadas:

| Requisito para VHDX | Motivo |
|-----------|----|
|Deve ser um disco de tabela de partição GUID (GPT) | Necessário para máquinas virtuais de geração 2 para dar suporte a UEFI|
|O tipo de disco deve ser **básico** em oposição ao **dinâmico**. <br>Observação: isso se refere ao tipo de disco lógico, não ao recurso VHDX "de expansão dinâmica" com suporte do Hyper-V. | O BitLocker não dá suporte a discos dinâmicos.|
|O disco tem pelo menos duas partições. Uma partição deve incluir a unidade na qual o Windows está instalado. Esta é a unidade que o BitLocker irá criptografar. A outra partição é a partição ativa, que contém o carregador de inicialização e permanece descriptografada para que o computador possa ser iniciado.|Necessário para o BitLocker|
|O sistema de arquivos é NTFS | Necessário para o BitLocker|
|O sistema operacional instalado no VHDX é um dos seguintes:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necessário para dar suporte a máquinas virtuais de geração 2 e ao modelo de inicialização segura da Microsoft|
|O sistema operacional deve ser generalizado (executar sysprep.exe) | O provisionamento de modelo envolve a especialização de VMs para a carga de trabalho de um locatário específico|

> [!NOTE]
> Não copie o disco de modelo na biblioteca do VMM neste estágio.

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Pacotes necessários para criar um disco de modelo do nano Server

Se você estiver planejando executar o nano Server como seu sistema operacional convidado em VMs blindadas, você deve garantir que a imagem do nano Server inclui os seguintes pacotes:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Executar Windows Update no sistema operacional do modelo

No disco de modelo, verifique se o sistema operacional tem todas as atualizações mais recentes do Windows instaladas. As atualizações lançadas recentemente melhoram a confiabilidade do processo de blindagem de ponta a ponta-um processo que pode falhar ao ser concluído se o sistema operacional do modelo não estiver atualizado.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Assinar e proteger o VHDX com o assistente de disco de modelo

Para usar um disco de modelo com VMs blindadas, o disco deve ser assinado e criptografado com o BitLocker. Para fazer isso, você usará o assistente de criação de disco de modelo blindado. Este assistente irá gerar um hash para o disco e adicioná-lo a um catálogo de assinatura de volume (VSC). O VSC é assinado usando um certificado que você especifica e é usado durante o processo de provisionamento para garantir que o disco que está sendo implantado para um locatário não tenha sido alterado ou substituído por um disco no qual o locatário não confia. Por fim, o BitLocker é instalado no sistema operacional do disco (se ainda não estiver lá) para preparar o disco para criptografia durante o provisionamento da VM.

> [!NOTE]
> O assistente de disco de modelo modificará o disco de modelo que você especificar no local. Talvez você queira fazer uma cópia do VHDX desprotegido antes de executar o assistente para fazer atualizações no disco mais tarde. Você não poderá modificar um disco que tenha sido protegido com o assistente de disco de modelo.

Execute as seguintes etapas em um computador que executa o Windows Server 2016 (não precisa ser um host protegido ou seu servidor do VMM):

1. Copie o VHDX generalizado criado em [preparar um vhdx do sistema operacional](#prepare-an-operating-system-vhdx) para o servidor, se ele ainda não estiver lá.

2. Instale o recurso **ferramentas de VM blindada** do **ferramentas de administração de servidor remoto** no computador.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Obtenha ou crie um certificado para assinar o VHDX que se tornará o disco de modelo para novas VMs blindadas. Os detalhes sobre esse certificado serão incorporados em um arquivo de dados de blindagem, que autoriza o disco como um disco confiável. Portanto, é importante obter esse certificado de uma autoridade de certificação que você e seu provedor de serviços de hospedagem confiam. Em cenários empresariais em que você é o hoster e o locatário, você pode considerar a emissão desse certificado de sua PKI.

    Se você estiver configurando um ambiente de teste e quiser apenas usar um certificado autoassinado para assinar seu disco de modelo, execute um comando semelhante ao seguinte em seu computador:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Inicie o **Assistente de disco de modelo** na pasta **Ferramentas administrativas** no menu iniciar ou digitando **TemplateDiskWizard.exe** em um prompt de comando.

5. Na página **certificado** , clique em **procurar** para exibir uma lista de certificados. Selecione o certificado com o qual assinar o modelo de disco. Clique em **OK** e clique em **Avançar**.

6. Na página disco virtual, clique em **procurar** para selecionar o VHDX que você preparou e clique em **Avançar**.

7. Na página catálogo de assinaturas, forneça um **nome de disco** e uma versão amigáveis **.** Esses campos estão presentes para ajudá-lo a identificar o disco depois que ele tiver sido assinado.

    Por exemplo, para o **nome do disco** , você pode digitar _WS2016_ e, para a **versão**, _1.0.0.0_

8. Examine suas seleções na página Configurações de revisão do assistente. Quando você clica em **gerar**, o assistente habilitará o BitLocker no disco de modelo, computará o hash do disco e criará o catálogo de assinatura de volume, que é armazenado nos metadados VHDX.

    Aguarde até que o processo de assinatura seja concluído antes de tentar montar ou mover o disco de modelo. Esse processo pode demorar um pouco para ser concluído, dependendo do tamanho do disco.

9. Na página **Resumo** , informações sobre o modelo de disco, o certificado usado para assinar o modelo e o emissor do certificado são mostrados. Clique em **Fechar** para sair do assistente.


Forneça o modelo de disco blindado para o provedor de serviços de hospedagem, junto com um arquivo de dados de blindagem que você cria, conforme descrito em [criando dados de blindagem para definir uma VM blindada](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="additional-references"></a>Referências adicionais

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
