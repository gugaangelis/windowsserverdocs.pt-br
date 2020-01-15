---
title: Criar um disco de modelo de VM blindada do Windows
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 04fdd52544b69d2c41abcbee00dd00b31bf5f21c
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949782"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Criar um disco de modelo de VM blindada do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2019


Assim como acontece com as VMs regulares, você pode criar um modelo de VM (por exemplo, um [modelo de VM no Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) para tornar mais fácil para os locatários e administradores implantar novas VMs na malha usando um disco de modelo. Como as VMs blindadas são ativos sensíveis à segurança, há etapas adicionais para criar um modelo de VM que dá suporte à blindagem. Este tópico aborda as etapas para criar um disco de modelo blindado e um modelo de VM no VMM.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [as etapas de configuração do provedor de serviço de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparar um VHDX do sistema operacional

Primeiro, prepare um disco do sistema operacional que será executado por meio do assistente de criação de disco de modelo blindado. Esse disco será usado como o disco do sistema operacional nas VMs de seu locatário. Você pode usar qualquer ferramenta existente para criar esse disco, como o Microsoft Desktop Image Service Manager (DISM), ou configurar manualmente uma VM com um VHDX em branco e instalar o sistema operacional nesse disco. Ao configurar o disco, ele deve aderir aos seguintes requisitos específicos para VMs de geração 2 e/ou blindadas: 

| Requisito para VHDX | Motivo |
|-----------|----|
|Deve ser um disco de tabela de partição GUID (GPT) | Necessário para máquinas virtuais de geração 2 para dar suporte a UEFI|
|O tipo de disco deve ser **básico** em oposição ao **dinâmico**. <br>Observação: isso se refere ao tipo de disco lógico, não ao recurso VHDX "de expansão dinâmica" com suporte do Hyper-V. | O BitLocker não dá suporte a discos dinâmicos.|
|O disco tem pelo menos duas partições. Uma partição deve incluir a unidade na qual o Windows está instalado. Esta é a unidade que o BitLocker irá criptografar. A outra partição é a partição ativa, que contém o carregador de inicialização e permanece descriptografada para que o computador possa ser iniciado.|Necessário para o BitLocker|
|O sistema de arquivos é NTFS | Necessário para o BitLocker|
|O sistema operacional instalado no VHDX é um dos seguintes:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necessário para dar suporte a máquinas virtuais de geração 2 e ao modelo de inicialização segura da Microsoft|
|O sistema operacional deve ser generalizado (executar Sysprep. exe) | O provisionamento de modelo envolve a especialização de VMs para a carga de trabalho de um locatário específico| 

> [!NOTE]
> Se você usar o VMM, não copie o disco de modelo na biblioteca do VMM neste estágio. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Executar Windows Update no sistema operacional do modelo

No disco de modelo, verifique se o sistema operacional tem todas as atualizações mais recentes do Windows instaladas. As atualizações lançadas recentemente melhoram a confiabilidade do processo de blindagem de ponta a ponta-um processo que pode falhar ao ser concluído se o sistema operacional do modelo não estiver atualizado.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparar e proteger o VHDX com o assistente de disco de modelo

Para usar um disco de modelo com VMs blindadas, o disco deve ser preparado e criptografado com o BitLocker usando o assistente de criação de disco de modelo blindado. Este assistente irá gerar um hash para o disco e adicioná-lo a um catálogo de assinatura de volume (VSC). O VSC é assinado usando um certificado que você especifica e é usado durante o processo de provisionamento para garantir que o disco que está sendo implantado para um locatário não tenha sido alterado ou substituído por um disco no qual o locatário não confia. Por fim, o BitLocker é instalado no sistema operacional do disco (se ainda não estiver lá) para preparar o disco para criptografia durante o provisionamento da VM.

> [!NOTE]
> O assistente de disco de modelo modificará o disco de modelo que você especificar no local. Talvez você queira fazer uma cópia do VHDX desprotegido antes de executar o assistente para fazer atualizações no disco mais tarde. Você não poderá modificar um disco que tenha sido protegido com o assistente de disco de modelo.

Execute as etapas a seguir em um computador que esteja executando o Windows Server 2016, Windows 10 (com as ferramentas de gerenciamento de servidor remoto, RSAT instalado) ou posterior (não precisa ser um host protegido ou um servidor do VMM):

1. Copie o VHDX generalizado criado em [preparar um vhdx do sistema operacional](#prepare-an-operating-system-vhdx) para o servidor, se ele ainda não estiver lá.

2. Para administrar o servidor localmente, instale o recurso **ferramentas de VM blindada** de **ferramentas de administração de servidor remoto** no servidor.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    Você também pode administrar o servidor de um computador cliente no qual você instalou o [Windows 10 ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520).

3. Obtenha ou crie um certificado para assinar o VSC para o VHDX que se tornará o disco de modelo para novas VMs blindadas. Os detalhes sobre esse certificado serão mostrados aos locatários quando criarem seus arquivos de dados de blindagem e estiverem autorizando discos em que confiam. Portanto, é importante obter esse certificado de uma autoridade de certificação mutuamente confiável por você e seus locatários. Em cenários empresariais em que você é o hoster e o locatário, você pode considerar a emissão desse certificado de sua PKI.

    Se você estiver configurando um ambiente de teste e quiser apenas usar um certificado autoassinado para preparar o disco de modelo, execute um comando semelhante ao seguinte:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Inicie o **Assistente de disco de modelo** na pasta **Ferramentas administrativas** no menu iniciar ou digitando **TemplateDiskWizard. exe** em um prompt de comando.

5. Na página **certificado** , clique em **procurar** para exibir uma lista de certificados. Selecione o certificado com o qual deseja preparar o modelo de disco. Clique em **OK** e clique em **Avançar**.

6. Na página disco virtual, clique em **procurar** para selecionar o VHDX que você preparou e clique em **Avançar**.

7. Na página catálogo de assinaturas, forneça um **nome de disco** e uma versão amigáveis **.** Esses campos estão presentes para ajudá-lo a identificar o disco quando ele tiver sido preparado.

    Por exemplo, para o **nome do disco** , você pode digitar _WS2016_ e, para a **versão**, _1.0.0.0_

8. Examine suas seleções na página Configurações de revisão do assistente. Quando você clica em **gerar**, o assistente habilitará o BitLocker no disco de modelo, computará o hash do disco e criará o catálogo de assinatura de volume, que é armazenado nos metadados VHDX.

    Aguarde até que o processo de preparação seja concluído antes de tentar montar ou mover o disco de modelo. Esse processo pode demorar um pouco para ser concluído, dependendo do tamanho do disco.

    > [!IMPORTANT]
    > Os discos de modelo só podem ser usados com o processo de provisionamento de VM blindado seguro.
    > A tentativa de inicializar uma VM normal (sem blindagem) usando um disco de modelo provavelmente resultará em um erro de parada (tela azul) e não terá suporte.

9. Na página **Resumo** , informações sobre o modelo de disco, o certificado usado para assinar o VSC e o emissor do certificado são mostrados. Clique em **Fechar** para sair do assistente.

Se você usar o VMM, siga as etapas nas seções restantes neste tópico para incorporar um disco de modelo em um modelo de VM blindada no VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copiar o disco de modelo para a biblioteca do VMM

Se você usar o VMM, depois de criar um disco de modelo, você precisará copiá-lo para um compartilhamento de biblioteca do VMM para que os hosts possam baixar e usar o disco ao provisionar novas VMs. Use o procedimento a seguir para copiar o disco de modelo na biblioteca do VMM e, em seguida, atualizar a biblioteca.

1. Copie o arquivo VHDX para a pasta de compartilhamento de biblioteca do VMM. Se você usou a configuração padrão do VMM, copie o disco do modelo para _\\<vmmserver>\MSSCVMMLibrary\VHDs_.

2. Atualize o servidor de biblioteca. Abra o espaço de trabalho **biblioteca** , expanda **servidores de biblioteca**, clique com o botão direito do mouse no servidor de biblioteca que você deseja atualizar e clique em **Atualizar**.

3. Em seguida, forneça ao VMM informações sobre o sistema operacional instalado no disco do modelo:

    a. Localize seu disco de modelo importado recentemente no servidor de biblioteca no espaço de trabalho **biblioteca** .

    b. Clique com o botão direito do mouse no disco e clique em **Propriedades**.

    c. Para **sistema operacional**, expanda a lista e selecione o sistema operacional instalado no disco. A seleção de um sistema operacional indica ao VMM que o VHDX não está em branco.

    d. Quando tiver atualizado as propriedades, clique em **OK**.

O ícone de escudo pequeno ao lado do nome do disco denota o disco como um disco de modelo preparado para VMs blindadas. Você também pode clicar com o botão direito do mouse nos cabeçalhos de coluna e alternar a coluna **blindada** para ver uma representação textual que indica se um disco destina-se a implantações de VM regulares ou blindadas.

![Disco de modelo de VM blindada](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Criar o modelo de VM blindada no VMM usando o disco de modelo preparado

Com um disco de modelo preparado em sua biblioteca do VMM, você está pronto para criar um modelo de VM para VMs blindadas. Os modelos de VM para VMs blindadas diferem ligeiramente dos modelos de VM tradicionais, pois determinadas configurações são corrigidas (VM de geração 2, UEFI e inicialização segura habilitada e assim por diante) e outras não estão disponíveis (a personalização de locatário está limitada a alguns, selecione Propriedades da VM) . Para criar o modelo de VM, execute as seguintes etapas:

1. No espaço de trabalho **biblioteca** , clique em **criar modelo de VM** na guia início na parte superior.

2. Na página **Selecionar origem**, clique em **Usar um modelo de VM existente ou um disco rígido virtual armazenado na biblioteca** e, em seguida, clique em **Procurar**.

3. Na janela que aparece, selecione um disco de modelo preparado na biblioteca do VMM. Para identificar mais facilmente quais discos estão preparados, clique com o botão direito do mouse em um cabeçalho de coluna e habilite a coluna **blindada** . Clique em **OK** e em **Avançar**.

4. Especifique um nome de modelo de VM e, opcionalmente, uma descrição e clique em **Avançar**.

5. Na página **Configurar hardware** , especifique os recursos das VMs criadas com base neste modelo. Verifique se pelo menos uma NIC está disponível e configurada no modelo de VM. A única maneira de um locatário se conectar a uma VM blindada é por meio de Conexão de Área de Trabalho Remota, Gerenciamento Remoto do Windows ou outras ferramentas de gerenciamento remoto pré-configuradas que funcionam em protocolos de rede.

    Se você optar por aproveitar pools de IP estáticos no VMM em vez de executar um servidor DHCP na rede de locatário, será necessário alertar seus locatários para essa configuração. Quando um locatário fornece o arquivo de dados de blindagem, que contém o arquivo autônomo do VMM, ele precisará fornecer valores especiais de espaço reservado para as informações de pool de IP estático. Para obter mais informações sobre espaços reservados do VMM em arquivos autônomos de locatário, consulte [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Na página **Configurar sistema operacional** , o VMM mostrará apenas algumas opções para VMs blindadas, incluindo a chave do produto, o fuso horário e o nome do computador. Algumas informações seguras, como a senha de administrador e o nome de domínio, são especificadas pelo locatário por meio de um arquivo de dados de blindagem (. Arquivo PDK). 

    > [!NOTE]
    > Se você optar por especificar uma chave do produto (Product Key) nessa página, verifique se ela é válida para o sistema operacional no disco do modelo. Se uma chave de produto incorreta for usada, a criação da VM falhará.

Depois que o modelo é criado, os locatários podem usá-lo para criar novas máquinas virtuais. Você precisará verificar se o modelo de VM é um dos recursos disponíveis para o função de usuário Administrador de Locatários (no VMM, as funções de usuário estão no espaço de trabalho **configurações** ).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparar e proteger o VHDX usando o PowerShell

Como alternativa para executar o assistente de disco de modelo, você pode copiar o disco de modelo e o certificado para um computador que esteja executando o RSAT e executar [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) para iniciar o processo de assinatura.
O exemplo a seguir usa as informações de nome e versão especificadas pelos parâmetros _TemplateName_ e _version_ .
O VHDX que você fornecer ao parâmetro `-Path` será substituído pelo disco de modelo atualizado, portanto, certifique-se de fazer uma cópia antes de executar o comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

O disco de modelo agora está pronto para ser usado para provisionar VMs blindadas.
Se você estiver usando System Center Virtual Machine Manager para implantar sua VM, agora você pode copiar o VHDX para a biblioteca do VMM.

Talvez você também queira extrair o catálogo de assinatura de volume do VHDX.
Esse arquivo é usado para fornecer informações sobre o certificado de autenticação, o nome do disco e a versão aos proprietários da VM que desejam usar seu modelo.
Eles precisam importar esse arquivo para o assistente de arquivo de dados de blindagem para autorizar você, o autor do modelo em posse do certificado de autenticação, para criar esse e futuros discos de modelo para eles.

Para extrair o catálogo de assinatura de volume, execute o seguinte comando no PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um arquivo de dados de blindagem](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Veja também

- [Etapas de configuração do provedor de serviços de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
