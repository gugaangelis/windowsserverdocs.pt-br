---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Introdução à virtualização do AD DS (Serviços de Domínio Active Directory) (Nível 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878087"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introdução à virtualização do AD DS (Serviços de Domínio Active Directory) (Nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A virtualização dos ambientes do AD DS (Serviços de Domínio Active Directory) já está sendo realizada há vários anos. Começando com o Windows Server 2012, o AD DS fornece um suporte mais abrangente para a virtualização de controladores de domínio com a introdução de recursos de virtualização segura.

## <a name="safe-virtualization-of-domain-controllers"></a>Virtualização segura dos controladores de domínio

Os ambientes virtuais apresentam desafios exclusivos a cargas de trabalho distribuídas que dependem de um esquema de replicação lógico baseado no relógio. A replicação do AD DS, por exemplo, usa um valor que aumenta continuamente (conhecido como USN ou Números de Sequência de Atualização) atribuído às transações em cada controlador de domínio. Instância de banco de dados de cada controlador de domínio também recebe uma identidade, conhecida como InvocationID. A InvocationID do controlador de domínio juntamente com seu USN atuam como um identificador exclusivo associado a cada transação de gravação executada em cada controlador de domínio e deve ser única na floresta.

A replicação do AD DS usa a InvocationID e os USNs em cada controlador de domínio para determinar as alterações que devem ser replicadas para outros controladores de domínio. Se um controlador de domínio é revertido no tempo fora de reconhecimento do controlador de domínio e o USN for reutilizado para uma transação totalmente diferente, a replicação não será convergida porque outros controladores de domínio vão achar que já receberam as atualizações associado com o USN reutilizado no contexto dessa invocationid.

Por exemplo, a ilustração a seguir mostra a sequência de eventos que ocorre no Windows Server 2008 R2 e em sistemas operacionais anteriores quando é detectada a reversão de USN no VDC2, o controlador de domínio de destino em execução na máquina virtual. Nesta ilustração, a detecção da reversão do USN ocorre no VDC2 quando um parceiro de replicação detecta que o VDC2 enviou um valor de USN atual visto anteriormente pelo parceiro de replicação, que indica que banco de dados do VDC2 foi revertido tempo incorretamente.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Uma máquina virtual (VM) facilita para os administradores do hipervisor reverter um domínio USNs do controlador (seu relógio lógico), por exemplo, aplicar um instantâneo fora de reconhecimento do controlador de domínio. Para obter mais informações sobre USN e reversão de USN, inclusive outra ilustração para demonstrar instâncias não detectadas de reversão de USN, consulte [USN e Reversão de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

Começando com o Windows Server 2012, controladores de domínio virtual do AD DS hospedados em plataformas de hipervisor que expõem um identificador chamado ID de geração de VM detectam e tomam medidas de segurança necessárias para proteger o ambiente do AD DS, se a máquina virtual é revertida de volta no tempo pelo aplicativo de um instantâneo VM. O design da ID de Geração de VM utiliza um mecanismo do fornecedor do hipervisor independente para expor esse identificador no espaço do endereço da máquina virtual convidada, assim a experiência de virtualização segura fica igualmente disponível em qualquer hipervisor com suporte à ID de Geração de VM. Esse identificador pode ser exemplificado por serviços e aplicativos executados na máquina virtual para detectar se a máquina virtual foi revertida.

### <a name="BKMK_HowSafeguardsWork"></a>Como funcionam essas garantias de virtualização?
Durante a instalação do controlador de domínio, o AD DS inicialmente armazena o identificador ID de geração de VM como parte do atributo msDS-GenerationID no objeto de computador do controlador de domínio em seu banco de dados (também conhecido como a árvore de informações do diretório ou a DIT). A ID de Geração de VM é acompanhada de forma independente por um driver do Windows na máquina virtual.

Quando o administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID de Geração de VM do driver da máquina virtual é comparado com o valor na DIT.

Se os dois valores forem diferentes, a invocationID será redefinida e o pool RID descartado, impedindo assim a reutilização do USN. Se os valores forem iguais, a transação será confirmada como de costume.

O AD DS também compara o valor atual da ID de Geração de VM da máquina virtual com o valor na DIT toda vez que o controlador de domínio é reinicializado e, se forem diferentes, ele redefinirá a invocationID, descartará o pool RID e atualizará a DIT com o novo valor. Ele também sincronizará a pasta SYSVOL de maneira não autoritativa para concluir a restauração segura. Isso permite estender as garantias para a aplicação de instantâneos às VMs que foram desligadas. Essas garantias introduzidas no Windows Server 2012 permitem que os administradores do AD DS aproveitar as vantagens exclusivas da implantação e gerenciamento de controladores de domínio em um ambiente virtualizado.

A ilustração a seguir mostra como as garantias de virtualização são aplicadas quando o reversão do mesmo USN é detectado em um controlador de domínio virtualizado que executa o Windows Server 2012 em um hipervisor que dá suporte a VM-GenerationID.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Neste caso, quando o hipervisor detecta uma alteração no valor da ID de Geração de VM, as garantias de virtualização são disparadas, incluindo a redefinição da InvocationID no controlador de domínio virtualizado (de A a B no exemplo anterior) e a atualização do valor da ID de Geração de VM salvo na VM para corresponder ao novo valor (G2) armazenado pelo hipervisor. As garantias asseguram que a replicação seja convergida nos dois controladores de domínio.

Com o Windows Server 2012, o AD DS aplica garantias a controladores de domínio virtuais hospedados em hipervisores com reconhecimento de VM-GenerationID e garante que a aplicação acidental de instantâneos ou outros hipervisor-mecanismos similares habilitados por pôde reverter uma máquina virtual estado da máquina não prejudique o ambiente do AD DS (por evitando problemas de replicação, como USNs ou objetos remanescentes). No entanto, a restauração de um controlador de domínio através da aplicação do instantâneo de uma máquina virtual não é recomendada como mecanismo alternativo de backup do controlador de domínio. É recomendado continuar usando o Backup do Windows Server ou outras soluções de backup baseadas no gravador VSS.

> [!CAUTION]
> Se um controlador de domínio em um ambiente de produção for revertido acidentalmente para um instantâneo, é recomendável que você consulte os fornecedores para os aplicativos e serviços hospedados na máquina virtual, para obter orientação sobre como verificar o estado desses programas após restauração de instantâneo.

Para obter mais informações, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonagem do controlador de domínio virtualizado
Começando com o Windows Server 2012, os administradores podem facilmente e com segurança implantar controladores de domínio de réplica, copiando um controlador de domínio virtual existente. Em um ambiente virtual, os administradores não precisam mais implantar várias vezes a imagem preparada de um servidor usando o sysprep.exe, promover o servidor a controlador de domínio e seguir os requisitos de configuração adicionais para implantar cada controlador de domínio replicado.

> [!NOTE]
> Os administradores precisam seguir os processos existentes para implantar o primeiro controlador de domínio em um domínio, por exemplo, usando o sysprep.exe para preparar o VHD (disco rígido virtual) do servidor, promover o servidor a controlador de domínio e, em seguida, cumprir com os requisitos de configuração adicionais. Em um cenário de recuperação de desastre, use o backup do servidor mais recente para restaurar o primeiro controlador de domínio em um domínio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Cenários que aproveitam a clonagem do controlador de domínio virtual

-   Implantação rápida de controladores de domínio adicionais em um novo domínio

-   Restauração rápida da continuidade dos negócios durante a recuperação de desastre restaurando a capacidade do AD DS através da implantação rápida dos controladores de domínio usando a clonagem

-   Otimização das implantações de nuvens privadas aproveitando o provisionamento elástico dos controladores de domínio para acomodar requisitos de maior escala

-   Rápido provisionamento de ambientes de teste que permitem implantação e teste de novos recursos e funcionalidades antes da distribuição de produção

-   Rápido atendimento das crescentes necessidades de capacidade em filiais através da clonagem dos controladores de domínio existentes nas filiais

Ao implantar rapidamente um grande número de controladores de domínio, continue seguindo os procedimentos existentes para validação da integridade de cada controlador de domínio após o término da instalação. Implante os controladores de domínio em lotes de tamanho razoável para conseguir validar sua integridade após a conclusão de cada lote de instalações. O tamanho recomendado do lote é 10. Para obter mais informações, consulte [Etapas para implantação de um controlador de domínio virtualizado de clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Separação clara das responsabilidades
A autorização para clonar controladores de domínio virtualizados é controlada pelo administrador do AD DS. Para que os administradores do hipervisor implantem mais controladores de domínio copiando controladores de domínio virtuais, o administrador do AD DS tem que selecionar e autorizar um controlador de domínio e, em seguida, executar as etapas preparatórias para habilitá-lo como a fonte da clonagem.

Com o provisionamento da máquina virtual normalmente sob controle do administrador do hipervisor, os administradores do hipervisor podem provisionar máquinas virtuais de controladores de domínio replicados copiando os controladores de domínio virtualizados que foram autorizados e preparados para clonagem pelo administrador do AD DS.

> [!WARNING]
> Qualquer pessoa com permissão para administrar o hipervisor que hospeda o controlador de domínio virtual deve ser totalmente de confiança e auditada no ambiente.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Como funciona a clonagem do controlador de domínio virtual?
O processo de clonagem envolve a cópia do VHD do controlador de domínio virtual existente (ou, para configurações mais complexas, a VM do controlador de domínio), sua autorização para clonagem no AD DS e criando um arquivo de configuração de clone. Isso reduz o número de etapas e o tempo envolvidos na implantação de um controlador de domínio virtual replicado, pois acaba com as tarefas de implantação repetitivas.

O controlador de domínio de clone usa os seguintes critérios para detectar que é uma cópia de outro controlador de domínio:

1.  O valor da ID de Geração de VM fornecido pela máquina virtual é diferente do valor armazenado na DIT.

    > [!NOTE]
    > A plataforma do hipervisor deve dar suporte à ID de geração de VM (Windows Server 2012 Hyper-V dá suporte à ID de geração de VM).

2.  Presença de um arquivo chamado DCCloneConfig.xml em um dos seguintes locais:

    -   O diretório em que a DIT reside

    -   %windir%\NTDS

    -   A raiz de uma unidade de mídia removível

Quando os critérios são atendidos, ele passa pelo processo de clonagem para se provisionar como controlador de domínio replicado.

O controlador de domínio de clone usa o contexto de segurança do controlador de domínio de origem (o controlador de domínio cuja cópia ele representa) entre em contato com o detentor da função de mestre de operações do ferramenta controlador de domínio primário (PDC) do Windows Server 2012 emulador (também conhecido como operações de mestre únicas flexíveis ou FSMO). O emulador do PDC deve estar executando o Windows Server 2012, mas ele não precisa estar em execução em um hipervisor.

> [!NOTE]
> Se você tiver uma extensão de esquema com atributos que fazem referência ao controlador de domínio de origem e o atributo estiver em um dos objetos copiados (objeto de computador, objeto Configurações NTDS) para criar o clone, esse atributo não será copiado nem atualizado para fazer referência ao controlador de domínio de clone.

Após verificar se o controlador de domínio solicitado está autorizado para clonagem, o emulador do PDC criará uma nova identidade da máquina, incluindo nova conta, SID, nome e senha, que a identifica como controlador de domínio replicado e envia essas informações de volta ao clone. Em seguida, o controlador de domínio de clone prepara os arquivos do banco de dados do AD DS para atuar como uma réplica e também limpa o estado da máquina.

Para obter mais informações, consulte [Virtualized domain controller cloning architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componentes de clonagem
Os componentes de clonagem incluem os novos cmdlets no módulo Active Directory para Windows PowerShell e os arquivos XML associados:

-   **New-ADDCCloneConfigFile** "Esse cmdlet cria e coloca o dccloneconfig. XML no local certo para garantir que ele está disponível para disparar a clonagem. Ele também executa as verificações de pré-requisitos para que a clonagem seja bem-sucedida. Ele está incluído no módulo Active Directory para Windows PowerShell. É possível executá-lo localmente em um controlador de domínio virtualizado que esteja preparado para clonagem ou remotamente usando a opção -offline. É possível especificar as configurações do controlador de domínio de clone, como seu nome, site e endereço IP.

    Veja a seguir as verificações de pré-requisitos que são executadas:

    > [!NOTE]
    > As verificações de pré-requisitos não são executadas quando a "opção offline é usada. Para obter mais informações, consulte [Executando New-ADDCCloneConfigFile em modo offline](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   O controlador de domínio que está sendo preparado tem autorização para clonagem (é membro do grupo **Controladores de Domínio Clonáveis**)

    -   O emulador PDC executa o Windows Server 2012.

    -   Todos os programas ou serviços listados após a execução de **Get-ADDCCloningExcludedApplicationList** estão incluídos no CustomDCCloneAllowList.xml (explicado mais detalhadamente no fim desta lista de componentes de clonagem).

-   **Dccloneconfig. XML** "para a clonagem bem-sucedida de um controlador de domínio virtualizado, esse arquivo deve estar presente no diretório onde a DIT reside, *%windir%\NTDS*, ou a raiz de uma unidade de mídia removível. Além de ser usado como um dos gatilhos para detectar e iniciar a clonagem, ele também oferece um recurso para especificar definições de configuração para o controlador de domínio de clone.

    O esquema e um arquivo de exemplo para o arquivo dccloneconfig. XML são armazenados em todos os computadores do Windows Server 2012 em:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    É recomendado usar o cmdlet New-ADDCCloneConfigFile para criar o arquivo DCCloneConfig.xml. Embora você também possa usar o arquivo de esquema com um editor de XML compatível para criar esse arquivo, a edição manual do arquivo aumenta a probabilidade de erros. Se você editar o arquivo, faça isso usando editores de XML compatíveis, como o Visual Studio, o [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)ou aplicativos de terceiros (não use o Bloco de Notas).

-   **Get-ADDCCloningExcludedApplicationList** "Este cmdlet é executado no controlador de domínio de origem antes de iniciar o processo de clonagem para determinar quais serviços ou programas instalados não estão na lista de padrões com suporte, Defaultdccloneallowlist. XML, ou uma inclusão definida pelo usuário chamada customdccloneallowlist de lista e, portanto, não foram avaliadas impacto de clonagem.

    Esse cmdlet procura os serviços no Gerenciador de Controle de Serviços do controlador de domínio de origem e os programas instalados listados em **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** que não estão especificados na lista padrão (DefaultDCCloneAllowList.xml) ou, se houver, na lista de inclusões definida pelo usuário (arquivo CustomDCCloneAllowList.xml). A lista de aplicativos e serviços retornada após a execução do cmdlet é a diferença entre o que já foi fornecido no arquivo DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml e a lista construída em tempo de execução, com base no que foi instalado no controlador de domínio de origem. Se você determinar que os serviços e programas podem ser clonados com segurança, a saída de serviços e programas de Get-ADDCCloningExcludedApplicationList pode ser adicionada ao arquivo customdccloneallowlist. XML. Para determinar se um serviço ou programa instalado pode ser clonado com segurança, avalie as seguintes condições:

    -   O serviço ou programa instalado é afetado pela identidade da máquina, como nome, SID, senha, etc?

    -   O serviço ou programa instalado armazena algum estado localmente no computador que possa afetar sua funcionalidade no clone?

    Você deve trabalhar com o fornecedor de software do aplicativo para determinar se o serviço ou programa pode ser clonado com segurança.

    > [!NOTE]
    > Antes de provisionar outros serviços ou programas no arquivo CustomDCCloneAllowList.xml, verifique se você tem a licença necessária para copiar o software que está na máquina virtual.

    Se não for possível clonar os aplicativos, remova-os do controlador de domínio de origem antes de criar a mídia de clone. Se aparecer um aplicativo na saída do cmdlet, mas se ele não estiver incluído no arquivo CustomDCCloneAllowList.xml, haverá falha na clonagem. Para a clonagem ser bem-sucedida, a saída do cmdlet não deve listar nenhum serviço ou programa. Em outras palavras, o aplicativo deve estar incluído no arquivo CustomDCCloneAllowList.xml ou ser removido do controlador de domínio de origem.

    A tabela a seguir explica as opções para execução do Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argumento|Explicação|
    |*<no argument specified>*|Exibe uma lista de serviços ou programas no console que não foram considerados na clonagem. Se já existir um CustomDCCloneAllowList.XML em qualquer um dos locais permitidos, ele usará esse arquivo para exibir os serviços e programas remanescentes (que poderá não ser nada se as listas forem correspondentes).|
    |-GenerateXml|Cria o arquivo CustomDCCloneAllowList.XML populado com os serviços e programas listados no console.|
    |-Force|Substitui um arquivo CustomDCCloneAllowList.XML existente.|
    |-Path|Caminho da pasta para criar o CustomDCCloneAllowList.XML.|

-   **Defaultdccloneallowlist** "Este arquivo está presente por padrão em todos os Windows Server 2012 domínio controlador in a *%windir%\system32*. Por padrão, ele lista os serviços e programas instalados que podem ser clonados com segurança. Você não deve alterar o local nem o conteúdo desse arquivo, senão haverá falha na clonagem.

-   **Customdccloneallowlist. XML** "Se você tiver serviços ou programas instalados que residam em seu controlador de domínio de origem que estão fora dos listados no arquivo Defaultdccloneallowlist XML, os serviços e programas devem ser incluídos neste arquivo. Para localizar os serviços ou programas instalados que não estão listados no arquivo DefaultDCCloneAllowList.xml, execute o cmdlet **Get-ADDCCloningExcludedApplicationList** . Você deve usar o **"GenerateXml** argumento para gerar o arquivo XML.

    O processo de clonagem verifica os seguintes locais na ordem desse arquivo e usa o primeiro arquivo XML encontrado, independentemente do conteúdo de outras pastas:

    1.  A seguinte chave do Registro:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Diretório de Trabalho DSA

    3.  %systemroot%\NTDS

    4.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade

### <a name="deployment-scenarios"></a>Cenários de implantação
Os cenários de implantação a seguir têm suporte para clonagem do controlador de domínio virtual:

-   Implante um controlador de domínio de clone fazendo uma cópia do arquivo de disco rígido virtual (vhd) de um controlador domínio de origem.

-   Implantação de um controlador de domínio de clone copiando a máquina virtual de um controlador de domínio de origem usando as semânticas de exportação/importação expostas pelo hipervisor.

> [!NOTE]
> As etapas na seção [etapas para implantar um controlador de domínio virtualizado de clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) demonstram como copiar uma máquina virtual usando o recurso de importação/exportação do Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Etapas para implantar um controlador de domínio virtualizado de clone

-   [Pré-requisitos](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Etapa 1: Conceder a permissão a ser clonado do controlador de domínio virtualizado de origem](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Etapa 2: Execute o cmdlet Get-ADDCCloningExcludedApplicationList](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Etapa 3: Run New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Etapa 4: Exportar e importar a máquina virtual do controlador de domínio de origem](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Pré-requisitos

-   Para concluir as etapas dos procedimentos a seguir, você deve ser membro do grupo Admins. do Domínio ou ter as permissões equivalentes.

-   Os comandos do Windows PowerShell usados neste guia devem ser executados em um prompt de comando elevado. Para fazer isso, clique com botão direito do **Windows PowerShell** ícone e clique **executar como administrador**.

-   Um servidor Windows Server 2012 com a função de servidor Hyper-V instalada (**HyperV1**).

-   Um segundo servidor do Windows Server 2012 com a função de servidor Hyper-V instalada (**HyperV2**).

    > [!NOTE]
    > -   Se estiver usando outro hipervisor, você deverá entrar em contato com o fornecedor do hipervisor para verificar se ele dá suporte à ID de Geração de VM. Se o hipervisor não der suporte à ID de Geração de VM e você tiver fornecido um DCCloneConfig.xml, a nova VM será inicializada em DSRM (Modo de Restauração dos Serviços de Diretório).
    > -   Para aumentar a disponibilidade do serviço do AD DS, este guia recomenda e inclui instruções sobre como usar dois hosts Hyper-V diferentes, o que ajuda a evitar um possível ponto único de falha. No entanto, você não precisa de dois hosts Hyper-V para realizar a clonagem do controlador de domínio virtual.
    > -   Você precisa ser membro do grupo local de administradores em cada servidor Hyper-V (**HyperV1** e **HyperV2**).
    > -   Para importar e exportar com êxito um arquivo VHD usando o Hyper-V, as chaves de rede virtual nos dois hosts Hyper-V devem ter o mesmo nome. Por exemplo, se você tem uma chave de rede virtual no **HyperV1** chamada VNet, também vai precisar de uma chave de rede virtual no **HyperV2** chamada VNet.
    > -   Se os dois hosts Hyper-V (**HyperV1** e **HyperV2**) tiverem processadores diferentes, desligue a máquina virtual (**VirtualDC1**) que você pretende exportar, clique com o botão direito do mouse na VM, clique em **Configurações**, **Processador** e, em **Compatibilidade do processador**, selecione **Migrar para um computador físico com versão diferente de processador** e clique em **OK**.

-   Um implantado Windows Server 2012 controlador de domínio (virtualizado ou físico) que hospeda a função de emulador do PDC (**DC1**). Para verificar se a função de emulador PDC é hospedada em um controlador de domínio do Windows Server 2012, execute o seguinte comando do Windows PowerShell:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    O valor OperatingSystemVersion deve ser retornado como a versão 6.2. Se necessário, você pode transferir a função de emulador PDC para um controlador de domínio que executa o Windows Server 2012. Para obter mais informações, consulte [Usando o Ntdsutil.exe para executar ou transferir funções FSMO para um controlador de domínio](https://support.microsoft.com/kb/255504).

-   Um controlador de domínio virtualizado convidado do Windows Server 2012 implantado (**VirtualDC1**) que está no mesmo domínio que o controlador de domínio do Windows Server 2012 hospeda a função de emulador do PDC (**DC1**). Esse será o controlador de domínio de origem usado para a clonagem. O controlador de domínio virtual convidado será hospedado em um servidor Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Para a clonagem ser bem-sucedida, o controlador de domínio de origem usado para criar o clone não pode ser de um controlador de domínio que tenha sido rebaixado desde a criação da mídia VHD de origem.
    > -   Desligue o controlador de domínio de origem antes de copiar a VM ou seu VHD.
    > -   Não convém clonar um VHD nem restaurar um instantâneo que seja anterior ao valor do tempo de vida da marca de exclusão (ou ao valor do tempo de vida do objeto excluído se a Lixeira do Active Directory estiver habilitada). Se estiver copiando um VHD de um controlador de domínio existente, verifique se o arquivo VHD não é anterior ao valor do tempo de vida da marca de exclusão (por padrão, 60 dias). Não convém copiar um VHD de um controlador de domínio em execução para criar a mídia de clone.

    Ejete qualquer VFD (unidade de disquete virtual) que possa estar no controlador de domínio de origem. Isso pode provocar um problema de compartilhamento ao tentar importar a nova VM.

    Somente o Windows Server 2012 controladores de domínio hospedados em um hipervisor de VM-GenerationID podem ser usados como uma fonte para clonagem. O controlador de domínio de origem Windows Server 2012 usado para clonagem deve ser em um estado íntegro. Para determinar o estado do controlador de domínio de origem, execute [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Para obter uma melhor compreensão da saída retornada pelo dcdiag, consulte [o que faz na verdade o DCDIAG... fazer? ](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Se o controlador de domínio de origem for um servidor DNS, o controlador de domínio clonado também será um servidor DNS. Convém escolher um servidor DNS que hospede apenas zonas integradas ao Active Directory.

    As configurações do cliente DNS não são clonadas, mas são especificadas no arquivo DCCloneConfig.xml. Se não forem especificadas, o controlador de domínio clonado apontará ele mesmo como o servidor DNS preferencial por padrão. O controlador de domínio clonado não terá uma delegação de DNS. O administrador da zona DNS pai deve atualizar a delegação de DNS do controlador de domínio clonado conforme necessário.

    > [!WARNING]
    > As garantias de virtualização não são estendidas para o AD LDS (Active Directory Lightweight Directory Services). Portanto, não convém tentar clonar um controlador de domínio do AD DS que hospede uma instância do AD LDS adicionado essa instância do AD LDS ao CustomDCCloneAllowList.xml. Como o AD LDS não tem reconhecimento de ID de Geração de VM, a clonagem de um controlador de domínio com AD LDS pode causar uma divergência induzida por reversão de USN no conjunto de configurações do AD LDS.

    As seguintes funções de servidor não têm suporte para clonagem:

    -   Protocolo DHCP

    -   Serviços de certificados do Active Directory (AD CS)

    -   AD LDS (Active Directory Lightweight Directory Services)

### <a name="bkmk4_grant_source"></a>Etapa 1: conceder permissão para clonagem do controlador de domínio virtualizado de origem
Neste procedimento, você concede permissão para clonar o controlador de domínio de origem usando o **Centro Administrativo do Active Directory** para adicionar o controlador de domínio de origem ao grupo **Controladores de Domínio Clonáveis**.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Para conceder permissão para clonar o controlador de domínio virtualizado de origem

1.  Em qualquer controlador de domínio que esteja no mesmo domínio que o controlador que está sendo preparado para clonagem (**VirtualDC1**), abra o ADAC ( **Centro Administrativo do Active Directory** ), localize o objeto do controlador de domínio virtualizado (os controladores de domínio normalmente estão localizados no contêiner **Controladores de Domínio** no ADAC), clique nele com o botão direito do mouse, escolha **Adicionar a grupo** e, em **Digite o nome do objeto a ser selecionado** , digite **Cloneable Domain Controllers** e clique em **OK**.

    A atualização da associação a um grupo realizada nesta etapa deve ser replicada ao emulador do PDC antes da execução da clonagem. Se o **controladores de domínio Clonáveis** grupo não for encontrado, a função de emulador do PDC não poderá ser hospedada em um controlador de domínio que executa o Windows Server 2012.

    > [!NOTE]
    > Para abrir o ADAC em um controlador de domínio do Windows Server 2012, abra o Windows PowerShell e digite **dsac.exe**.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O seguinte cmdlet do Windows PowerShell executa a mesma função que o procedimento anterior:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Etapa 2: executar o cmdlet Get-ADDCCloningExcludedApplicationList
Neste procedimento, execute o cmdlet `Get-ADDCCloningExcludedApplicationList` no controlador de domínio virtualizado de origem para identificar qualquer programa ou serviço que não tenha sido avaliado para clonagem. Você precisa executar o cmdlet Get-ADDCCloningExcludedApplicationList antes do cmdlet New-ADDCCloneConfigFile, porque se o cmdlet New-ADDCCloneConfigFile detectar um aplicativo excluído, ele não criará um arquivo DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Para identificar aplicativos ou serviços que sejam executados em um controlador de domínio de origem e que não tenham sido avaliados para clonagem

1.  No controlador de domínio de origem (**VirtualDC1**), clique em **Gerenciador do Servidor**, em **Ferramentas**, **Módulo Active Directory para Windows PowerShell** e digite o seguinte comando:


    Get-ADDCCloningExcludedApplicationList


2.  Verifique a lista retornada de serviços e programas instalados com o fornecedor do software para determinar se eles podem ser clonados com segurança. Se não for possível clonar com segurança os aplicativos ou serviços da lista, remova-os do controlador de domínio de origem para não haver falha na clonagem.

3.  Para o conjunto de serviços e programas instalados determinados para clonagem segura, execute o comando novamente com o **"GenerateXML** para provisionar esses serviços e programas no **customdccloneallowlist. XML**  arquivo.


    Get-ADDCCloningExcludedApplicationList – GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Etapa 3: Executar novo-ADDCCloneConfigFile
Execute o New-ADDCCloneConfigFile no controlador de domínio de origem e, opcionalmente, especifique as definições de configuração do controlador de domínio de clone, como nome, endereço IP e resolvedor de DNS.

Por exemplo, para criar um controlador de domínio de clone chamado VirtualDC2 com um endereço IPv4 estático, digite:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> O controlador de domínio de clone ficará localizado no mesmo site que o controlador de domínio de origem, a menos que outro site seja especificado no arquivo DCCloneConfig.xml. É recomendável especificar um site adequado no arquivo DCCloneConfig.xml para o controlador de domínio de clone baseado em seu endereço IP.

O nome do computador é opcional. Se você não especificar um, será gerado um nome exclusivo com base no seguinte algoritmo:

-   O prefixo são os oito primeiros caracteres do nome do computador do controlador de domínio de origem. Por exemplo, o nome do computador de origem SourceComputer é truncado a uma cadeia de caracteres de prefixo SourceCo.

-   Um sufixo de nomeação exclusivo no formato "" CL*nnnn*"é acrescentado à cadeia de caracteres de prefixo em que *nnnn* é o próximo valor disponível de 0001 a 9999 que o PDC determina como não está em uso no momento. Por exemplo, se 0047 for o próximo número disponível no intervalo permitido, usando o exemplo anterior do prefixo do nome do computador SourceCo, o nome derivado a ser usado para o computador de clone será definido como SourceCo-CL0047.

> [!NOTE]
> Um servidor GC (catálogo global) é necessário para o cmdlet New-ADDCCloneConfigFile funcionar corretamente. Associação do controlador de domínio de origem do **controladores de domínio Clonáveis** grupo deve ser refletido no GC. O GC não precisa ser o mesmo controlador de domínio que o emulador do PDC, mas é melhor que ele esteja no mesmo site. Se um GC não estiver disponível, o comando falhará com o erro "o servidor não está operacional". Para obter mais informações, consulte [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Para criar um controlador de domínio de clone chamado Clone1 com configurações de IPv4 estáticas e especificar servidores WINS preferenciais e alternativos, digite:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Se você especificar servidores WINS, você deve especificar ambos **"PreferredWINSServer** e **" AlternateWINSServer**. Se você especificar apenas um desses argumentos, haverá falha na clonagem com o código de erro 0x80041005 exibido no dcpromo.log.

Para criar um controlador de domínio de clone chamado Clone2 com configurações de IPv4 dinâmicas, digite:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> Nesse caso, deve haver um servidor DHCP no ambiente que o clone possa acessar e obter o endereço IP e outras configurações de rede relevantes.

Para criar um controlador de domínio de clone chamado Clone2 com configurações de IPv4 dinâmicas e especificar servidores WINS preferenciais e alternativos, digite:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Para criar um controlador de domínio de clone com configurações de IPv6 dinâmicas, digite:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Para criar um controlador de domínio de clone com configurações de IPv6 estáticas, digite:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Ao especificar as configurações de IPv6, a única diferença entre as configurações estáticas e dinâmicas é a inclusão de **-estático** alternar. A inclusão do **-estático** switch torna obrigatório especificar pelo menos um **IPv6DNSResolver**. O endereço IPv6 estático deve ser configurado por meio de endereço sem monitoração de estado SLAAC (configuração automática) com prefixos atribuído do roteador. Com o IPv6 dinâmico, os resolvedores de DNS são opcionais, mas espera-se que o clone possa acessar um servidor DHCP habilitado para IPv6 na sub-rede para obter as informações de configuração de DNS e endereço IPv6.

#### <a name="BKMK_OfflineMode"></a>Executando New-ADDCCloneConfigFile em modo offline
Se você tem várias cópias da mídia do controlador de domínio de origem que foram preparadas para clonagem (o que significa que o controlador de domínio de origem foi autorizado para clonagem, o cmdlet Get-ADDCCloningExcludedApplicationList foi executado, etc.) e deseja especificar configurações diferentes para cada cópia da mídia, pode executar o New-ADDCCloneConfigFile em modo offline. Isso pode ser mais eficaz do que preparar individualmente cada VM, por exemplo, importando cópia por cópia.

Nesse caso, os administradores de domínio podem montar o disco offline e usar ferramentas de administração de servidor remoto (RSAT) para executar o cmdlet New-ADDCCloneConfigFile com o argumento - offline para adicionar os arquivos XML, que permite a automação de fábrica semelhante a usar novos Opções do Windows PowerShell incluídas no Windows Server 2012. Para obter mais informações sobre como montar o disco offline para executar o cmdlet New-ADDCCloneConfigFile em modo offline, consulte [Adicionando XML ao disco do sistema offline](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Execute primeiro o cmdlet localmente na mídia de origem para confirmar se as verificações dos pré-requisitos foram aprovadas. As verificações de pré-requisitos não são realizadas em modo offline porque o cmdlet pode ser executado de uma máquina que não seja do mesmo domínio ou de um computador que ingressou no domínio. Depois que você executar o cmdlet localmente, ele criará um arquivo DCCloneConfig.xml. É possível excluir o DCCloneConfig.xml criado localmente se você pretende usar o modo offline na sequência.

Para criar um controlador de domínio de clone chamado CloneDC1 em modo offline, em um site chamado REDMOND"com o endereço IPv4 estático, digite:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Para criar um controlador de domínio de clone chamado Clone2 em modo offline com configurações de IPv4 e IPv6 estáticas, digite:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Para criar um controlador de domínio de clone em modo offline com configurações de IPv4 estáticas e de IPv6 dinâmicas e especificar vários servidores DNS para as definições do resolvedor de DNS, digite:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Para criar um controlador de domínio de clone chamado Clone1 em modo offline com configurações de IPv4 dinâmicas e de IPv6 estáticas, digite:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Para criar um controlador de domínio de clone em modo offline com configurações de IPv4 e IPv6 dinâmicas, digite:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Etapa 4: exportar e importar a máquina virtual do controlador de domínio de origem
Neste procedimento, exporte a máquina virtual do controlador de domínio virtualizado de origem e depois importe-a. Essa ação cria um controlador de domínio virtualizado de clone em seu domínio.

Você precisa ser membro do grupo local de administradores em cada host Hyper-V. Se você usa credenciais diferentes para cada servidor, execute os cmdlets do Windows PowerShell para exportar e importar a VM em sessões distintas do Windows PowerShell.

Se houver instantâneos no controlador de domínio de origem, eles deverão ser excluídos antes da exportação do controlador de domínio de origem, pois a VM não será importada se um instantâneo tiver configurações de processador incompatíveis com o host hyper-v de destino. Se as configurações de processador forem compatíveis entre os hosts hyper-v de origem e destino, será possível exportar e copiar a origem sem antes ter de excluir os instantâneos. Após a importação, no entanto, os instantâneos devem ser excluídos da VM de clone antes que ela seja iniciada.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Para copiar um controlador de domínio virtual exportando e importando o controlador de domínio virtualizado de origem

1.  No **HyperV1**, desligue o controlador de domínio de origem (**VirtualDC1**).

    ![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    Stop-VM-nome VirtualDC1 - ComputerName HyperV1


2.  No **HyperV1**, exclua os instantâneos e, em seguida, exporte o controlador de domínio de origem (VirtualDC1) para o diretório c:\CloneDCs.

> [!NOTE]
> Você precisa excluir todos os instantâneos associados, pois cada vez que um instantâneo é tirado, um novo arquivo AVHD é criado agindo como um disco de diferenciação. Isso cria um efeito em cadeia. Se você tirou instantâneos e inserir o arquivo DCCLoneConfig.xml no VHD, poderá acabar criando um clone de uma versão da DIT mais antiga ou inserindo o arquivo de configuração no arquivo VHD errado. A exclusão do instantâneo mescla todos esses AVHDs em um VHD base.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copie a pasta **virtualdc1** para o diretório c:\Import do **HyperV2**.

4.  No **HyperV2**, usando o **Gerenciador do Hyper-V**, importe a máquina virtual (usando o **Assistente para Importar Máquina Virtual** no **Gerenciador do Hyper-V**) da pasta **c:\Import\virtualdc1** e exclua todos os **Instantâneos**associados.

Use a opção **Copiar a máquina virtual (criar uma nova ID exclusiva)** quando importar a máquina virtual.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Para criar vários controladores de domínio de clone do mesmo controlador de domínio de origem:

  -   Interface do usuário: no **Assistente para Importar Máquina Virtual** , especifique novos locais para a **Pasta de configuração de máquina virtual**, o **Repositório de instantâneos**, a **Pasta de Paginação Inteligente**e outro **Local** para os discos rígidos virtuais da máquina virtual.

  -   Windows PowerShell: especifique novos locais para a máquina virtual usando os seguintes parâmetros para o `Import-VM` cmdlet:

        $path = get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual máquinas" Import-VM-caminho $path.fullname - Copy - GenerateNewId - VhdDestinationPath - ComputerName HyperV2 "caminho" - SnapshotFilePath "caminho" - SmartPagingFilePath "path" - VirtualMachinePath "caminho"


> [!NOTE]
> O tamanho do lote recomendado para criação de vários controladores de domínio de clone simultaneamente é 10. O número máximo está restrito ao número máximo de conexões de replicação externas, que, por padrão, é 16 para DFSR (Replicação DFS) e 10 para FRS (Serviço de Replicação de Arquivos). Não convém implantar mais que o número recomendado de controladores de domínio de clone ao mesmo tempo, a menos que você tenha testado completamente esse número em seu ambiente.

5.  No **HyperV1**, reinicie o controlador de domínio de origem (**(VirtualDC1**) para que volte a ficar online.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  No **HyperV2**, inicie a máquina virtual (**VirtualDC2**) para ficar online como um controlador de domínio de clone no domínio.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> O emulador do PDC deve estar em execução para a clonagem ser bem-sucedida. Se ele estava desligado, verifique se foi iniciado e se foi feita a sincronização inicial para reconhecer a função de emulador do PDC. Para obter mais informações, consulte o [artigo 305476 da base de dados de conhecimento](https://support.microsoft.com/kb/305476)da Microsoft.

Após o término da clonagem, verifique o nome do computador de clone para confirmar se a operação de clonagem foi bem-sucedida. Verifique se a VM não foi iniciada em DSRM (Modo de Restauração dos Serviços de Diretório). Se você tentar fazer logon e receber um erro indicando que não há servidores de logon disponíveis, faça logon em DSRM. Se o controlador de domínio não tiver sido clonado corretamente e for inicializado em DSRM, verifique os logs no Visualizador de Eventos e os logs do dcpromo na pasta %systemroot%/debug.

O controlador de domínio clonado será membro do grupo **Controladores de Domínio Clonáveis**, pois ele copia a associação do controlador de domínio de origem. Como prática recomendada, convém deixar o grupo **Controladores de Domínio Clonáveis** vazio até você estar pronto para realizar as operações de clonagem, e é preciso remover os membros após o término dessas operações.

Se o controlador de domínio de origem armazena uma mídia de backup, o controlador de domínio clonado fará o mesmo. É possível executar `wbadmin get versions` para mostrar a mídia de backup no controlador de domínio clonado. Um membro do grupo Admins. do Domínio deve excluir a mídia de backup do controlador de domínio clonado para impedir que ela seja acidentalmente restaurada. Para obter mais informações sobre como excluir um backup de estado do sistema usando o wbadmin.exe, consulte [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Solução de problemas
Se o controlador de domínio de clone (**VirtualDC2**) for iniciado em DSRM (Modo de Restauração dos Serviços de Diretório), ele não voltará para o modo normal sozinho na próxima reinicialização. Para fazer logon em um controlador de domínio iniciado em DSRM, use **.\Administrator** e especifique a senha do DSRM.

Corrija a causa da falha na clonagem e verifique se o dcpromo.log não indica que é impossível repetir a clonagem. Se for impossível repetir a clonagem, descarte a mídia com segurança. Se for possível repetir a clonagem, remova o sinalizador de inicialização do Modo de Restauração DS para tentar clonar novamente.

1.  Abra o Windows Server 2012 com um comando com privilégios elevados (direito clique em Windows Server 2012 e escolha Executar como administrador) e digite **msconfig**.

2.  Na guia **Inicialização** , em **Opções de inicialização**, desmarque **Inicialização segura** (já selecionada com a opção **Reparo do Active Directory**habilitada).

3.  Clique em **OK** e reinicie quando solicitado.

Para obter mais informações sobre solução de problemas dos controladores de domínio virtualizados, consulte [Solução de problemas do controlador de domínio virtualizado](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


