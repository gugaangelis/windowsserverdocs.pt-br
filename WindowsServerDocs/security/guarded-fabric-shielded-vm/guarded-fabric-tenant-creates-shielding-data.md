---
title: VMs blindadas para locatários – criando dados de blindagem para definir uma VM blindada
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 49f4e84d-c1f7-45e5-9143-e7ebbb2ef052
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 3c36eff8aabd1fa1c6456dce1d08ebe504102e8c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284161"
---
# <a name="shielded-vms-for-tenants---creating-shielding-data-to-define-a-shielded-vm"></a>VMs blindadas para locatários – criando dados de blindagem para definir uma VM blindada

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Um arquivo de dados de blindagem (também chamado de um arquivo de dados de provisionamento ou arquivo PDK) é um arquivo criptografado que um locatário ou proprietário de VM cria para proteger as informações de configuração de VM importantes, como a senha do administrador, RDP e outros certificados relacionados à identidade, credenciais de associação de domínio e assim por diante. Este tópico fornece informações sobre como criar um arquivo de dados de blindagem. Antes de criar o arquivo, você deve obter um disco de modelo de provedor de serviços de hospedagem ou crie um disco de modelo, conforme descrito em [VMs Blindadas para locatários – criar um disco de modelo (opcional)](guarded-fabric-tenant-creates-template-disk.md).

