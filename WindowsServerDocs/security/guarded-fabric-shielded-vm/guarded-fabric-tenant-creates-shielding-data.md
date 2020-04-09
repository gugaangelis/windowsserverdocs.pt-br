---
title: VMs blindadas para locatários – criando dados de blindagem para definir uma VM blindada
ms.prod: windows-server
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: 6ff502e7246c899a7b4f29125266bf05d07e40ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856449"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>VMs blindadas para locatários – criando dados de blindagem para definir uma VM blindada

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Um arquivo de dados de blindagem (também chamado de um arquivo de dados de provisionamento ou arquivo PDK) é um arquivo criptografado que um locatário ou proprietário de VM cria para proteger as informações de configuração de VM importantes, como a senha do administrador, RDP e outros certificados relacionados à identidade, credenciais de associação de domínio e assim por diante. Este tópico fornece informações sobre como criar um arquivo de dados de blindagem. Antes de criar o arquivo, você deve obter um disco de modelo do seu provedor de serviços de hospedagem ou criar um disco de modelo conforme descrito em [VMs blindadas para locatários-criando um disco de modelo (opcional)](guarded-fabric-tenant-creates-template-disk.md).

Para obter uma lista e um diagrama do conteúdo de um arquivo de dados de blindagem, confira [o que são os dados de blindagem e por que ele é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> As etapas nesta seção devem ser concluídas em um computador confiável e separado fora da malha protegida. Normalmente, o proprietário da VM (locatário) criaria os dados de blindagem para suas VMs, não para os administradores de malha.

Para se preparar para criar um arquivo de dados de blindagem, execute as seguintes etapas:

- [Obter um certificado para Conexão de Área de Trabalho Remota](#optional-obtain-a-certificate-for-remote-desktop-connection)
- [Criar um arquivo de resposta](#create-an-answer-file)
- [Obter o arquivo de catálogo de assinatura de volume](#get-the-volume-signature-catalog-file)
- [Selecionar malhas confiáveis](#select-trusted-fabrics)

Em seguida, você pode criar o arquivo de dados de blindagem:

- [Criar um arquivo de dados de blindagem e adicionar guardiões](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)

## <a name="optional-obtain-a-certificate-for-remote-desktop-connection"></a>Adicional Obter um certificado para Conexão de Área de Trabalho Remota

Como os locatários só podem se conectar às suas VMs blindadas usando Conexão de Área de Trabalho Remota ou outras ferramentas de gerenciamento remoto, é importante garantir que os locatários possam verificar se estão se conectando ao ponto de extremidade correto (ou seja, não há um "homem no meio" interceptando a conexão).

Uma maneira de verificar se você está se conectando ao servidor pretendido é instalar e configurar um certificado para Serviços de Área de Trabalho Remota apresentar quando você inicia uma conexão. O computador cliente que está se conectando ao servidor verificará se ele confia no certificado e mostrará um aviso caso contrário. Em geral, para garantir que o cliente de conexão confie no certificado, os certificados RDP são emitidos por meio da PKI do locatário. Mais informações sobre como [usar certificados no serviços de área de trabalho remota](https://technet.microsoft.com/library/dn781533.aspx) podem ser encontradas no TechNet.

 Para ajudá-lo a decidir se você precisa obter um certificado RDP personalizado, considere o seguinte:

- Se você estiver apenas testando VMs blindadas em um ambiente de laboratório, **não** precisará de um certificado RDP personalizado.
- Se sua VM estiver configurada para ingressar em um domínio de Active Directory, um certificado de computador normalmente será emitido pela autoridade de certificação da sua organização automaticamente e usado para identificar o computador durante as conexões RDP. Você **não** precisa de um certificado RDP personalizado.
- Se sua VM não estiver ingressada no domínio, mas você quiser uma maneira de verificar se está se conectando ao computador correto ao usar o Área de Trabalho Remota, **considere** o uso de certificados RDP personalizados.

> [!TIP]
> Ao selecionar um certificado RDP para incluir em seu arquivo de dados de blindagem, certifique-se de usar um certificado curinga. Um arquivo de dados de blindagem pode ser usado para criar um número ilimitado de VMs. Como cada VM compartilhará o mesmo certificado, um certificado curinga garante que o certificado será válido, independentemente do nome de host da VM.

## <a name="create-an-answer-file"></a>Crie um arquivo de resposta

Como o disco de modelo assinado no VMM é generalizado, os locatários são necessários para fornecer um arquivo de resposta para especializar suas VMs blindadas durante o processo de provisionamento. O arquivo de resposta (geralmente chamado de arquivo autônomo) pode configurar a VM para sua função pretendida – ou seja, ele pode instalar recursos do Windows, registrar o certificado RDP criado na etapa anterior e executar outras ações personalizadas. Ele também fornecerá as informações necessárias para a instalação do Windows, incluindo a senha do administrador padrão e a chave do produto (Product Key).

Para obter informações sobre como obter e usar a função **New-ShieldingDataAnswerFile** para gerar um arquivo de resposta (arquivo Unattend. xml) para a criação de VMs blindadas, consulte [gerar um arquivo de resposta usando a função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Usando a função, você pode gerar mais facilmente um arquivo de resposta que reflita opções como as seguintes:

- A VM deve estar ingressada no domínio no final do processo de inicialização?
- Você usará uma licença por volume ou uma chave de produto específica por VM?
- Você está usando o DHCP ou o IP estático?
- Você usará um certificado de protocolo RDP personalizado (RDP) que será usado para provar que a VM pertence à sua organização?
- Deseja executar um script no final da inicialização?

Os arquivos de resposta usados em arquivos de dados de blindagem serão usados em todas as VMs criadas usando esse arquivo de dados de blindagem. Portanto, você deve certificar-se de não embutir nenhuma informação específica da VM no arquivo de resposta. O VMM dá suporte a algumas cadeias de caracteres de substituição (consulte a tabela abaixo) no arquivo Unattend para lidar com valores de especialização que podem mudar de VM para VM. Não é necessário usá-los; no entanto, se eles estiverem presentes, o VMM aproveitará eles.

Ao criar um arquivo Unattend. xml para VMs blindadas, tenha em mente as seguintes restrições:

- Se você estiver usando o VMM para gerenciar seu datacenter, o arquivo autônomo deverá fazer com que a VM seja desligada após ter sido configurada. Isso é para permitir que o VMM saiba quando deve relatar ao locatário que a VM concluiu o provisionamento e está pronta para uso. O VMM ligará automaticamente a VM quando detectar que ela foi desativada durante o provisionamento.

- Certifique-se de habilitar o RDP e a regra de firewall correspondente para que você possa acessar a VM depois que ela tiver sido configurada. Você não pode usar o console do VMM para acessar VMs blindadas, portanto, você precisará do RDP para se conectar à sua VM. Se você preferir gerenciar seus sistemas com a comunicação remota do Windows PowerShell, verifique se o WinRM está habilitado também.

- As únicas cadeias de caracteres de substituição com suporte em arquivos autônomos da VM blindada são as seguintes:

    | Elemento substituível | Cadeia de caracteres de substituição |
    |-----------|-----------|
    | ComputerName        | @ComputerName@      |
    | Fuso horário            | @TimeZone@          |
    | ProductKey          | @ProductKey@        |
    | IPAddr4-1           | @IP4Addr-1@         |
    | IPAddr6-1           | @IP6Addr-1@         |
    | MACAddr-1           | @MACAddr-1@         |
    | Prefixo-1-1          | @Prefix-1-1@        |
    | NextHop-1-1         | @NextHop-1-1@       |
    | Prefixo-1-2          | @Prefix-1-2@        |
    | NextHop-1-2         | @NextHop-1-2@       |

    Se você tiver mais de uma NIC, poderá adicionar várias cadeias de caracteres de substituição para a configuração de IP incrementando o primeiro dígito. Por exemplo, para definir o endereço IPv4, a sub-rede e o gateway para 2 NICs, você usaria as seguintes cadeias de caracteres de substituição:

    | Cadeia de caracteres de substituição | Substituição de exemplo |
    |---------------------|----------------------|
    | @IP4Addr-1@         | 192.168.1.10/24      |
    | @MACAddr-1@         | Ethernet             |
    | @Prefix-1-1@        | 24                   |
    | @NextHop-1-1@       | 192.168.1.254        |
    | @IP4Addr-2@         | 10.0.20.30/24        |
    | @MACAddr-2@         | Ethernet 2           |
    | @Prefix-2-1@        | 24                   |
    | @NextHop-2-1@       | 10.0.20.1            |

Ao usar cadeias de caracteres de substituição, é importante garantir que as cadeias de caracteres serão preenchidas durante o processo de provisionamento da VM. Se uma cadeia de caracteres como @ProductKey@ não for fornecida no momento da implantação, deixando o nó &lt;ProductKey&gt; no arquivo autônomo em branco, o processo de especialização falhará e você não poderá se conectar à sua VM.

Além disso, observe que as cadeias de caracteres de substituição relacionadas à rede em direção ao final da tabela só serão usadas se você estiver aproveitando os pools de endereços IP estáticos do VMM. Seu provedor de serviços de hospedagem deve ser capaz de informá-lo se essas cadeias de substituição forem necessárias. Para obter mais informações sobre endereços IP estáticos em modelos do VMM, consulte o seguinte na documentação do VMM:

- [Diretrizes para pools de endereços IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools)
- [Configurar pools de endereços IP estáticos na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Por fim, é importante observar que o processo de implantação de VM blindada criptografará apenas a unidade do sistema operacional. Se você implantar uma VM blindada com uma ou mais unidades de dados, é altamente recomendável que você adicione um comando autônomo ou Política de Grupo configuração no domínio do locatário para criptografar automaticamente as unidades de dados.

## <a name="get-the-volume-signature-catalog-file"></a>Obter o arquivo de catálogo de assinatura de volume

Os arquivos de dados de blindagem também contêm informações sobre os discos de modelo que um locatário confia. Os locatários adquirem as assinaturas de disco de discos de modelo confiáveis na forma de um arquivo de catálogo de assinatura de volume (VSC). Essas assinaturas são validadas quando uma nova VM é implantada. Se nenhuma das assinaturas no arquivo de dados de blindagem coincidir com o disco de modelo que está tentando ser implantado com a VM (ou seja, ela foi modificada ou trocada por um disco diferente, potencialmente mal-intencionado), o processo de provisionamento falhará.

> [!IMPORTANT]
> Embora o VSC garanta que um disco não foi violado, ainda é importante para o locatário confiar no disco em primeiro lugar. Se você for o locatário e o disco de modelo for fornecido pelo hoster, implante uma VM de teste usando esse disco de modelo e execute suas próprias ferramentas (antivírus, verificadores de vulnerabilidade e assim por diante) para validar se o disco é, na verdade, em um estado em que você confia.

Há duas maneiras de adquirir o VSC de um disco de modelo:

1. O hoster (ou locatário, se o locatário tiver acesso ao VMM) usa os cmdlets do PowerShell do VMM para salvar o VSC e o fornece ao locatário. Isso pode ser realizado em qualquer computador com o console do VMM instalado e configurado para gerenciar o ambiente do VMM da malha de hospedagem. Os cmdlets do PowerShell para salvar o VSC são:

    ```powershell
    $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"

    $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk

    $vsc.WriteToFile(".\templateDisk.vsc")
    ```

2. O locatário tem acesso ao arquivo de disco do modelo. Esse pode ser o caso se o locatário criar um disco de modelo para ser carregado em um provedor de serviços de hospedagem ou se o locatário puder baixar o disco de modelo do hoster. Nesse caso, sem o VMM na imagem, o locatário executaria o seguinte cmdlet (instalado com o recurso de ferramentas de VM blindada, parte do Ferramentas de Administração de Servidor Remoto):

    ```powershell
    Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc
    ```

## <a name="select-trusted-fabrics"></a>Selecionar malhas confiáveis

O último componente do arquivo de dados de blindagem está relacionado ao proprietário e aos Guardiões de uma VM. Os guardiões são usados para designar o proprietário de uma VM blindada e as malhas protegidas nas quais ele está autorizado a ser executado.

Para autorizar uma malha de hospedagem a executar uma VM blindada, você deve obter os metadados do guardião do serviço guardião de host do provedor de serviço de hospedagem. Geralmente, o provedor de serviços de hospedagem fornecerá esses metadados por meio de suas ferramentas de gerenciamento. Em um cenário empresarial, você pode ter acesso direto para obter os metadados por conta própria.

Você ou seu provedor de serviços de hospedagem podem obter os metadados do guardião do HGS executando uma das seguintes ações:

- Obtenha os metadados do guardião diretamente do HGS executando o seguinte comando do Windows PowerShell ou navegando até o site e salvando o arquivo XML que é exibido:

    ```powershell
    Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml
    ```

- Obtenha os metadados do guardião do VMM usando os cmdlets do PowerShell do VMM:

    ```powershell
    $relecloudmetadata = Get-SCGuardianConfiguration
    $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8
    ```

Obtenha os arquivos de metadados do guardião para cada malha protegida na qual você deseja autorizar a execução de suas VMs blindadas antes de continuar.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Criar um arquivo de dados de blindagem e adicionar guardiões usando o assistente de arquivo de dados de blindagem

Execute o assistente de arquivo de dados de blindagem para criar um arquivo de dados de blindagem (PDK). Aqui, você adicionará o certificado RDP, o arquivo autônomo, os catálogos de assinatura de volume, o guardião de proprietário e os metadados do guardião baixados obtidos na etapa anterior.

1. Instale **Ferramentas de Administração de Servidor Remoto &gt; ferramentas de administração de recursos &gt; ferramentas de VM blindadas** em seu computador usando Gerenciador do servidor ou o seguinte comando do Windows PowerShell:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

2. Abra o assistente de arquivo de dados de blindagem na seção ferramentas do administrador no menu iniciar ou executando o seguinte executável **C:\\Windows\\System32\\ShieldingDataFileWizard. exe**.

3. Na primeira página, use a caixa de seleção de segundo arquivo para escolher um local e nome de arquivo para o arquivo de dados de blindagem. Normalmente, você deve nomear um arquivo de dados de blindagem após a entidade que possui qualquer VM criada com esses dados de blindagem (por exemplo, RH, ti, Finanças) e a função de carga de trabalho que está executando (por exemplo, servidor de arquivos, servidor Web ou qualquer outra pessoa configurada pelo arquivo autônomo). Deixe o botão de opção definido como **blindar dados para modelos blindados**.

    > [!NOTE]
    > No assistente de arquivo de dados de blindagem, você observará as duas opções abaixo:
    >- **Dados de blindagem para modelos blindados**
    >- **Blindagem de dados para VMs existentes e modelos não blindados**<br>
    > A primeira opção é usada ao criar novas VMs blindadas de modelos blindados. A segunda opção permite que você crie dados de blindagem que só podem ser usados durante a conversão de VMs existentes ou a criação de VMs blindadas de modelos não blindados.

    ![Assistente de arquivo de dados de blindagem, seleção de arquivo](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

    Além disso, você deve escolher se as VMs criadas usando esse arquivo de dados de blindagem serão verdadeiramente blindadas ou configuradas no modo "criptografia com suporte". Para obter mais informações sobre essas duas opções, consulte [quais são os tipos de máquinas virtuais que uma malha protegida pode executar?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Preste muita atenção à próxima etapa, pois ela define o proprietário de suas VMs blindadas e em quais malhas suas VMs blindadas serão autorizadas a serem executadas.<br>A posse do **guardião proprietário** é necessária para alterar posteriormente uma VM blindada existente de **blindado** para **criptografia com suporte** ou vice-versa.

4. Sua meta nesta etapa é de duas dobras:

    - Criar ou selecionar um guardião proprietário que representa você como o proprietário da VM

    - Importe o Guardião que você baixou do serviço guardião de host do provedor de hospedagem (ou seu próprio) na etapa anterior

    Para designar um guardião de proprietário existente, selecione o guardião apropriado no menu suspenso. Somente os guardiões instalados em seu computador local com as chaves privadas intactas aparecerão nessa lista. Você também pode criar seu próprio guardião de proprietário selecionando **gerenciar guardiões locais** no canto inferior direito e clicando em **criar** e concluindo o assistente.

    Em seguida, importamos os metadados do guardião baixados antes novamente usando a página **proprietário e guardiões** . Selecione **gerenciar guardiões locais** no canto inferior direito. Use o recurso de **importação** para importar o arquivo de metadados do guardião. Clique em **OK** depois de importar ou adicionar todos os guardiões necessários. Como prática recomendada, nomeie os Guardiões após o provedor de serviços de hospedagem ou Datacenter corporativo que eles representam. Por fim, selecione todos os guardiões que representam os data centers nos quais sua VM blindada está autorizada para execução. Você não precisa selecionar o guardião proprietário novamente. Clique em **Avançar** depois de concluir.

    ![Assistente de arquivo de dados de blindagem, proprietário e guardiões](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5. Na página qualificadores de ID de volume, clique em **Adicionar** para autorizar um disco de modelo assinado em seu arquivo de dados de blindagem. Quando você seleciona um VSC na caixa de diálogo, ele mostra informações sobre o nome do disco, a versão e o certificado que foi usado para conectá-lo. Repita esse processo para cada disco de modelo que você deseja autorizar.

6. Na página **valores de especialização** , clique em **procurar** para selecionar o arquivo Unattend. XML que será usado para especializar suas VMs.

    Use o botão **Adicionar** na parte inferior para adicionar arquivos adicionais ao PDK que são necessários durante o processo de especialização. Por exemplo, se o arquivo autônomo estiver instalando um certificado RDP na VM (conforme descrito em [gerar um arquivo de resposta usando a função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), você deverá adicionar o arquivo PFX do certificado RDP e o script RDPCertificateConfig. ps1 aqui. Observe que todos os arquivos que você especificar aqui serão automaticamente copiados para C:\\\\ Temp na VM que é criada. O arquivo autônomo deve esperar que os arquivos estejam nessa pasta ao fazer referência a eles por caminho.

7. Examine suas seleções na próxima página e clique em **gerar**.

8. Feche o assistente após a conclusão.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Criar um arquivo de dados de blindagem e adicionar guardiões usando o PowerShell

Como alternativa ao assistente de arquivo de dados de blindagem, você pode executar [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) para criar um arquivo de dados de blindagem.

Todos os arquivos de dados de blindagem precisam ser configurados com o proprietário correto e os certificados do guardião para autorizar que suas VMs blindadas sejam executadas em uma malha protegida.
Você pode verificar se você tem qualquer guardião instalado localmente executando [Get-HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Os guardiões de proprietário têm chaves privadas, enquanto os guardiões do seu datacenter normalmente não o fazem.

Se você precisar criar um guardião proprietário, execute o seguinte comando:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Esse comando cria um par de certificados de autenticação e criptografia no repositório de certificados do computador local na pasta "certificados locais da VM blindada".
Você precisará dos certificados de proprietário e suas chaves privadas correspondentes para desproteger uma máquina virtual, portanto, verifique se esses certificados são submetidos a backup e protegidos contra roubo.
Um invasor com acesso aos certificados do proprietário pode usá-los para iniciar sua máquina virtual blindada ou alterar sua configuração de segurança.

Se você precisar importar informações do guardião de uma malha protegida em que você deseja executar sua máquina virtual (seu datacenter primário, data centers de backup, etc.), execute o comando a seguir para cada [arquivo de metadados recuperado de suas malhas protegidas](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Se você usou certificados autoassinados ou se os certificados registrados com o HGS expirarem, talvez seja necessário usar os sinalizadores `-AllowUntrustedRoot` e/ou `-AllowExpired` com o comando Import-HgsGuardian para ignorar as verificações de segurança.

Você também precisará [obter um catálogo de assinatura de volume](#get-the-volume-signature-catalog-file) para cada disco de modelo que deseja usar com esse arquivo de dados de blindagem e um arquivo de resposta de dados de [blindagem](#create-an-answer-file) para permitir que o sistema operacional conclua automaticamente suas tarefas de especialização.
Por fim, decida se você deseja que sua VM seja totalmente blindada ou apenas vTPM.
Use `-Policy Shielded` para uma VM totalmente blindada ou `-Policy EncryptionSupported` para uma VM habilitada para vTPM que permite conexões básicas do console e do PowerShell Direct.

Quando tudo estiver pronto, execute o seguinte comando para criar o arquivo de dados de blindagem:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

> [!TIP]
> Se você estiver usando um certificado RDP personalizado, chaves SSH ou outros arquivos que precisam ser incluídos com o arquivo de dados de blindagem, use o parâmetro `-OtherFile` para incluí-los. Você pode fornecer uma lista separada por vírgulas de caminhos de arquivo, como `-OtherFile "C:\source\myRDPCert.pfx", "C:\source\RDPCertificateConfig.ps1"`

No comando acima, o guardião chamado "proprietário" (obtido de Get-HgsGuardian) poderá alterar a configuração de segurança da VM no futuro, enquanto "o datacenter do leste dos EUA" pode executar a VM, mas não alterar suas configurações.
Se você tiver mais de um guardião, separe os nomes dos Guardiões com vírgulas, como `'EAST-US Datacenter', 'EMEA Datacenter'`.
O qualificador de ID de volume especifica se você confia apenas na versão exata (Equals) do disco de modelo ou nas versões futuras (GreaterThanOrEquals) também.
O nome do disco e o certificado de autenticação devem corresponder exatamente à comparação de versão a ser considerada no momento da implantação.
Você pode confiar em mais de um disco de modelo fornecendo uma lista separada por vírgulas de qualificadores de ID de volume para o parâmetro `-VolumeIDQualifier`.
Por fim, se você tiver outros arquivos que precisam acompanhar o arquivo de resposta com a VM, use o parâmetro `-OtherFile` e forneça uma lista separada por vírgulas de caminhos de arquivo.

Consulte a documentação do cmdlet para [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) e [New-VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) para saber mais sobre maneiras adicionais de configurar o arquivo de dados de blindagem.

## <a name="see-also"></a>Consulte também

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
