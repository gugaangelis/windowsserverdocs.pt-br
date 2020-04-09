---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: Implantação e configuração do controlador de domínio virtualizado
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 97d726f8bfbbe664dfdfd6b7000988f009174631
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824689"
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Implantação e configuração do controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda:  
  
-   [Considerações sobre a instalação](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Inclui os requisitos de plataforma e outras restrições importantes.  
  
-   [Clonagem do controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Explica em detalhes todo o processo de clonagem do controlador de domínio virtualizado.  
  
-   [Restauração segura do controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Explica detalhadamente as validações feitas durante a restauração segura do controlador de domínio virtualizado.  
  
## <a name="installation-considerations"></a><a name="BKMK_InstallConsiderations"></a>Considerações sobre a instalação  
Não há nenhuma instalação de função ou recurso especial para controladores de domínio virtualizados; todos os controladores de domínio contêm automaticamente funcionalidades de clonagem e restauração segura. Não é possível remover ou desabilitar essas funcionalidades.  
  
O uso de controladores de domínio do Windows Server 2012 requer um Esquema AD DS do Windows Server 2012 versão 56 ou superior e nível funcional de floresta igual ao do Windows Server 2003 Native ou superior.  
  
O controlador de domínio gravável e o somente leitura dão suporte a todos os aspectos de DC virtualizado, assim como os Catálogos Globais e as funções FSMO.  
  
> [!IMPORTANT]  
> O suporte da função FSMO do Emulador PDC deve estar online quando a clonagem começar.  
  
### <a name="platform-requirements"></a><a name="BKMK_PlatformReqs"></a>Requisitos de plataforma  
A clonagem do Controlador de Domínio Virtualizado requer:  
  
-   Função FSMO de emulador PDC hospedada em um DC do Windows Server 2012  
  
-   Emulador PDC disponível durante as operações de clonagem  
  
Tanto a clonagem quanto a restauração segura requerem:  
  
-   Convidados virtualizados do Windows Server 2012  
  
-   A plataforma do host de virtualização dá suporte a VMGID (ID de Geração VM)  
  
Examine a tabela abaixo para ver os produtos de virtualização e se eles dão suporte a controladores de domínio virtualizados e ID de Geração de VM.  
  
|||  
|-|-|  
|**Produto de virtualização**|**Dá suporte a controladores de domínio virtualizados e VMGID**|  
|**Microsoft Windows Server 2012 Server com recurso Hyper-V**|Sim|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Sim|  
|**Recurso de cliente do Microsoft Windows 8 com Hyper-V**|Sim|  
|**Windows Server 2008 R2 e Windows Server 2008**|Não|  
|**Soluções de virtualização que não são da Microsoft**|Contate o fornecedor|  
  
Embora a Microsoft dê suporte a Windows 7 Virtual PC, Virtual PC 2007, Virtual PC 2004 e Virtual Server 2005, eles não podem executar convidados de 64 bits, nem dão suporte a VM-GenerationID.  
  
Para obter ajuda com produtos de virtualização de terceiros e suas posturas de suporte com controladores de domínio virtualizados, contate diretamente o fornecedor.  
  
Para obter mais informações, examine a Política de suporte do [Microsoft software running in non-Microsoft hardware virtualization software (Software Microsoft em execução em um software de virtualização de hardware não Microsoft)](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Advertências críticas  
Controladores de domínio virtualizados *não* dão suporte a restauração segura de:  
  
-   Arquivos VHD e VHDX copiados manualmente sobre arquivos VHD existentes  
  
-   Arquivos VHD e VHDX restaurados usando o backup de arquivo ou software de backup de disco completo  
  
> [!NOTE]  
> Arquivos VHDX são novos para o Windows Server 2012 Hyper-V.  
  
Nenhuma dessas operações é coberta sob a semântica VM-GenerationID e, portanto, não altera a ID de Geração de VM. A restauração de controladores de domínio usando esses métodos pode resultar em uma reversão de USN, na quarentena do controlador de domínio ou na introdução de objetos remanescentes e na necessidade de amplas operações de limpeza da floresta.  
  
> [!WARNING]  
> A restauração segura do controlador de domínio virtualizado não é uma substituição para backups de estado do sistema e a Lixeira do AD DS.  
>   
> Depois da restauração de um instantâneo, os deltas das alterações não replicadas anteriormente, originárias desse controlador de domínio depois do instantâneo, ficam perdidos de forma permanente. A restauração segura implementa a restauração não autoritativa automatizada para evitar *somente*a quarentena de controlador de domínio acidental.  
  
Para obter mais informações sobre bolhas USN e objetos remanescentes, consulte [Troubleshooting Active Directory operations that fail with error 8606: "Insufficient attributes were given to create an object" (Solução de problemas de operações do Active Directory que falham com o erro 8606: “Foram dados atributos insuficientes para criar um objeto”)](https://support.microsoft.com/kb/2028495).  
  
## <a name="virtualized-domain-controller-cloning"></a><a name="BKMK_VDCCloning"></a>Clonagem do controlador de domínio virtualizado  
Há diversos estágios e etapas para clonar um controlador de domínio virtualizado, independentemente do uso de ferramentas gráficas ou do Windows PowerShell. Em um nível alto, os três estágios são:  
  
**Preparar o ambiente**  
  
-   Etapa 1: Validar que o hipervisor dá suporte à ID de Geração de VM e portanto, à clonagem  
  
-   Etapa 2: Verifique se a função de emulador de PDC é hospedada por um controlador de domínio que executa o Windows Server 2012 e que ele está online e acessível pelo controlador de domínio clonado durante a clonagem.  
  
**Preparar o controlador de domínio de origem**  
  
-   Etapa 3: Autorizar o controlador de domínio de origem para clonagem  
  
-   Etapa 4: Remover serviços ou programas incompatíveis ou adicioná-los ao arquivo CustomDCCloneAllowList.xml.  
  
-   Etapa 5: Criar DCCloneConfig.xml  
  
-   Etapa 6: Tornar o controlador de domínio de origem offline  
  
**Criar o controlador de domínio clonado**  
  
-   Etapa7: Copiar ou exportar a VM de origem e adicionar o XML, se ainda não tiver sido copiado  
  
-   Etapa 8: Criar uma nova máquina virtual da cópia  
  
-   Etapa 9: Iniciar a nova máquina virtual para dar início à clonagem  
  
Não há diferenças de procedimentos na operação ao usar ferramentas gráficas como o Console de Gerenciamento do Hyper-V ou ferramentas de linha de comando, como o Windows PowerShell. Portanto, as etapas são apresentadas somente uma vez com ambas as interfaces. Este tópico fornece exemplos do Windows PowerShell para você explorar a automação de ponta a ponta do processo de clonagem; eles não são necessários para nenhuma etapa. Não há nenhuma ferramenta de gerenciamento gráfico para controladores de domínio virtualizados incluídos no Windows Server 2012.  
  
Há vários pontos no procedimento em que há opções de como criar o computador clonado e como adicionar os arquivos xml; essas etapas estão descritas em detalhes abaixo. Em outros aspectos, o processo é inalterável.  
  
O diagrama a seguir ilustra o processo de clonagem do controlador de domínio virtualizado, em que o domínio já existe.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Etapa 1 — Validar o hipervisor  
Verifique se o controlador de domínio de origem está em execução em um hipervisor com suporte examinando a documentação do fornecedor. Os controladores de domínio virtualizados são independentes do hipervisor e não requerem Hyper-V.  
  
Se o hipervisor estiver Microsoft Hyper-V, verifique se ele está em execução no Windows Server 2012. Você pode validar isso usando o Gerenciamento de Dispositivo  
  
Abra o **Devmgmt.msc** e examine se há dispositivos e drivers Microsoft Hyper-V nos **Dispositivos do Sistema** . O dispositivo do sistema especifico necessário para um controlador de domínio virtualizado é o **Microsoft Hyper-V Generation Counter** (driver: vmgencounter.sys).  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Etapa 2 — Verificar a função PDCE FSMO  
Antes de tentar clonar um DC, valide que o controlador de domínio que hospeda o Emulador de Controlador de Domínio Primário FSMO executa o Windows Server 2012. O PDCE (emulador PDC) é necessário por vários motivos:  
  
1.  O PDCE cria o grupo especial **Controladores de Domínio Clonáveis** e define sua permissão na raiz do domínio para permitir que um controlador de domínio clone a si mesmo.  
  
2.  O controlador de domínio de clonagem contata o PDCE diretamente usando o protocolo DRSUAPI RPC, a fim de criar objetos de computador para o DC do clone.  
  
    > [!NOTE]  
    > O Windows Server 2012 estende o Protocolo Remoto do DRS (Serviço de Replicação de Diretório) existente (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) para incluir um novo método RPC **IDL_DRSAddCloneDC** (Opnum **28**). O método **IDL_DRSAddCloneDC** cria um novo objeto de controlador de domínio copiando atributos de um objeto de controlador de domínio existente.  
    >   
    > Os estados de um controlador de domínio são compostos de computador, servidor, configurações de NTDS, FRS, DFSR e objetos de conexão mantidos para cada controlador de domínio. Ao duplicar um objeto, esse método RPC substitui todas as referências ao controlador de domínio original por objetos correspondentes do novo controlador de domínio. O chamador deve ter o direito de acesso de controle DS-Clone-Domain-Controller no contexto de nomenclatura do domínio.  
    >   
    > O uso desse novo método sempre requer que o chamador tenha acesso direto ao controlador de domínio do emulador PDC.  
    >   
    > Como esse método RPC é novo, seu software de análise de rede requer analisadores atualizados para incluir campos do novo Opnum 28 no UUID E3514235-4B06-11D1-AB04-00C04FC2DCD2 existente. Caso contrário, não será possível analisar esse tráfego.  
    >   
    > Para obter mais informações, consulte [4.1.29 IDL_DRSAddCloneDC (Opnum 28)](https://msdn.microsoft.com/library/hh554213(v=prot.13).aspx).  
  
***Isso também significa que, ao usar redes não totalmente roteadas, a clonagem do controlador de domínio virtualizado requer segmentos de rede com acesso ao PDCE***. É aceitável mover um controlador de domínio clonado para uma rede diferente após a clonagem — da mesma forma que um controlador de domínio físico — desde que você tenha o cuidado de atualizar as informações de site lógico do AD DS.  
  
> [!IMPORTANT]  
> Ao clonar um domínio que contém somente um único controlador de domínio, verifique se o DC de origem está novamente online antes de iniciar as cópias do clone. Um domínio de produção deve sempre conter no mínimo dois controladores de domínio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Usuários do Active Directory e método de computadores  
  
1.  Usando o snap-in Dsa.msc, clique com o botão direito do mouse no domínio e clique em **Mestres de Operações**. Observe o controlador de domínio nomeado na guia PDC e feche o diálogo.  
  
2.  Clique com o botão direito do mouse no objeto de computador do DC e clique em **Propriedades**. Valide as informações do Sistema Operacional.  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode combinar os seguintes cmdlets do Módulo Windows PowerShell do Active Directory para retornar a versão do emulador PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Se o domínio não for fornecido, esses cmdlets presumem o domínio do computador em que estão sendo executados.  
  
O seguinte comando retorna o PDCE e as informações do Sistema Operacional:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
O exemplo abaixo demonstra como especificar o nome de domínio e filtrar as propriedades retornadas antes do pipeline do Windows PowerShell:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Etapa 3 — Autorizar um DC de origem  
O controlador de domínio de origem deve ter o CAR (direito de acesso de controle) **Permitir que um controlador de domínio crie um clone dele mesmo** no cabeçalho NC do domínio. Por padrão, o grupo bem conhecido **Controlador de Domínio Clonáveis** tem essa permissão e não contém membros. O PDCE cria esse grupo quando essa função FSMO é transferida para um controlador de domínio do Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Método do Centro Administrativo do Active Directory  
  
1.  Inicie Dsac.exe, navegue até o DC de origem e abra sua página de detalhes.  
  
2.  Na seção **Membro de** , adicione o grupo **Controladores de Domínio Clonáveis** para esse domínio.  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode combinar os seguintes cmdlets do Módulo Windows PowerShell do Active Directory, **get-adcomputer** e **add-adgroupmember**, para adicionar um controlador de domínio ao grupo **Controladores de Domínio Clonáveis**:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Por exemplo, isso adiciona o servidor DC1 ao grupo, sem a necessidade de especificar um nome diferenciado do membro do grupo:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Recompilando permissões padrão  
Se você remover essa permissão do cabeçalho do domínio, a clonagem falhará. Você pode recriar a permissão usando o Centro Administrativo do Active Directory ou o Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Método do Centro Administrativo do Active Directory  
  
1.  Abra o **Centro Administrativo do Active Directory**, clique com o botão direito do mouse no cabeçalho do domínio, clique em **Propriedades**, clique na guia **Extensões** , clique em **Segurança**e em **Avançado**. Clique em **Apenas Este Objeto**.  
  
2.  Clique em **Adicionar**, sob **Digite o nome do objeto a ser selecionado**, digite o nome do grupo **Controladores de Domínio Clonáveis.**  
  
3.  Em Permissões, clique em **Permitir que um controlador de domínio crie um clone dele mesmo**, e clique em **OK**.  
  
> [!NOTE]  
> Você também pode remover a permissão padrão e adicionar controladores de domínio individuais. Entretanto, é provável que fazer isso cause problemas de manutenção contínuos, em que novos administradores não reconhecem essa personalização. A alteração das configurações padrão não aumenta a segurança e isso não é recomendado.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Use os comandos a seguir em um prompt de console do Windows PowerShell com permissões de administrador. Estes comandos detectam o nome de domínio e adicionam novamente as permissões padrão:  
  
```  
import-module activedirectory  
cd ad:  
$domainNC = get-addomain  
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
$sid1 = (get-adgroup $dcgroup).sid  
$acl = get-acl $domainNC  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
cd c:  
```  
  
Você também pode executar a amostra [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) em um console do Windows PowerShell, em que o console é iniciado como um administrador elevado em um controlador de domínio no domínio afetado. Isso faz com que as permissões sejam definidas automaticamente. A amostra está localizada no apêndice deste módulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Etapa 4 — Remover aplicativos ou serviços incompatíveis (se não usar CustomDCCloneAllowList.xml)  
Quaisquer programas ou serviços retornados anteriormente pelo Get-ADDCCloningExcludedApplicationList — *e não adicionados ao CustomDCCloneAllowList.xml* — devem ser removidos antes da clonagem. Desinstalar o aplicativo ou serviço é o método recomendado.  
  
> [!WARNING]  
> Todo programa ou serviço incompatível não desinstalado ou adicionado ao CustomDCCloneAllowList.xml impede a clonagem.  
  
Use o cmdlet Get-AdComputerServiceAccount para localizar quaisquer MSAs (Contas de Serviços Gerenciados) independentes no domínio e se esse computador estiver usando qualquer uma delas. Se alguma MSA estiver instalada, use o cmdlet Uninstall-ADServiceAccount para remover a conta de serviço instalada localmente. Depois de ter tornado o controlador de domínio de origem offline na etapa 6, já poderá adicionar novamente a MSA usando Install-ADServiceAccount quando o servidor voltar a ficar online. Para obter mais informações, consulte [Uninstall-ADServiceAccount](https://technet.microsoft.com/library/hh852310).  
  
> [!IMPORTANT]  
> MSAs independentes — liberadas primeiramente no Windows Server 2008 R2 — foram substituídas no Windows Server 2012 pelas MSAs de grupo. MSAs de grupo dão suporte a clonagem.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Etapa 5 — Criar DCCloneConfig.xml  
O arquivo DcCloneConfig.xml é necessário para clonagem de controladores de domínio. Seu conteúdo permite que você especifique detalhes exclusivos, como o novo nome do computador e o endereço IP.  
  
O arquivo CustomDCCloneAllowList.xml é opcional, a menos que você instale aplicativos ou serviços Windows potencialmente incompatíveis no controlador de domínio de origem. Os arquivos requerem nomenclatura, formatação e colocação precisas; caso contrário, a clonagem falhará.  
  
Por esse motivo, sempre use cmdlets do Windows PowerShell para criar os arquivos XML e colocá-los no local correto.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Gerando com New-ADDCCloneConfigFile  
O módulo Windows PowerShell do Active Directory contém um novo cmdlet no Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Execute o cmdlet no controlador de domínio de origem proposto que você pretende clonar. O cmdlet dá suporte a vários argumentos e, quando usado, sempre testa o computador e o ambiente em que é executado, a menos que você especifique o argumento -offline.  
  
||||  
|-|-|-|  
|**Active**<p>**Cmdlet**|**Argumentos**|**Explica**|  
|**New-ADDCCloneConfigFile**|*<no argument specified>*|Cria um arquivo DcCloneConfig.xml em branco no Diretório de Trabalho DSA (padrão: %systemroot%\ntds)|  
||-CloneComputerName|Especifica o nome do computador do DC do clone. Tipo de dados String.|  
||-Path|Especifica a pasta para criar DcCloneConfig.xml. Se não especificado, grava no Diretório de Trabalho DSA (padrão: %systemroot%\ntds). Tipo de dados String.|  
||-SiteName|Especifica o nome do site lógico do AD a se ingressar durante a criação da conta do computador clonado. Tipo de dados String.|  
||-IPv4Address|Especifica o endereço IPv4 estático do computador clonado. Tipo de dados String.|  
||-IPv4SubnetMask|Especifica a máscara de sub-rede do IPv4 estática do computador clonado. Tipo de dados String.|  
||-IPv4DefaultGateway|Especifica o endereço de gateway padrão do IPv4 estático do computador clonado. Tipo de dados String.|  
||-IPv4DNSResolver|Especifica as entradas DNS do IPv4 estático do computador clonado em uma lista separada por vírgulas. Tipo de dados Array. Até quatro entradas podem ser fornecidas.|  
||-PreferredWINSServer|Especifica o endereço IPv4 estático do servidor WINS primário. Tipo de dados String.|  
||-AlternateWINSServer|Especifica o endereço IPv4 estático do servidor WINS secundário. Tipo de dados String.|  
||-IPv6DNSResolver|Especifica as entradas DNS do IPv6 estático do computador clonado em uma lista separada por vírgulas. Não há maneiras de configurar informações estáticas do Ipv6 em uma clonagem do controlador de domínio virtualizado. Tipo de dados Array.|  
||-Offline|Não executa os testes de validação e substitui qualquer dccloneconfig.xml existente. Não possui parâmetros.|  
||*-Estático*|Necessário se especificar argumentos IP estáticos IPv4SubnetMask, IPv4SubnetMask ou IPv4DefaultGateway. Não possui parâmetros.|  
  
Testes feitos ao se executar no modo online:  
  
-   Emulador PDC é Windows Server 2012 ou posterior  
  
-   O controlador de domínio de origem é um membro do grupo Controladores de Domínio Clonáveis  
  
-   O controlador de domínio de origem não inclui quaisquer aplicativos ou serviços excluídos  
  
-   O controlador de domínio de origem ainda não contém um DcCloneConfig.xml no caminho especificado  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Etapa 6 — Tornar o controlador de domínio de origem offline  
Você não pode copiar um DC de origem em execução; ele deve ser desligado normalmente. Não clone um controlador de domínio interrompido forçosamente por falta de energia.  
  
#### <a name="graphical-method"></a>Método gráfico  
Use o botão de desligamento no DC em execução, ou o botão de desligamento do Gerenciador Hyper-V.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode desligar uma máquina virtual usando um dos seguintes cmdlets:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Stop-computer é um cmdlet que dá suporte a desligamento de computadores independente de virtualização, e é análogo ao utilitário Shutdown.exe herdado. Stop-vm é um novo cmdlet no módulo Windows PowerShell do Windows Server 2012 Hyper-V, e é equivalente às opções de energia no Gerenciador Hyper-V. Este é útil em ambientes de laboratório em que o controlador de domínio geralmente opera em uma rede virtualizada privada.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Etapa 7 — Copiar discos  
Uma opção administrativa é necessária na fase de cópia:  
  
-   Copiar os discos manualmente, sem Hyper-V  
  
-   Exportar a VM, usando Hyper-V  
  
-   Exportar os discos mesclados, usando Hyper-V  
  
Todos os discos de uma máquina virtual devem ser copiados, não apenas a unidade do sistema. Se o controlador de domínio de origem usa discos diferenciais e você pretende mover seu controlador de domínio clonado para outro host Hyper-V, exporte.  
  
É recomendado copiar os discos manualmente se o controlador de domínio de origem tiver somente *uma* unidade. Exportar/importar é recomendado para VMs com *mais de uma* unidade ou outras personalizações de hardware virtualizado complexas como várias NICs.  
  
Se for copiar arquivos manualmente, exclua quaisquer instantâneos antes de copiá-los. Se for exportar a VM, exclua os instantâneos antes de exportar ou exclua-os da nova VM após a importação.  
  
> [!WARNING]  
> Os instantâneos são discos diferenciais que podem retornar um controlador de domínio para o estado anterior. Se clonasse um controlador de domínio e restaurasse seu instantâneo pré-clonado, você teria controladores de domínio duplicados na floresta. Não há valor nenhum em instantâneos anteriores em um controlador de domínio recentemente clonado.  
  
#### <a name="manually-copying-disks"></a>Copiando discos manualmente  
  
##### <a name="hyper-v-manager-method"></a>Método do Gerenciador Hyper-V  
Usar o snap-in do Gerenciador Hyper-V para determinar quais discos estão associados ao controlador de domínio de origem. Use a opção Inspecionar para validar se o controlador de domínio usa discos diferenciais (o que requer também copiar o disco pai)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Para excluir os instantâneos, selecione uma VM e exclua a subárvore do instantâneo.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
Você pode então copiar manualmente os arquivos VHD ou VHDX usando Windows Explorer, Xcopy.exe ou Robocopy.exe. Nenhuma etapa especial é necessária. A melhor prática é alterar os nomes de arquivo mesmo se movê-los para outra pasta.  
  
> [!NOTE]  
> Se copiar entre computadores host em uma LAN (de 1 Gbit ou superior), a opção **Xcopy.exe /J** copia arquivos VHD/VHDX de forma consideravelmente mais rápida do que qualquer outra ferramenta, ao custo de um uso muito maior da largura de banda.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para determinar os discos que usam o Windows PowerShell, use os Módulos do Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Por exemplo, você pode retornar todas as unidades de disco rígido IDE de uma VM denominada **DC2** com a seguinte amostra:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Se o caminho do disco aponta para um arquivo AVHD ou AVHDX, ele é um instantâneo. Para excluir os instantâneos associados a um disco e mesclar no VHD ou VHDX real, use cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por exemplo, para excluir todos os instantâneos de uma VM denominada DC2-SOURCECLONE:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Para copiar os arquivos usando o Windows PowerShell, use o seguinte cmdlet:  
  
```  
Copy-Item  
```  
  
Combine-o com cmdlets de VM em pipelines para ajudar a automação. O pipeline é um canal usado entre vários cmdlets para passar dados. Por exemplo, para copiar a unidade de um controlador de domínio de origem offline denominada DC2-SOURCECLONE para um novo disco chamado c:\temp\copy.vhd sem precisar saber o caminho exato a sua unidade do sistema:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> Você não pode usar discos de passagem com clonagem, pois eles não usam um arquivo de disco virtual e sim um disco rígido real.  
  
> [!NOTE]  
> Para obter outras informações sobre mais operações do Windows PowerShell com pipelines, consulte [Canalização e pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportando a VM  
Como uma alternativa para copiar os discos, você pode exportar toda a VM do Hyper-V como uma cópia. A exportação automática cria uma pasta denominada para a VM, contendo todos os discos e informações de configuração.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Método do Gerenciador Hyper-V  
Para exportar uma VM com o Gerenciador Hyper-V:  
  
1.  Clique com o botão direito do mouse no controlador de domínio de origem e clique em **Exportar**.  
  
2.  Selecione uma pasta existente como o contêiner de exportação.  
  
3.  Aguarde que a coluna Status pare de mostrar **Exportando**.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para exportar uma VM usando o módulo Windows PowerShell do Hyper-V, use o cmdlet:  
  
```  
Export-vm  
```  
  
Por exemplo, para exportar uma VM denominada DC2-SOURCECLONE para uma pasta denominada C:\VM:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> O Windows Server 2012 Hyper-V dá suporte a novas funcionalidades de exportação e importação que estão fora do escopo deste treinamento. Examine a TechNet para obter mais informações.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportando discos mesclados usando Hyper-V  
A opção final é usar a mesclagem de disco e as opções de conversão dentro do Hyper-V. Essas opções permitem que você faça uma cópia de uma estrutura de disco existente — mesmo ao incluir arquivos AVHD/AVHDX de instantâneos — em um único disco novo. Assim como o cenário de cópia manual de disco, isso destina-se principalmente a máquinas virtuais mais simples que usam apenas uma única unidade, como C:\\. Sua única vantagem é que, diferente da cópia manual, ela não requer que você primeiro exclua os instantâneos. Essa operação é necessariamente mais lenta do que simplesmente excluir os instantâneos e copiar os discos.  
  
##### <a name="hyper-v-manager-method"></a>Método do Gerenciador Hyper-V  
Para criar um disco mesclado usando o Gerenciador Hyper-V:  
  
1.  Clique em **Editar disco**.  
  
2.  Procure o menor disco filho. Por exemplo, se estiver usando um disco diferencial, o disco filho é o menor filho. Se a máquina virtual tiver um instantâneo (ou vários), o instantâneo selecionado atualmente será o menor disco filho.  
  
3.  Selecione a opção **Mesclar** para criar um único disco da inteira estrutura pai-filho.  
  
4.  Selecione um novo disco rígido virtual e forneça um caminho. Isso reconcilia os arquivos VHD/VHDX existentes em uma única unidade portátil nova que não está em risco de restaurar instantâneos anteriores.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para criar um disco mesclado de um conjunto complexo de pais usando o módulo Windows PowerShell do Hyper-V, use o cmdlet:  
  
```  
Convert-vm  
```  
  
Por exemplo, para exportar a cadeia inteira de instantâneos de disco de uma VM (dessa vez não incluindo nenhum disco diferencial) e o disco pai em um único disco novo denominado DC4-CLONED.VHDX:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="adding-xml-to-the-offline-system-disk"></a><a name="BKMK_Offline"></a>Adicionando XML ao disco do sistema offline  
Se você copiou o Dccloneconfig.xml para o DC de origem em execução, agora copie o arquivo dccloneconfig.xml atualizado no disco do sistema copiado/exportado offline. Dependendo dos aplicativos instalados detectados com Get-ADDCCloningExcludedApplicationList anteriormente, você pode ter que copiar o arquivo CustomDCCloneAllowList.xml no disco.  
  
Os locais a seguir podem conter o arquivo DcCloneConfig.xml:  
  
1.  Diretório de Trabalho DSA  
  
2.  %windir%\NTDS  
  
3.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade  
  
Esses caminhos não são configuráveis. Depois do início da clonagem, ela verificará esses locais na ordem específica e utilizará o primeiro arquivo DcCloneConfig.xml encontrado, independente do conteúdo da outra pasta.  
  
Os locais a seguir podem conter o arquivo CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Diretório de Trabalho DSA  
  
3.  %windir%\NTDS  
  
4.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade  
  
Você pode executar New-ADDCCloneConfigFile com o argumento **-offline** (também conhecido como modo offline) para criar o arquivo DcCloneConfig.xml e colocá-lo em um local correto. Os exemplos a seguir mostram como executar New-ADDCCloneConfigFile no modo offline.  
  
Para criar um controlador de domínio de clone chamado CloneDC1 no modo offline, em um site chamado "REDMOND" com endereço IPv4 estático, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio de clone chamado Clone2 em modo offline com configurações de IPv4 e IPv6 estáticas, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio de clone em modo offline com configurações de IPv4 estáticas e de IPv6 dinâmicas e especificar vários servidores DNS para as definições do resolvedor de DNS, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Para criar um controlador de domínio de clone chamado Clone1 em modo offline com configurações de IPv4 dinâmicas e de IPv6 estáticas, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio de clone em modo offline com configurações de IPv4 e IPv6 dinâmicas, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Método do Windows Explorer  
O Windows Server 2012 agora oferece uma opção gráfica para montar arquivos VHD e VHDX. Isso requer a instalação do recurso Experiência Desktop no Windows Server 2012.  
  
1.  Clique no arquivo VHD/VHDX copiado recentemente que contém a unidade do sistema do DC de origem ou a pasta de localização do Diretório de Trabalho DSA, e depois clique em **Montar** no menu **Ferramentas de Imagem de Disco**.  
  
2.  Na unidade montada, copie os arquivos XML para um local válido. Pode-se solicitar permissões para a pasta.  
  
3.  Clique na unidade montada e clique em **Ejetar** no menu **Ferramentas de Disco**.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você também pode montar o disco offline e copiar o arquivo XML usando os cmdlets do Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Isso permite que você tenha controle completo sobre o processo. Por exemplo, a unidade pode ser montada com uma letra da unidade específica, o arquivo copiado e a unidade desmontada.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Por exemplo:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
Você também pode usar o novo cmdlet **Mount-DiskImage** para montar um arquivo VHD (ou ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Etapa 8 — Criar a nova máquina virtual  
A etapa de configuração final antes do início do processo de clonagem é criar uma nova VM que utilize os discos do controlador de domínio de origem copiado. Dependendo da seleção feita na fase de cópia de discos, você tem duas opções:  
  
1.  Associar uma nova VM ao disco copiado  
  
2.  Importar a VM exportada  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Associando uma nova VM com discos copiados  
Se você copiou o disco do sistema manualmente, crie uma nova máquina virtual usando o disco copiado. O hipervisor configura automaticamente a ID de Geração de VM quando uma nova VM é criada; nenhuma alteração na configuração é necessária no host VM ou Hyper-V.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Método do Gerenciador Hyper-V  
  
1.  Crie uma nova máquina virtual.  
  
2.  Especifique o nome, a memória e a rede da VM.  
  
3.  Na página Conectar Disco Rígido Virtual, especifique o disco do sistema copiado.  
  
4.  Conclua o assistente de criação de VM.  
  
Se houver vários discos, adaptadores de rede ou outras personalizações, configure-as antes de iniciar o controlador de domínio. O método "Exportar-Importar" de cópia de discos é recomendado para VMs complexas.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode usar o módulo Windows PowerShell do Hyper-V para automatizar a criação de VMs no Windows Server 2012, usando o seguinte cmdlet:  
  
```  
New-VM  
```  
  
Por exemplo, aqui a VM DC4-CLONEDFROMDC2 é criada usando 1GB de RAM, com inicialização do arquivo c:\vm\dc4-systemdrive-clonedfromdc2.vhd e uso da rede virtual 10.0:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importar VM  
Se você exportou anteriormente sua VM, agora precisa importá-la de volta como uma cópia. Isso usa o XML exportado para recriar o computador usando todas as configurações, unidades, redes e configurações de memória anteriores.  
  
Se você pretende criar cópias adicionais a partir da mesma VM exportada, faça quantas cópias da VM exportada forem necessárias. Depois, use Importar para cada cópia.  
  
> [!IMPORTANT]  
> É importante usar a opção **Copiar** , pois a exportação preserva todas as informações da origem; importar o servidor com **Mover** ou **In loco** causa colisão de informações se feito no mesmo servidor host do Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Método do Gerenciador Hyper-V  
Para importar usando o snap-in do Gerenciador Hyper-V:  
  
1.  Clique em **Importar Máquina Virtual**  
  
2.  Na página **Localizar Pasta**, selecione o arquivo de definição da VM exportada usando o botão Procurar  
  
3.  Na página **Selecionar Máquina Virtual**, clique no computador de origem.  
  
4.  Na página **Escolher Tipo de Importação** , clique em **Copiar a máquina virtual (criar uma nova ID exclusiva)** , e clique em **Concluir**.  
  
5.  Renomeie a VM importada se estiver importando no mesmo host Hyper-V, ela terá o mesmo nome que o controlador de domínio de origem exportado.  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Lembre-se de remover quaisquer instantâneos importados usando o snap-in de Gerenciamento do Hyper-V:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> Excluir quaisquer instantâneos importados é fundamental; se aplicável, eles poderiam retornar o controlador de domínio clonado para o estado de um DC anterior — e possivelmente ativo —, resultando em falha de replicação, informações de IP duplicadas e interrupções.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode usar o módulo Windows PowerShell do Hyper-V para automatizar a importação de VMs no Windows Server 2012, usando os seguintes cmdlets:  
  
```  
Import-VM  
Rename-VM  
```  
  
Por exemplo, aqui a VM exportada DC2-CLONED é importada usando seu arquivo XML determinado automaticamente, depois é imediatamente renomeada para seu novo nome de VM DC5-CLONEDFROMDC2:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Lembre-se de remover quaisquer instantâneos importados usando os seguintes cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por exemplo:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]
> Certifique-se de que, ao importar o computador, os endereços MAC estáticos não foram designados para o controlador de domínio de origem. Se um computador de origem com um MAC estático for clonado, aqueles computadores copiados não enviarão ou receberão corretamente qualquer tráfego de rede. Defina um novo endereço MAC estático ou dinâmico exclusivo, se esse for o caso. Você pode ver se uma VM usa endereços MAC estáticos com o comando:  
> 
> **Get-VM-VMName**   
>  ***Test-VM* | Get-VMNetworkAdapter | FL \\** *  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Etapa 9 — Clonar a nova máquina virtual  
Uma opção antes de iniciar a clonagem é reiniciar o controlador de domínio de origem do clone offline. Independente de qualquer coisa. verifique se o emulador PDC está online.  
  
Para começar a clonagem, simplesmente inicie a nova máquina virtual. O processo é iniciado automaticamente e o controlador de domínio é reinicializado automaticamente após a conclusão da clonagem.  
  
> [!IMPORTANT]  
> Manter os controladores de domínio desligados por um longo período de tempo não é recomendado e se o clone for ingressar no mesmo site que seu DC de origem, a topologia de replicação intra e entre sites poderá levar mais tempo para ser compilada caso o controlador de domínio de origem esteja offline.  
  
Se usar o Windows PowerShell para iniciar uma VM, o cmdlet do novo Módulo do Hyper-V será:  
  
```  
Start-VM  
```  
  
Por exemplo:  
  
![Implantação de DC virtualizado](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Assim que o computador tiver sido reiniciado após a conclusão da clonagem, ele será um controlador de domínio e você poderá fazer logon normalmente para confirmar a operação normal. Se houver erros, o servidor será definido para iniciar no Modo de Restauração dos Serviços de Diretório para investigação.  
  
## <a name="virtualization-safeguards"></a><a name="BKMK_VDCSafeRestore"></a>Proteções de virtualização  
Diferente da clonagem do controlador de domínio virtualizado, as garantias de virtualização do Windows Server 2012 não têm etapas de configuração. O recurso funciona sem intervenção desde que você atenda a algumas condições simples:  
  
-   O hipervisor dá suporte a ID de Geração de VM  
  
-   Há um controlador de domínio de parceiro válido a partir do qual um controlador de domínio restaurado pode replicar alterações de maneira não autoritativa.  
  
### <a name="validate-the-hypervisor"></a>Validar o hipervisor  
Verifique se o controlador de domínio de origem está em execução em um hipervisor com suporte examinando a documentação do fornecedor. Os controladores de domínio virtualizados são independentes do hipervisor e não requerem Hyper-V.  
  
Examine a seção anterior [Requisitos de plataforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) para obter o suporte conhecido de ID de Geração de VM.  
  
Se estiver migrando VMs de um hipervisor de origem para um hipervisor de destino diferente, as garantias de virtualização poderão ou não ser disparadas, dependendo da ID de Geração de VM do suporte de hipervisor, conforme explicado na tabela a seguir.  
  
|Hipervisor de origem|Hipervisor de destino|Resultado|  
|---------------------|---------------------|----------|  
|Dá suporte a ID de Geração de VM|Não dá suporte a ID de Geração de VM|Garantias não disparadas (se um DCCloneConfigFile.xml estiver presente, o DC será inicializado no DSRM)|  
|Não dá suporte a ID de Geração de VM|Dá suporte a ID de Geração de VM|Garantias disparadas|  
|Dá suporte a ID de Geração de VM|Dá suporte a ID de Geração de VM|Garantias não disparadas porque a definição da VM não foi alterada, o que significa, portanto, que a ID de Geração de VM permanece a mesma|  
  
### <a name="validate-the-replication-topology"></a>Validar a topologia de replicação  
As garantias de virtualização iniciam a replicação de entrada não autoritativa para o delta da replicação do Active Directory, assim como a ressincronização não autoritativa de todo conteúdo do SYSVOL. Isso assegura que o controlador de domínio retorne de um instantâneo com total funcionalidade e que seja finalmente consistente com o restante do ambiente.  
  
Com essa nova funcionalidade, vêm diversos requisitos e limitações:  
  
-   Um controlador de domínio restaurado deve ser capaz de contatar um DC gravável  
  
-   Todos os controladores de domínio em um domínio não devem ser restaurados simultaneamente  
  
-   Quaisquer alterações originárias de um controlador de domínio restaurado que ainda não tenham passado por replicação de saída desde que o instantâneo foi tirado são perdidas para sempre  
  
Embora a seção de resolução de problemas aborde esses cenários, os detalhes abaixo asseguram que você não crie uma topologia que possa causar problemas.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilidade do controlador de domínio gravável  
Se restaurado, um controlador de domínio deve ter conectividade com um controlador de domínio gravável; um controlador de domínio somente leitura não pode enviar o delta de atualizações. A topologia provavelmente já está correta, pois um controlador de domínio gravável sempre precisou de um parceiro gravável. No entanto, se todos os controladores de domínio graváveis forem restaurados simultaneamente, nenhum deles poderá encontrar uma origem válida. O mesmo acontece se os controladores de domínio graváveis estiverem offline para manutenção ou inacessíveis de outra forma pela rede.  
  
#### <a name="simultaneous-restore"></a>Restauração simultânea  
Não restaure todos os controladores de domínio em um único domínio simultaneamente. Se todos os instantâneos forem restaurados de uma vez, a replicação do Active Directory funcionará normalmente, mas a replicação de SYSVOL ficará parada. A arquitetura de restauração de FRS e DFSR requer configuração de sua instância de réplica para o modo de sincronização não autoritativa. Se todos os controladores de domínio foram restaurados de uma vez, e cada controlador de domínio marcar a si próprio como não autoritativo para SYSVOL, todos eles tentarão sincronizar scripts e políticas de grupo com base em um parceiro autoritativo; nesse ponto, entretanto, todos os parceiros também serão não autoritativos.  
  
> [!IMPORTANT]  
> Se todos os controladores de domínio forem restaurados de uma vez, use os artigos a seguir para definir um controlador de domínio — normalmente o emulador PDC — como autoritativo, de forma que os outros controladores de domínio possam retornar para a operação normal:  
>   
> [Usando a chave do registro BurFlags para reinicializar os conjuntos de réplicas do serviço de replicação de arquivo](https://support.microsoft.com/kb/290762)  
>   
> [Como forçar uma sincronização autoritativa e não autoritativa para o SYSVOL replicado pelo DFSR (como "D4/D2" para o FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> Não execute todos os controladores de domínio em uma floresta ou domínio no mesmo host de hipervisor. Isso introduz um único ponto de falha que causa danos no AD DS, no Exchange, no SQL e em outras operações corporativas cada vez que o hipervisor fica offline. Isso não é diferente de usar somente um controlador de domínio para uma floresta ou um domínio inteiro. Vários controladores de domínio em várias plataformas fornecem redundância e tolerância a falhas.  
  
#### <a name="post-snapshot-replication"></a>Replicação pós-instantâneo  
Não restaure instantâneos até que todas as alterações originadoras locais feitas desde a criação do instantâneo tenham passado pela replicação de saída. Quaisquer alterações originadoras serão perdidas para sempre se outros controladores de domínio ainda não as tiverem recebido por meio de replicação.  
  
Use Repadmin.exe para mostrar as alterações de saída não replicadas entre um controlador de domínio e seus parceiros:  
  
1.  Retorne os nomes do parceiro do DC e GUID de Objeto DSA com:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Retorne a replicação de entrada pendente do controlador de domínio parceiro para o controlador de domínio a ser restaurado:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Outra alternativa é apenas ver a contagem de alterações não replicadas:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Por exemplo (com saída modificada para legibilidade e entradas importantes ***em itálico***), aqui você vê as parcerias de replicação do DC4:  
  
```  
C:\>repadmin.exe /showrepl dc4.corp.contoso.com /repsto  
  
Default-First-Site-Name\DC4  
DSA Options: IS_GC  
Site Options: (none)  
DSA object GUID: 5d083398-4bd3-48a4-a80d-fb2ebafb984f  
DSA invocationID: 730fafec-b6d4-4911-88f2-5b64e48fc2f1  
  
==== OUTBOUND NEIGHBORS FOR CHANGE NOTIFICATIONS ============  
  
DC=corp,DC=contoso,DC=com  
    Default-First-Site-Name\DC3 via RPC  
        DSA object GUID: f62978a8-fcf7-40b5-ac00-40aa9c4f5ad3  
        Last attempt @ 2011-11-11 15:04:12 was successful.  
    Default-First-Site-Name\DC2 via RPC  
        DSA object GUID: 3019137e-d223-4b62-baaa-e241a0c46a11  
        Last attempt @ 2011-11-11 15:04:15 was successful.  
```  
  
Agora você sabe que isso está replicando com DC2 e DC3. Você então mostra a lista de alterações que o DC2 afirma ainda não ter de DC4, e observa que há um novo grupo:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com  
  
==== SOURCE DSA: (null) ====  
Objects returned: 1  
(0) add CN=newgroup4,CN=Users,DC=corp,DC=contoso,DC=com  
    1> parentGUID: 55fc995a-04f4-4774-b076-d6a48ac1af99  
    1> objectGUID: 96b848a2-df1d-433c-a645-956cfbf44086  
    2> objectClass: top; group  
    1> instanceType: 0x4 = ( WRITE )  
    1> whenCreated: 11/11/2011 3:03:57 PM Eastern Standard Time  
```  
  
Você também deve testar o outro parceiro para assegurar que ele ainda não tenha sido replicado.  
  
Você também pode, caso não tenha importado com quais objetos não foram replicados e apenas se interessou por objetos pendentes, usar a opção **/statistics**:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Teste todos os parceiros graváveis se observar alguma falha ou replicação pendente. Contanto que pelo menos um esteja convergido, geralmente é seguro restaurar o instantâneo, já que a replicação transitiva por fim reconcilia os outros servidores.  
>   
> Certifique-se de observar se há erros na replicação mostrada por /showchanges e não continue até que sejam corrigidos.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlets de Instantâneo do Windows PowerShell  
Os cmdlets do módulo Windows PowerShell Hyper-V a seguir fornecem funcionalidade de instantâneo no Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


