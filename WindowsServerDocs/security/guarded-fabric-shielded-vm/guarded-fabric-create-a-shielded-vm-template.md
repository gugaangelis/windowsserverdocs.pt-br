---
title: Criar um disco de modelo VM blindado do Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: e00322186ea34784048366bf17881af742cb4444
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853687"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Criar um disco de modelo VM blindado do Windows

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Como com VMs regulares, você pode criar um modelo de VM (por exemplo, uma [modelo de VM no Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) para tornar mais fácil para locatários e administradores implantar novas VMs na malha do usando um disco de modelo. Como as VMs blindadas são ativos confidenciais de segurança, há etapas adicionais para criar um modelo VM que dá suporte para blindagem. Este tópico aborda as etapas para criar um disco de modelo blindado e um modelo de VM no VMM.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [hospedar as etapas de configuração de provedor de serviço para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparar um VHDX do sistema operacional

Primeiro, prepare um disco do sistema operacional que você executará, em seguida, por meio do Assistente de criação de disco modelo blindado. Esse disco será usado como o disco do sistema operacional em VMs do locatário. Você pode usar as ferramentas existentes para criar esse disco, como o Microsoft Desktop imagem Service Manager (DISM), ou configurar uma VM com um VHDX em branco e instalar manualmente o sistema operacional nesse disco. Ao configurar o disco, ele deve cumprir os seguintes requisitos que são específicos para a geração 2 e/ou VMs blindadas: 

| Requisito para VHDX | Motivo |
|-----------|----|
|Deve ser um disco de tabela de partição GUID (GPT) | Necessário para máquinas virtuais de geração 2 deem suporte à UEFI|
|Tipo de disco deve ser **básicas** em vez de **dinâmico**. <br>Observação: Isso se refere ao tipo de disco lógico, não o "expansão dinâmica" recurso VHDX suportado pelo Hyper-V. | O BitLocker não oferece suporte a discos dinâmicos.|
|O disco tem pelo menos duas partições. Uma partição deve incluir a unidade em que o Windows está instalado. Esta é a unidade que o BitLocker criptografará. A outra partição é a partição ativa, que contém o carregador de inicialização e permanecerá não criptografada para que o computador pode ser iniciado.|Necessário para o BitLocker|
|É o sistema de arquivos NTFS | Necessário para o BitLocker|
|O sistema operacional instalado no VHDX é um dos seguintes:<br>-Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 <br>- Windows 10, Windows 8.1, Windows 8| Necessário para dar suporte a máquinas virtuais de geração 2 e o modelo de inicialização segura do Microsoft|
|Sistema operacional deve ser generalizado (execução sysprep.exe) | Modelo de provisionamento envolve especializado VMs para carga de trabalho de um locatário específico| 

> [!NOTE]
> Se você usar o VMM, não copie o disco de modelo para a biblioteca do VMM neste estágio. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Execute o Windows Update no sistema operacional do modelo

No disco do modelo, verifique se o sistema operacional tem todas as atualizações mais recentes do Windows instaladas. Recentemente, atualizações lançadas melhoram a confiabilidade do processo de blindagem de ponta a ponta - um processo que pode falhar ao se completa, o sistema operacional do modelo não está atualizado.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparar e proteger o VHDX com o Assistente de disco de modelo

Para usar um disco de modelo com VMs blindadas, o disco deve ser preparado e criptografado com BitLocker usando o Assistente de criação de disco modelo blindado. Este assistente irá gerar um hash para o disco e adicioná-lo a um catálogo de assinatura de volume (VSC). O VSC é assinado usando um certificado que você especificar e é usado durante o processo de provisionamento para garantir que o disco que está sendo implantado para um locatário não foi alterado ou substituído por um disco que o locatário não confia. Por fim, o BitLocker é instalado no sistema de operacional do disco (se ele não ainda estiver lá) para preparar o disco para a criptografia durante o provisionamento da VM.

> [!NOTE]
> O Assistente de disco de modelo irá modificar o disco de modelo que você especificar no local. Você talvez queira fazer uma cópia de VHDX desprotegido antes de executar o Assistente para fazer atualizações para o disco em um momento posterior. Você não poderá modificar um disco que foi protegido com o Assistente de disco de modelo.

Execute as seguintes etapas em um computador executando o Windows Server 2016 (não precisa ser um host protegido ou um servidor do VMM):

1. Copie o VHDX generalizado criado na [preparar um sistema operacional VHDX](#prepare-an-operating-system-vhdx) para o servidor, se ele não ainda estiver lá.

2. Para administrar o servidor localmente, instale o **ferramentas de VM Blindada** recurso de **ferramentas de administração de servidor remoto** no servidor.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    Você também pode administrar o servidor de um computador cliente no qual você instalou o [ferramentas de administração de servidor remoto do Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

3. Obter ou criar um certificado para assinar o VSC para o VHDX que se tornará o disco de modelo para novas VMs blindadas. Detalhes sobre o certificado serão exibidos para locatários quando criarem seus arquivos de dados de blindagem e autorizar o discos que confiam. Portanto, é importante obter este certificado de autoridade de certificação mutuamente confiável para você e seus locatários. Cenários de negócios em que você está o hoster e o locatário, você pode considerar esse certificado de emissão de sua PKI.

    Se você estiver configurando um ambiente de teste e deseja apenas usar um certificado autoassinado para preparar seu disco de modelo, execute um comando semelhante ao seguinte:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Iniciar o **Assistente de modelo de disco** da **ferramentas administrativas** pasta no menu Iniciar ou digitando **TemplateDiskWizard.exe** em um prompt de comando.

5. Sobre o **certificado** , clique em **procurar** para exibir uma lista de certificados. Selecione o certificado com a qual você pode preparar o modelo de disco. Clique em **OK** e clique em **Avançar**.

6. Na página de disco Virtual, clique em **navegue** para selecionar o VHDX que você preparou, em seguida, clique em **próxima**.

7. Na página Catálogo de assinatura, forneça um amigável **nome do disco** e **versão.** Esses campos estão presentes para ajudá-lo a identificar o disco depois que ele foi preparado.

    Por exemplo, para **nome do disco** você pode digitar _WS2016_ e para **versão**, _1.0.0.0_

8. Examine suas seleções na página de configurações de análise do assistente. Quando você clica em **gerar**, o Assistente para habilitar o BitLocker no disco do modelo, computar o hash do disco e criar o catálogo de assinatura de Volume, que é armazenado nos metadados do VHDX.

    Aguarde até que o processo de preparação foi concluído antes de tentar montar ou mover o disco de modelo. Esse processo pode demorar um pouco para ser concluído, dependendo do tamanho do disco.

    > [!IMPORTANT]
    > Discos de modelo só podem ser usados com a processo de provisionamento da VM blindada segura.
    > A tentativa de inicializar uma VM (não blindada) comum usando um disco de modelo será provavelmente resultará em um erro de parada (tela azul) e não tem suporte.

9. Sobre o **resumo** página informações sobre o modelo de disco, o certificado usado para assinar o VSC, e o emissor do certificado é mostrado. Clique em **Fechar** para sair do assistente.

Se você usar o VMM, siga as etapas nas seções restantes deste tópico para incorporar um disco de modelo em um modelo de VM blindada no VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copie o disco de modelo à biblioteca do VMM

Se você usar o VMM, depois de criar um disco de modelo, você precisará copiá-lo a um compartilhamento de biblioteca do VMM para que hosts podem baixar e usar o disco durante o provisionamento de novas VMs. Use o procedimento a seguir para copiar o disco de modelo na biblioteca do VMM e, em seguida, atualizar a biblioteca.

1. Copie o arquivo VHDX para a pasta de compartilhamento de biblioteca do VMM. Se você usou a configuração padrão do VMM, copie o disco de modelo para  _\\ <vmmserver>\MSSCVMMLibrary\VHDs_.

2. Atualize o servidor de biblioteca. Abra o **biblioteca** espaço de trabalho, expanda **servidores de biblioteca**, clique com botão direito no servidor de biblioteca que você deseja atualizar e clique em **atualizar**.

3. Em seguida, fornece VMM com informações sobre o sistema operacional instalado no disco do modelo:

    a. Localizar o disco de modelo importado recentemente no servidor de biblioteca na **biblioteca** espaço de trabalho.

    b. Clique com botão direito no disco e, em seguida, clique em **propriedades**.

    c. Para **sistema operacional**, expanda a lista e selecione o sistema operacional instalado no disco. A seleção de um sistema operacional indica ao VMM que o VHDX não está em branco.

    d. Quando tiver atualizado as propriedades, clique em **OK**.

O ícone de escudo pequeno ao lado do nome do disco indica o disco como um disco de modelo preparado para VMs blindadas. Você pode também clique com botão direito cabeçalhos de coluna e ativar/desativar a **Blindadas** coluna para ver uma representação textual que indica se um disco destina-se para implantações de VM blindadas ou regulares.

![Disco de modelo de vm blindada](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Criar o modelo de VM blindada no VMM usando o disco de modelo preparado

Com um disco de modelo preparado na biblioteca do VMM, você está pronto para criar um modelo de VM para VMs blindadas. Modelos de VM para VMs blindadas diferem ligeiramente de modelos de VM tradicionais em que algumas configurações são fixas (geração 2 VMs, UEFI e inicialização segura habilitada e assim por diante) e outras pessoas não estão disponíveis (personalização de locatário é limitada a alguns, selecione Propriedades da VM) . Para criar o modelo de VM, execute as seguintes etapas:

1. No **biblioteca** espaço de trabalho, clique em **criar modelo de VM** na guia Início na parte superior.

2. Na página **Selecionar origem**, clique em **Usar um modelo de VM existente ou um disco rígido virtual armazenado na biblioteca** e, em seguida, clique em **Procurar**.

3. Na janela que aparece, selecione um disco de modelo preparado da biblioteca do VMM. Para identificar mais facilmente quais discos estão preparados, um cabeçalho de coluna com o botão direito e habilitar o **Blindadas** coluna. Clique em **Okey** , em seguida, **próxima**.

4. Especifique um nome de modelo VM e, opcionalmente, uma descrição e, em seguida, clique em **próxima**.

5. Sobre o **configurar Hardware** , especifique os recursos de VMs criadas com base neste modelo. Certifique-se de que pelo menos uma NIC está disponível e configurado no modelo de VM. A única maneira de um locatário para se conectar a uma máquina virtual blindada é por meio de Conexão de área de trabalho remota, gerenciamento remoto do Windows ou outras ferramentas de gerenciamento remoto pré-configurado que funcionam por meio de protocolos de rede.

    Se você optar por aproveitar os pools IP estáticos no VMM em vez de executar um servidor DHCP na rede de locatário, você precisará para seus locatários para essa configuração de alerta. Quando um locatário fornece seu arquivo de dados de blindagem, que contém o arquivo autônomo para o VMM, precisará fornecer valores de espaço reservado especial para as informações de pool IP estático. Para obter mais informações sobre espaços reservados VMM nos arquivos autônomos de locatário, consulte [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. Sobre o **configurar o sistema operacional** página, o VMM mostrará apenas algumas opções para as VMs blindadas, incluindo a chave do produto, o fuso horário e o nome do computador. Algumas informações seguras, como o nome de domínio e senha de administrador, são especificadas pelo locatário por meio de um arquivo de dados de blindagem (. Arquivo PDK). 

    > [!NOTE]
    > Se você optar por especificar uma chave do produto nesta página, verifique se que ele é válido para o sistema operacional no disco do modelo. Se uma chave do produto incorreto for usada, a criação da VM falhará.

Depois que o modelo é criado, locatários podem usá-lo para criar novas máquinas virtuais. Você precisará verificar se o modelo de VM é um dos recursos disponíveis para a função de usuário administrador de locatários (no VMM, as funções de usuário estão na **configurações** espaço de trabalho).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparar e proteger o VHDX usando o PowerShell

Como uma alternativa para executar o Assistente de disco de modelo, você pode copiar o disco de modelo e o certificado para um computador que executa o RSAT e executar [TemplateDisk proteger](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) para iniciar o processo de assinatura.
O exemplo a seguir usa as informações de versão e o nome especificadas pelo _TemplateName_ e _versão_ parâmetros.
O VHDX que você fornecer para o `-Path` parâmetro será será substituída com o disco de modelo atualizado, portanto, certifique-se de fazer uma cópia antes de executar o comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

O disco de modelo agora está pronto para ser usado para provisionar VMs blindadas.
Se você estiver usando o System Center Virtual Machine Manager para implantar a VM, agora você pode copiar o VHDX para a biblioteca do VMM.

Talvez você queira extrair o catálogo de assinatura de volume de VHDX.
Esse arquivo é usado para fornecer informações sobre o certificado de autenticação, o nome do disco e a versão para os proprietários da VM que desejam usar seu modelo.
Eles precisam importar esse arquivo para o Assistente de arquivo de dados de blindagem para autorizá-lo, o autor do modelo em posse do certificado de autenticação, para criar este e discos de modelo futuras para eles.

Para extrair o catálogo de assinatura de volume, execute o seguinte comando no PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Criar um arquivo de dados de blindagem](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Consulte também

- [Etapas de configuração de provedor de serviço de hospedagem de hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