Para obter uma lista e um diagrama do conteúdo de um arquivo de dados de blindagem, consulte [o que é de dados de blindagem e por que é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

> [!IMPORTANT]
> As etapas nesta seção devem ser concluídas em um computador do locatário executando o Windows Server 2016. Esse computador não deve fazer parte de uma malha protegida (ou seja, não deve ser configurado para usar um cluster HGS).

Para se preparar para criar um arquivo de dados de blindagem, execute as seguintes etapas:

- [Obter um certificado para a Conexão de área de trabalho remota](#obtain-a-certificate-for-remote-desktop-connection)
- [Criar um arquivo de resposta](#create-an-answer-file)
- [Obter o arquivo de catálogo de assinatura de volume](#get-the-volume-signature-catalog-file)
- [Selecione as malhas confiáveis](#select-trusted-fabrics)

Em seguida, você pode criar o arquivo de dados de blindagem:

- [Crie um arquivo de dados de blindagem e adicione guardiões](#create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard)


## <a name="obtain-a-certificate-for-remote-desktop-connection"></a>Obter um certificado para a Conexão de área de trabalho remota

Como locatários só são capazes de se conectar às suas VMs blindadas usando Conexão de área de trabalho remota ou outras ferramentas de gerenciamento remoto, é importante garantir que os locatários podem verificar a conexão ao ponto de extremidade à direita (ou seja, não há um "homem no meio" interceptando a conexão).

É uma maneira de verificar se que você estiver se conectando com o servidor pretendido instalar e configurar um certificado para os serviços de área de trabalho remota apresentar quando você iniciar uma conexão. O computador cliente se conectar ao servidor verificará se ele confia o certificado e mostrar um aviso se não existir. Em geral, para garantir que o cliente está se conectando confia no certificado, RDP são emitidos de PKI do locatário. Para obter mais informações sobre [uso de certificados nos serviços de área de trabalho remota](https://technet.microsoft.com/library/dn781533.aspx) podem ser encontradas no TechNet.

<!-- The previous link comes from Windows 2012 R2 content, but as of Sept 2016, there isn't a more recent link that covers the same information. -->

> [!NOTE]
> Ao selecionar um certificado RDP para incluir em seu arquivo de dados de blindagem, certifique-se de usar um certificado curinga. Um arquivo de dados de blindagem pode ser usado para criar um número ilimitado de máquinas virtuais. Desde que cada VM compartilharão o mesmo certificado, um certificado curinga garantirá que o certificado seja válido, independentemente do nome de host da VM.

Se você estiver avaliando as VMs blindadas e não ainda estiver pronto para solicitar um certificado da autoridade de certificação, você pode criar um certificado autoassinado no computador do locatário executando o seguinte comando do Windows PowerShell (onde *contoso.com*  é o domínio do Locatário):

``` powershell
$rdpCertificate = New-SelfSignedCertificate -DnsName '\*.contoso.com'
$password = ConvertTo-SecureString -AsPlainText 'Password1' -Force
Export-PfxCertificate -Cert $RdpCertificate -FilePath .\rdpCert.pfx -Password $password
```

## <a name="create-an-answer-file"></a>Crie um arquivo de resposta

Uma vez que o disco de modelo assinado no VMM é generalizado, os locatários são necessários para fornecer um arquivo de resposta para especializar suas VMs blindadas durante o processo de provisionamento. O arquivo de resposta (geralmente chamado do arquivo autônomo) pode configurar a máquina virtual para sua função pretendida - ou seja, ele pode instalar os recursos do Windows, registrar o certificado RDP criado na etapa anterior e executar outras ações personalizadas. Ele também fornecerá as informações necessárias para a instalação do Windows, incluindo a chave de produto e senha do administrador padrão.

Para obter informações sobre como obter e usar o **New-ShieldingDataAnswerFile** função para gerar um arquivo de resposta (arquivo Unattend. xml) para a criação de VMs blindadas, consulte [gerar um arquivo de resposta usando o Função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md). Usando a função, você pode gerar mais facilmente um arquivo de resposta que reflete opções como o seguinte:

- A VM deve ser ingressado no final do processo de inicialização?
- Você usará uma licença de volume ou uma chave de produto específico por VM?
- Você está usando DHCP ou IP estático?
- Você usará um certificado de protocolo de área de trabalho remota (RDP) que será usado para provar que a máquina virtual pertence à sua organização?
- Você deseja executar um script ao final da inicialização?
- Você está usando um servidor de Desired State Configuration (DSC) para configurações adicionais?

Arquivos de resposta usados em arquivos de dados de blindagem serão usados em todas as VMs criadas usando esse arquivo de dados de blindagem. Portanto, assegure-se de que você não codificar todas as informações específicas da VM no arquivo de resposta. O VMM oferece suporte a algumas cadeias de caracteres de substituição (consulte a tabela a seguir) no arquivo autônomo para lidar com valores de especialização que podem ser alterados de VM para VM. Não é necessário para usá-los; No entanto, se eles estiverem presentes VMM irá tirar proveito deles.

Ao criar um arquivo Unattend. XML para as VMs blindadas, tenha em mente as seguintes restrições:

-   O arquivo autônomo deve resultar na VM que está sendo desativado depois que ele foi configurado. Isso é para permitir que o VMM saber quando deve relatar para o locatário que a VM concluir o provisionamento e está pronta para uso. VMM será automaticamente ligado a VM volta quando ele detecta que ele foi desativado durante o provisionamento.

-   É altamente recomendável que você configure um certificado RDP para garantir que você está se conectando para a direita VM e não em outro computador configurado para um ataque man-in-the-middle.

-   Certifique-se de habilitar o RDP e a regra de firewall correspondente para que possa acessar a VM depois que ele foi configurado. Você não pode usar o console do VMM para VMs blindada de acesso, portanto, será necessário o RDP para se conectar à sua VM. Se você preferir gerenciar seus sistemas com a comunicação remota do Windows PowerShell, verifique se que o WinRM é habilitado, muito.

-   As cadeias de caracteres de substituição somente tem suportadas no blindado arquivos autônomos VM são os seguintes:

| Elemento substituível | Cadeia de caracteres de substituição |
|-----------|-----------|
| ComputerName        | @ComputerName@      |
| TimeZone            | @TimeZone@          |
| ProductKey          | @ProductKey@        |
| IPAddr4-1           | @IP4Addr-1@         |
| IPAddr6-1           | @IP6Addr-1@         |
| MACAddr-1           | @MACAddr-1@         |
| Prefix-1-1          | @Prefix-1-1@        |
| NextHop-1-1         | @NextHop-1-1@       |
| Prefix-1-2          | @Prefix-1-2@        |
| NextHop-1-2         | @NextHop-1-2@       |

Ao usar cadeias de caracteres de substituição, é importante garantir que as cadeias de caracteres serão preenchidas durante o processo de provisionamento da VM. Se uma cadeia de caracteres, como @ProductKey@ não for fornecido no momento da implantação, deixando o &lt;ProductKey&gt; nó no arquivo autônomo em branco, o processo de especialização falhará e não será possível se conectar à sua VM.

Além disso, observe que as cadeias de caracteres de substituição relacionados à rede até o final da tabela são usadas somente se você está aproveitando os Pools de endereços IP estáticos de VMM. O provedor de serviços de hospedagem deve ser capaz de informar se essas cadeias de caracteres de substituição são necessárias. Para obter mais informações sobre endereços IP estáticos em modelos do VMM, consulte o seguinte na documentação do VMM:

- [Diretrizes para pools de endereços IP](https://technet.microsoft.com/system-center-docs/vmm/plan/plan-network#guidelines-for-ip-address-pools) 
- [Configurar pools de endereços IP estáticos na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-network-static-address-pools)

Por fim, é importante observar que o processo de implantação de VM blindado só irá criptografar a unidade do sistema operacional. Se você implantar uma VM blindada com um ou mais unidades de dados, é altamente recomendável que você adicione um comando autônomo ou a configuração de diretiva de grupo no domínio de locatário para automaticamente criptografar as unidades de dados.

## <a name="get-the-volume-signature-catalog-file"></a>Obter o arquivo de catálogo de assinatura de volume

Arquivos de dados de blindagem também contêm informações sobre os discos de modelo que um locatário confia. Locatários adquirem as assinaturas de discos de modelo confiável na forma de um arquivo de catálogo (VSC) de assinatura de volume do disco. Essas assinaturas são validadas, em seguida, quando uma nova VM é implantada. Se nenhuma das assinaturas no arquivo de dados de blindagem coincidir com o disco de modelo tentar ser implantados com a VM (ou seja, ele foi modificado ou trocado por um disco diferente, potencialmente mal-intencionado), o processo de provisionamento falhará.

> [!IMPORTANT]
> Enquanto o VSC garante que um disco não foram violado, ainda é importante para o locatário confiar no disco em primeiro lugar. Se você for o locatário e o disco de modelo é fornecido pelo seu hoster, implante uma VM usando esse disco de modelo de teste e execute suas próprias ferramentas (antivírus, scanners de vulnerabilidade e assim por diante) para validar o disco está, na verdade, em um estado que você confia.

Há duas maneiras para adquirir o VSC de um disco de modelo:

-  O hoster (ou Locatário, se o locatário tem acesso ao VMM) usa os cmdlets do PowerShell do VMM para salvar o VSC e concede a ele para o locatário. Isso pode ser executado em qualquer computador com o console do VMM instalado e configurado para gerenciar o ambiente de hospedagem da malha do VMM. Os cmdlets do PowerShell para salvar o VSC são:

        $disk = Get-SCVirtualHardDisk -Name "templateDisk.vhdx"
    
        $vsc = Get-SCVolumeSignatureCatalog -VirtualHardDisk $disk
    
        $vsc.WriteToFile(".\templateDisk.vsc")

-  O locatário tem acesso ao arquivo de disco de modelo. Isso pode ser o caso se o locatário cria um disco de modelo carregado para um provedor de serviços de hospedagem ou se o locatário pode fazer o download do disco de modelo do hoster. Nesse caso, sem o VMM na figura, o locatário executaria o seguinte cmdlet (instalado com o recurso ferramentas de VM Blindada, parte das ferramentas de administração de servidor remoto):

        Save-VolumeSignatureCatalog -TemplateDiskPath templateDisk.vhdx -VolumeSignatureCatalogPath templateDisk.vsc

## <a name="select-trusted-fabrics"></a>Selecione as malhas confiáveis

O último componente no arquivo de dados de blindagem se relaciona com o proprietário e guardiões de uma VM. Guardiões são usadas para designar os dois o proprietário de uma VM blindada e malhas protegidas no qual ele está autorizado a executar.

Para autorizar uma malha de hospedagem para executar uma VM blindada, você deve obter os metadados do guardião do serviço de guardião de Host do provedor de serviço hospedagem. Muitas vezes, o provedor de serviços de hospedagem fornecerá esses metadados através de suas ferramentas de gerenciamento. Em um cenário empresarial, você pode ter acesso direto a obter os metadados por conta própria.

Você ou seu provedor de serviços de hospedagem pode obter os metadados do guardião do HGS executando uma das seguintes ações:

-  Obter os metadados do guardião diretamente do HGS executando o seguinte comando do Windows PowerShell, ou navegando até o site e salvar o arquivo XML que é exibido:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

-  Obter os metadados do guardião do VMM usando os cmdlets do PowerShell do VMM:

        $relecloudmetadata = Get-SCGuardianConfiguration

        $relecloudmetadata.InnerXml | Out-File .\RelecloudGuardian.xml -Encoding UTF8

<!-- Note that the VMM PowerShell cmdlets aren't Windows PowerShell, so "VMM PowerShell" is the correct terminology for them. -->

Obter guardião de arquivos de metadados para cada malha protegida, que você deseja autorizar suas VMs blindadas para executar em antes de continuar.

## <a name="create-a-shielding-data-file-and-add-guardians-using-the-shielding-data-file-wizard"></a>Crie um arquivo de dados de blindagem e adicione guardiões usando o Assistente de arquivo de dados de blindagem

Execute o Assistente de arquivo de dados de blindagem para criar um arquivo de blindagem (PDK) de dados. Aqui, você irá adicionar o certificado RDP, arquivo, catálogos de assinatura de volume, guardião proprietário autônomo e os metadados do guardião baixado obtidos na etapa anterior.

1.  Instale **ferramentas de administração de servidor remoto &gt; Feature Administration Tools &gt; ferramentas de VM Blindada** em seu computador usando o Gerenciador do servidor ou o seguinte comando do Windows PowerShell:

        Install-WindowsFeature RSAT-Shielded-VM-Tools

2.  Abrir o Assistente de arquivo de dados de blindagem da seção de ferramentas do administrador, no menu Iniciar ou executando o seguinte executável **c:\\Windows\\System32\\ShieldingDataFileWizard.exe**.

3.  Na primeira página, use a segunda caixa de seleção de arquivo para escolher um local e o nome para seu arquivo de dados de blindagem. Normalmente, você deve nomear um arquivo de dados de blindagem depois que a entidade que possui todas as VMs criadas com esses dados de blindagem (por exemplo, RH, IT, Finance) e a função de carga de trabalho está executando (por exemplo, servidor de arquivos, servidor web ou qualquer outra coisa configurado pelo arquivo autônomo). Deixe o botão de opção definido como **os dados para modelos de Blindadas de blindagem**.

    > [!NOTE]
    > O Assistente de arquivo de dados de blindagem, você observará duas opções abaixo:
    >- **Dados de blindagem para modelos de Blindadas**
    >- **Os dados de blindagem para VMs existentes e os modelos não blindado**<br>
    > A primeira opção é usada ao criar novas VMs blindadas de modelos blindados. A segunda opção permite que você crie dados de blindagem que só podem ser usados ao converter as VMs existentes ou criar VMs blindadas de modelos não blindado.

    ![Assistente de arquivo de dados de blindagem, seleção de arquivo](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-01.png)

       Além disso, você deve escolher se as VMs criadas usar esse arquivo de dados de blindagem será realmente blindado ou configurado no modo de "suporte de criptografia". Para obter mais informações sobre essas duas opções, consulte [quais são os tipos de máquinas virtuais que pode ser executados por uma malha protegida?](guarded-fabric-and-shielded-vms.md#what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run).

    > [!IMPORTANT]
    > Preste muita atenção para a próxima etapa conforme ela define o proprietário de suas VMs blindadas e em quais malhas de suas VMs blindadas serão ser autorizados a executar.<br>Posse da **guardião proprietário** é necessária para alterar posteriormente uma VM blindada existente da **Blindadas** para **suporte para a criptografia** ou vice-versa.
    
4.  Seu objetivo nesta etapa é composto por duas etapas:

    - Crie ou selecione um guardião do proprietário que você como o proprietário da VM

    - Importar o guardião que você baixou a partir do provedor de hospedagem (ou seu próprio) serviço guardião de Host na etapa anterior

    Para designar um guardião proprietário existente, selecione o apropriado guardian no menu suspenso. Somente guardiões instalados no computador local com as chaves privadas intactas aparecerão nessa lista. Você também pode criar seu próprios guardião do proprietário, selecionando **gerenciar Local guardiões** no canto inferior canto direito e clicando em **criar** e Concluindo o assistente.

    Em seguida, podemos importar os metadados do guardião baixado anteriormente novamente usando o **proprietário e guardiões** página. Selecione **gerenciar Local guardiões** do canto inferior direito. Use o **importação** recurso para importar o arquivo de metadados do guardião. Clique em **Okey** depois que você tiver importado ou adicionado todos os guardiões necessárias. Como prática recomendada, nome guardiões após o hospedagem serviço provedor ou data center corporativo que eles representam. Por fim, selecione todos os guardiões que representam os data centers em que sua máquina virtual blindada está autorizada a executar. Você não precisa selecionar novamente o guardião de proprietário. Clique em **próxima** após a conclusão.

    ![Assistente de arquivo de dados, proprietário e guardiões de blindagem](../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-wizard-02.png)

5.  Na página de qualificadores de ID de Volume, clique em **adicionar** para autorizar um disco de modelo assinado no seu arquivo de dados de blindagem. Quando você seleciona um VSC na caixa de diálogo, ele mostrará informações sobre o nome do disco, versão e o certificado que foi usado para assiná-lo. Repita esse processo para cada disco de modelo que você deseja autorizar.

6.  Sobre o **valores de especialização** , clique em **procurar** para selecionar o arquivo Unattend. XML que será usado para especializar suas VMs.

    Use o **adicionar** botão na parte inferior para adicionar arquivos adicionais para o PDK que são necessários durante o processo de especialização. Por exemplo, se o arquivo autônomo é instalar um certificado RDP na máquina virtual (conforme descrito em [gerar um arquivo de resposta usando a função New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md)), você deve adicionar o arquivo RDPCert.pfx mencionado no autônoma arquivo aqui. Observe que os arquivos que você especificar aqui serão automaticamente copiados na unidade c:\\temp\\ na VM que é criado. O arquivo autônomo deve esperar que os arquivos a serem nessa pasta, ao fazer referência a eles pelo caminho.

7.  Examine suas seleções na próxima página e, em seguida, clique em **gerar**.

8.  Feche o assistente após sua conclusão.

## <a name="create-a-shielding-data-file-and-add-guardians-using-powershell"></a>Crie um arquivo de dados de blindagem e adicione guardiões usando o PowerShell

Como uma alternativa para o Assistente de arquivo de dados de blindagem, você pode executar [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/new-shieldingdatafile?view=win10-ps) para criar um arquivo de dados de blindagem.

Todos os arquivos de dados de blindagem precisam ser configurado com o proprietário correto e os certificados de guardião para autorizar a suas VMs blindadas para ser executado em uma malha protegida.
Você pode verificar se você tiver qualquer guardiões instalados localmente, executando [Get-HgsGuardian](https://docs.microsoft.com/powershell/module/hgsclient/get-hgsguardian?view=win10-ps). Guardiões proprietário têm chaves privadas ao guardiões para seu datacenter normalmente não.

Se você precisar criar um guardião do proprietário, execute o seguinte comando:

```powershell
New-HgsGuardian -Name "Owner" -GenerateCertificates
```

Este comando cria um par de certificados de assinatura e criptografia no repositório de certificados do computador local sob a pasta "Blindado VM Local certificados".
Você precisará de certificados de proprietário e suas chaves privadas correspondentes para unshield uma máquina virtual, portanto, verifique se esses certificados são submetidos a backup e protegidos contra roubo.
Um invasor com acesso aos certificados de proprietário pode usá-los para iniciar sua máquina virtual blindada ou alterar sua configuração de segurança.

Se você precisar importar informações de guardião de uma malha protegida em que você deseja executar sua máquina virtual (o seu datacenter primário, backup datacenters, etc.), execute o seguinte comando para cada [recuperado do arquivo de metadados do seu malhas protegidas ](#select-trusted-fabrics).

```powershell
Import-HgsGuardian -Name 'EAST-US Datacenter' -Path '.\EastUSGuardian.xml'
```

> [!TIP]
> Se você tiver usado certificados autoassinados ou certificados registrados com HGS está expirado, você talvez precise usar o `-AllowUntrustedRoot` e/ou `-AllowExpired` sinalizadores com o comando de importação HgsGuardian para ignorar as verificações de segurança.

Você também precisará [obter um catálogo de assinatura de volume](#get-the-volume-signature-catalog-file) para cada disco de modelo que você deseja usar com esse arquivo de dados de blindagem e uma [arquivo de resposta de dados de blindagem](#create-an-answer-file) para permitir que o sistema operacional para concluir sua especialização tarefas automaticamente.
Por fim, decida se deseja que sua VM seja totalmente blindado ou apenas habilitadas para vTPM.
Use `-Policy Shielded` de uma VM blindada totalmente ou `-Policy EncryptionSupported` para um vTPM habilitado VM que permite conexões de console básico e o PowerShell Direct.

Quando tudo estiver pronta, execute o seguinte comando para criar o arquivo de dados de blindagem:

```powershell
$viq = New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\marketing-ws2016.vsc' -VersionRule Equals
New-ShieldingDataFile -ShieldingDataFilePath "C:\temp\Marketing-LBI.pdk" -Policy EncryptionSupported -Owner 'Owner' -Guardian 'EAST-US Datacenter' -VolumeIDQualifier $viq -AnswerFile 'C:\temp\marketing-ws2016-answerfile.xml'
```

No comando acima, o guardião chamado "Proprietário" (obtido em Get-HgsGuardian) será capaz de alterar a configuração de segurança da VM no futuro, enquanto 'Datacenter EAST-US' podem executar a VM, mas não alterar suas configurações.
Se você tiver mais de um guardião, separado os nomes dos guardiões com vírgulas como `'EAST-US Datacenter', 'EMEA Datacenter'`.
O qualificador de ID do volume Especifica se você confia apenas a versão exata (igual a) de disco do modelo ou em versões futuras (GreaterThanOrEquals) também.
O nome do disco e o certificado de assinatura devem corresponder exatamente para a comparação de versão a ser considerado no momento da implantação.
Você pode confiar em mais de um disco de modelo, fornecendo uma lista separada por vírgulas de volume qualificadores de ID para o `-VolumeIDQualifier` parâmetro.
Por fim, se você tiver outros arquivos que precisam para acompanhar o arquivo de resposta com a VM, use o `-OtherFile` parâmetro e fornece uma lista separada por vírgulas de caminhos de arquivo.

Consulte a documentação de cmdlet do [New-ShieldingDataFile](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-ShieldingDataFile?view=win10-ps) e [New VolumeIDQualifier](https://docs.microsoft.com/powershell/module/shieldedvmdatafile/New-VolumeIDQualifier?view=win10-ps) para saber mais sobre outras maneiras de configurar seu arquivo de dados de blindagem.

## <a name="see-also"></a>Consulte também

- [Implantar VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
