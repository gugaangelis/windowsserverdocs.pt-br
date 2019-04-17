---
ms.assetid: b146f47e-3081-4c8e-bf68-d0f993564db2
title: "Implantação de controlador de domínio virtualizado e configuração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dcb377cef003458bcdf2e4b3564167cee4310020
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-deployment-and-configuration"></a>Implantação de controlador de domínio virtualizado e configuração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda:  
  
-   [Considerações sobre a instalação](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_InstallConsiderations)  
  
    Isso inclui os requisitos de plataforma e outras restrições importantes.  
  
-   [Virtualizados domínio clonagem do controlador](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCCloning)  
  
    Isso explica detalhadamente o controlador de domínio de virtualizado todo processo de clonagem.  
  
-   [Restauração segura de controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_VDCSafeRestore)  
  
    Isso explica detalhadamente as validações que são feitas durante a restauração de segurança de controlador de domínio virtualizado.  
  
## <a name="BKMK_InstallConsiderations"></a>Considerações sobre a instalação  
Há um papel especial ou a instalação do recurso virtualizado para controladores de domínio; todos os controladores de domínio automaticamente contêm recursos de restauração clonagem e seguro. Você não pode remover ou desativar esses recursos.  
  
Uso de controladores de domínio do Windows Server 2012 exige uma AD DS esquema versão 56 ou superior do Windows Server 2012 e o nível funcional da floresta igual ao Windows Server 2003 nativa ou superior.  
  
Ambos os controladores de domínio gravável e somente leitura oferecem suporte ao todos os aspectos de DC virtualizado, como as funções de catálogos globais e FSMO.  
  
> [!IMPORTANT]  
> O proprietário da função de FSMO de emulador do PDC deve estar online quando clonagem começa.  
  
### <a name="BKMK_PlatformReqs"></a>Requisitos de plataforma  
Clonagem de controlador de domínio virtualizado requer:  
  
-   Função de FSMO do emulador do PDC hospedada em um controlador de domínio do Windows Server 2012  
  
-   Emulador do PDC disponível durante operações de clonagem  
  
Restauração de clonagem e segura exigem:  
  
-   Windows Server 2012 virtualizados convidados  
  
-   ID de geração de VM (VMGID) dá suporte a plataforma de host de virtualização  
  
Examine a tabela abaixo para produtos de virtualização e se eles dão suporte virtualizados controladores de domínio e ID de geração de VM.  
  
|||  
|-|-|  
|**Produto de virtualização**|**Suporta virtualizados controladores de domínio e VMGID**|  
|**Servidor do Microsoft Windows Server 2012 com o recurso do Hyper-V**|Sim|  
|**Microsoft Windows Server 2012 Hyper-V Server**|Sim|  
|**Recursos do Microsoft Windows 8 com o cliente Hyper-V**|Sim|  
|**Windows Server 2008 R2 e Windows Server 2008**|Não|  
|**Soluções de virtualização não são da Microsoft**|Contate o fornecedor|  
  
Mesmo que a Microsoft dá suporte a 7 Windows Virtual PC, Virtual PC 2007, Virtual PC 2004 e Virtual Server 2005, não podem executar convidados de 64 bits, nem eles dão suporte GenerationID VM.  
  
Para obter ajuda com produtos de virtualização de terceiros e sua posição de suporte com controladores de domínio virtualizado, contate o fornecedor diretamente.  
  
Para obter mais informações, reveja a política de suporte para [software Microsoft executado no software de virtualização de hardware não são da Microsoft](https://support.microsoft.com/kb/897615).  
  
### <a name="critical-caveats"></a>Advertências críticas  
Controladores de domínio virtualizado *não* suporte segura restauração dos procedimentos a seguir:  
  
-   Arquivos VHD e VHDX copiados manualmente arquivos VHD existentes  
  
-   Arquivos VHD e VHDX restaurados usando software de backup de disco de backup ou completo do arquivo  
  
> [!NOTE]  
> Arquivos VHDX são novos para o Windows Server 2012 Hyper-V.  
  
Nenhuma dessas operações é abrangida pelo VM GenerationID semântica e, portanto, não altere a ID de geração de VM. Restaurando o domínio controladores usando esses métodos resultaria uma reversão de USN e o controlador de domínio de quarentena ou apresentar objetos remanescentes e a necessidade de operações de limpeza largo de floresta.  
  
> [!WARNING]  
> Restauração segura de controlador de domínio virtualizado não é um substituto para backups de estado do sistema e a Lixeira de texto do AD DS.  
>   
> Depois de restaurar um instantâneo, as diferenças entre anteriormente cancelou replicadas alterações sejam originados pelo controlador de domínio após o instantâneo estarão permanentemente perdidas. Restauração segura implementa restauração não autorizada automatizada para evitar quarentena de controlador de domínio acidental *somente*.  
  
Para obter mais informações sobre USN bolhas e objetos remanescentes, consulte [operações do Active Directory de solução de problemas que uma falha com erro 8606: "atributos insuficientes foram dada para criar um objeto"](https://support.microsoft.com/kb/2028495).  
  
## <a name="BKMK_VDCCloning"></a>Virtualizados domínio clonagem do controlador  
Há um número de estágios e as etapas para clonagem um controlador de domínio virtualizado, independentemente do usando ferramentas gráficas ou o Windows PowerShell. Em um nível alto, os três estágios são:  
  
**Preparar o ambiente**  
  
-   Etapa 1: Validar o hipervisor dá suporte a ID de geração de VM e, portanto, clonagem  
  
-   Etapa 2: Verifique se que a função de emulador do PDC é hospedada por um controlador de domínio que executa o Windows Server 2012 e que ele está online e acessível pelo controlador de domínio clonados durante clonagem.  
  
**Preparar o controlador de domínio de origem**  
  
-   Etapa 3: Autorizar o controlador de domínio de origem para clonagem  
  
-   Etapa 4: Remover serviços incompatíveis ou programas ou adicioná-los ao arquivo CustomDCCloneAllowList.xml.  
  
-   Etapa 5: Criar DCCloneConfig.xml  
  
-   Etapa 6: Colocar o controlador de domínio de origem offline  
  
**Criar o controlador de domínio clonados**  
  
-   Etapa 7: Copiar ou exportar a fonte de VM e adicione o XML se não já copiou  
  
-   Etapa 8: Criar uma nova máquina virtual da cópia  
  
-   Etapa 9: Inicie a máquina virtual novo para começar a clonagem  
  
Não existem diferenças na operação de procedimentos ao usar ferramentas gráficas, como o Console de gerenciamento do Hyper-V ou ferramentas de linha de comando como o Windows PowerShell, portanto, as etapas são apresentadas apenas uma vez com ambas as interfaces. Este tópico fornece exemplos do Windows PowerShell para explorar automação de ponta a ponta do processo de clonagem; elas não são necessárias para todas as etapas. Existe uma ferramenta de gerenciamento gráfico virtualizado para controladores de domínio incluídos no Windows Server 2012.  
  
Há vários pontos no procedimento onde você tem opções para como criar o computador clonado e como adicionar os arquivos xml; estas etapas são indicadas nos detalhes abaixo. Caso contrário, o processo é inalterável.  
  
O diagrama a seguir ilustra o virtualizado processo controlador de domínio clonagem, onde o domínio já existe.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_CloningProcessFlow.png)  
  
### <a name="step-1---validate-the-hypervisor"></a>Etapa 1 - validar o hipervisor  
Certifique-se de que o controlador de domínio de origem está em execução em um hipervisor com suporte, analisando a documentação do fornecedor. Controladores de domínio virtualizado sejam independentes do hipervisor e não exigem Hyper-V.  
  
Se o hipervisor Microsoft Hyper-V, certifique-se de que ele está sendo executado no Windows Server 2012. Você pode validar isso usando o gerenciamento de dispositivo  
  
Abrir **Devmgmt.msc** e examine **dispositivos do sistema** para dispositivos Microsoft Hyper-V e drivers instalados. O dispositivo de sistema específico necessário para um controlador de domínio virtualizado é o **contador de geração do Microsoft Hyper-V** (driver: vmgencounter.sys).  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVVMGenIDCounter.png)  
  
### <a name="step-2---verify-the-pdce-fsmo-role"></a>Etapa 2 - Verifique se a função PDCE FSMO  
Antes de tentar clonar um controlador de domínio, você deve validar o controlador de domínio que hospeda o FSMO de emulador de controlador de domínio primário executa Windows Server 2012. O emulador do PDC (PDCE) é necessário por vários motivos:  
  
1.  O PDCE cria o especial **Cloneable controladores de domínio** de grupo e define sua permissão na raiz do domínio para permitir que um controlador de domínio clonar em si.  
  
2.  O controlador de domínio clonagem contata o PDCE diretamente usando o protocolo DRSUAPI RPC, para criar objetos de computador para o clone DC.  
  
    > [!NOTE]  
    > Windows Server 2012 estende o protocolo remoto do serviço de replicação de diretório (DRS) existente (UUID **E3514235-4B06-11D1-AB04-00C04FC2DCD2**) para incluir um novo método RPC **IDL_DRSAddCloneDC** (Opnum **28**). O **IDL_DRSAddCloneDC** método cria um novo objeto de controlador de domínio copiando atributos de um objeto de controlador de domínio existente.  
    >   
    > Os estados de um controlador de domínio são compostos de objetos de computador, servidor, as configurações NTDS, FRS, DFSR e conexão mantidos para cada controlador de domínio. Durante a duplicação de um objeto, esse método RPC substitui todas as referências ao controlador de domínio original com objetos correspondentes do novo controlador de domínio. O chamador deve ter o controle de acesso correto DS-Clone-controlador de domínio no contexto de nomenclatura do domínio.  
    >   
    > O uso desse método sempre requer acesso direto para o controlador de domínio de emulador do PDC do chamador.  
    >   
    > Como esse método RPC é novo, o software de análise de rede requer analisadores atualizados para incluir os campos para o novo 28 Opnum no existente UUID E3514235-4B06 - 11D 1-AB04-00C04FC2DCD2. Caso contrário, você não pode analisar esse tráfego.  
    >   
    > Para obter mais informações, consulte [4.1.29 IDL_DRSAddCloneDC (28 Opnum)](https://msdn.microsoft.com/en-us/library/hh554213(v=prot.13).aspx).  
  
***Isso também significa que quando usando as redes que não são totalmente roteadas, clonagem de controlador de domínio virtualizado requer segmentos de rede com acesso ao PDCE***. É aceitável para mover um controlador de domínio clonados a uma rede diferente após a clonagem - assim como um controlador de domínio físico - desde que você tenha o cuidado de atualizar as informações de site lógico do AD DS.  
  
> [!IMPORTANT]  
> Ao clonar um domínio que contém somente um único controlador de domínio, você deve garantir que o controlador de domínio de origem está online novamente antes de iniciar as cópias clone. Um domínio de produção sempre deve conter pelo menos dois controladores de domínio.  
  
#### <a name="active-directory-users-and-computers-method"></a>Os usuários do Active Directory e o método de computadores  
  
1.  Usando o snap-in Dsa.msc, clique com botão direito no domínio e clique em **mestres de operações**. Observe o controlador de domínio denominado na guia PDC e feche a caixa de diálogo.  
  
2.  Clique com botão direito o objeto de computador do controlador de domínio e clique em **propriedades**e, em seguida, validar as informações do sistema operacional.  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode combinar os seguintes cmdlets do módulo do Active Directory Windows PowerShell para retornar a versão do emulador do PDC:  
  
```  
Get-adddomaincontroller  
Get-adcomputer  
```  
  
Se não fornecido no domínio, esses cmdlets assumir o domínio do computador onde executado.  
  
O comando a seguir retorna informações PDCE e sistema operacional:  
  
```  
get-adcomputer(Get-ADDomainController -Discover -Service "PrimaryDC").name -property * | format-list dnshostname,operatingsystem,operatingsystemversion  
```  
  
Este exemplo a seguir demonstra especificando o nome de domínio e filtrando as propriedades retornadas antes de pipeline do Windows PowerShell:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PDCOSInfo.png)  
  
### <a name="step-3---authorize-a-source-dc"></a>Etapa 3 - autorizar um controlador de domínio de origem  
O controlador de domínio de origem deve ter controle acesso à direita (CARRO) **permitem que um controlador de domínio criar um clone de si** no domínio head NC. Por padrão, o grupo conhecido **Cloneable controladores de domínio** tem essa permissão e não contém membros. O PDCE cria esse grupo quando essa função FSMO transfere para um controlador de domínio do Windows Server 2012.  
  
#### <a name="active-directory-administrative-center-method"></a>Método de centro administrativo do Active Directory  
  
1.  Iniciar Dsac.exe e navegue até o controlador de domínio de origem e abra sua página de detalhes.  
  
2.  No **membro de** seção, adicione o **Cloneable controladores de domínio** grupo para esse domínio.  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode combinar os seguintes cmdlets do PowerShell módulo do Active Directory Windows **get-adcomputer** e **adgroupmember adicionar** para adicionar um controlador de domínio para o **Cloneable controladores de domínio** grupo:  
  
```  
Get-adcomputer <dc name> | %{add-adgroupmember "cloneable domain controllers" $_.samaccountname}  
```  
  
Por exemplo, isso adiciona servidor DC1 ao grupo, sem a necessidade de especificar o nome diferenciado do membro do grupo:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_AddDcToGroup.png)  
  
#### <a name="rebuilding-default-permissions"></a>Recriando permissões padrão  
Se você remover essa permissão do cabeçote de domínio, clonagem falha. Você pode recriar a permissão usando o Centro Administrativo do Active Directory ou o Windows PowerShell.  
  
##### <a name="active-directory-administrative-center-method"></a>Método de centro administrativo do Active Directory  
  
1.  Abrir **Centro Administrativo do Active Directory**, clique com botão direito head do domínio, clique em **propriedades**, clique no **extensões** , clique em **segurança**e clique em **avançado**. Clique em **somente esse objeto**.  
  
2.  Clique em **adicionar**, em **insira o nome do objeto para selecionar**, digite o nome do grupo **Cloneable controladores de domínio.**  
  
3.  Em permissões, clique em **permitem que um controlador de domínio criar um clone de si**e clique em **Okey**.  
  
> [!NOTE]  
> Você também pode remover a permissão de padrão e adicionar controladores de domínio individuais. Isso é provavelmente causará problemas de manutenção contínua no entanto, em que os administradores novo desconhece essa personalização. Alterando a configuração padrão não aumente a segurança e é.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Use os seguintes comandos em um prompt de console do Windows PowerShell elevado de administrador. Esses comandos detectam o nome de domínio e adicione-os nas permissões padrão:  
  
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
  
Como alternativa, execute a amostra [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms) em um console do Windows PowerShell, onde o console é iniciado como um administrador com privilégios elevados em um controlador de domínio no domínio afetado. -Definir automaticamente as permissões. O exemplo está localizado no Apêndice deste módulo.  
  
### <a name="step-4---remove-incompatible-applications-or-services-if-not-using-customdccloneallowlistxml"></a>Etapa 4 - Remover incompatível aplicativos ou serviços (se não estiver usando CustomDCCloneAllowList.xml)  
Todos os programas ou serviços anteriormente retornados por Get-ADDCCloningExcludedApplicationList - *e não foi adicionado para o CustomDCCloneAllowList.xml* -deve ser removida antes de clonagem. Desinstalar o aplicativo ou serviço é o método recomendado.  
  
> [!WARNING]  
> Qualquer programa incompatível ou impede que o serviço não desinstalados ou adicionados para o CustomDCCloneAllowList.xml clonagem.  
  
Use o cmdlet Get-AdComputerServiceAccount para localizar qualquer conta de serviço gerenciado do autônomo (MSAs) no domínio e se este computador está usando qualquer um deles. Se qualquer MSA estiver instalado, use o cmdlet desinstalar ADServiceAccount para remover a conta de serviço instalado localmente. Depois de terminar com levando o controlador de domínio de origem offline na etapa 6, você pode adicionar novamente o MSA usando ADServiceAccount instalar quando o servidor está online novamente. Para obter mais informações, consulte [desinstalar ADServiceAccount](https://technet.microsoft.com/en-us/library/hh852310).  
  
> [!IMPORTANT]  
> MSAs autônomo - lançado pela primeira vez no Windows Server 2008 R2 - foram substituídos no Windows Server 2012 com MSAs de grupo. Suportam a grupo MSAs clonagem.  
  
### <a name="step-5---create-dccloneconfigxml"></a>Etapa 5 - Criar DCCloneConfig.xml  
O arquivo DcCloneConfig.xml é necessário para clonagem controladores de domínio. Seu conteúdo permitem que você especifique detalhes exclusivos, como o novo nome do computador e o endereço IP.  
  
O arquivo CustomDCCloneAllowList.xml é opcional, a menos que você instalar aplicativos ou serviços do Windows potencialmente incompatíveis no controlador de domínio de origem. Os arquivos exigem precisos de nomenclatura, formatação e posicionamento; Caso contrário, clonagem falha.  
  
Por esse motivo, você sempre deve usar os cmdlets do Windows PowerShell para criar os arquivos XML e colocá-los no local correto.  
  
#### <a name="generating-with-new-addccloneconfigfile"></a>Gerando com novos ADDCCloneConfigFile  
O módulo do Active Directory Windows PowerShell contém um novo cmdlet no Windows Server 2012:  
  
```  
New-ADDCCloneConfigFile  
```  
  
Execute o cmdlet no controlador de domínio de origem proposto que você pretende clone. O cmdlet oferece suporte a vários argumentos e quando usada, sempre testa o computador e o ambiente em que ele é executado, a menos que você especifique o - argumento offline.  
  
||||  
|-|-|-|  
|**Active Directory**<br /><br />**Cmdlet**|**Argumentos**|**Explicação**|  
|**Novo ADDCCloneConfigFile**|*<no argument specified>*|Cria um arquivo DcCloneConfig.xml em branco no diretório de trabalho DSA (padrão: % systemroot%\ntds)|  
||-CloneComputerName|Especifica o nome do computador clone DC. Tipo de dados de cadeia de caracteres.|  
||-Path|Especifica a pasta para criar o DcCloneConfig.xml. Se não for especificado, grava o diretório de trabalho DSA (padrão: % systemroot%\ntds). Tipo de dados de cadeia de caracteres.|  
||-Nome do site|Especifica o nome de site lógico de anúncios para participar durante a criação de conta de computador clonados. Tipo de dados de cadeia de caracteres.|  
||-IPv4Address|Especifica o endereço IPv4 estático do computador clonado. Tipo de dados de cadeia de caracteres.|  
||-IPv4SubnetMask|Especifica a máscara de sub-rede IPv4 estática do computador clonado. Tipo de dados de cadeia de caracteres.|  
||-IPv4DefaultGateway|Especifica o endereço de gateway padrão estático IPv4 do computador clonado. Tipo de dados de cadeia de caracteres.|  
||-IPv4DNSResolver|Especifica as entradas de DNS IPv4 estáticas do computador clonado em uma lista separada por vírgulas. Tipo de dados de matriz. Até quatro entradas podem ser fornecidas.|  
||-PreferredWINSServer|Especifica o endereço IPv4 estático do servidor WINS primário. Tipo de dados de cadeia de caracteres.|  
||-AlternateWINSServer|Especifica o endereço IPv4 estático do servidor WINS secundário. Tipo de dados de cadeia de caracteres.|  
||-IPv6DNSResolver|Especifica as entradas de DNS IPv6 estáticas do computador clonado em uma lista separada por vírgulas. Não há nenhuma maneira de definir informações estáticas Ipv6 na clonagem de controlador de domínio virtualizado. Tipo de dados de matriz.|  
||-Offline|Não executar os testes de validação e substitui qualquer dccloneconfig.xml existente. Não tem parâmetros. Para obter mais informações, consulte [nova ADDCCloneConfigFile em execução em modo offline](../../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).|  
||*-Estático*|Obrigatório se especificar argumentos IP estáticos IPv4SubnetMask, IPv4SubnetMask ou IPv4DefaultGateway. Não tem parâmetros.|  
  
Testes realizados quando executado em modo online:  
  
-   Emulador do PDC é o Windows Server 2012 ou posterior  
  
-   Controlador de domínio de origem for um membro do grupo Cloneable controladores de domínio  
  
-   Controlador de domínio de origem não inclui quaisquer serviços ou aplicativos excluídos  
  
-   Controlador de domínio de origem ainda não contiver um DcCloneConfig.xml no caminho especificado  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewDCCloneConfig.png)  
  
### <a name="step-6---take-the-source-domain-controller-offline"></a>Etapa 6 - desativar o controlador de domínio de origem  
Você não pode copiar uma fonte em execução DC; ele deve ser encerrado normalmente. Não clone um controlador de domínio interrompido por perda de energia graceless.  
  
#### <a name="graphical-method"></a>Método gráfico  
Use o botão de desligamento dentro do controlador de domínio em execução ou o botão de desligamento do Gerenciador do Hyper-V.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_Shutdown.png)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVShutdown.png)  
  
#### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode desligar uma máquina virtual usando um dos cmdlets a seguir:  
  
```  
Stop-computer  
Stop-vm  
```  
  
Parar o computador é um cmdlet que permite desligar a computadores, independentemente de virtualização e é análoga ao utilitário Shutdown.exe herdado. Parar vm é um novo cmdlet no módulo Windows Server 2012 Hyper-V Windows PowerShell e é equivalente às opções de energia no Gerenciador do Hyper-V. A última opção é útil em ambientes de laboratório em que o controlador de domínio muitas vezes opera em uma rede privada virtualizada.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopComputer2.png)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_StopVM.png)  
  
### <a name="step-7---copy-disks"></a>Etapa 7 - discos de cópia  
Uma escolha administrativa é necessária na fase de cópia:  
  
-   Copie os discos manualmente, sem Hyper-V  
  
-   Exportar a VM, usando o Hyper-V  
  
-   Exportar os discos mesclados, usando o Hyper-V  
  
Todos os discos de uma máquina virtual devem ser copiados, não apenas a unidade do sistema. Se o controlador de domínio de origem usa discos diferenciais e quiser mover o controlador de domínio clonados para outro host do Hyper-V, você deve exportar.  
  
Copiar discos manualmente é recomendada quando o controlador de domínio de origem tem apenas *um* unidade. Exportação e importação é recomendada para VMs com *mais de um* unidade ou outras personalizações de hardware virtualizado complexos, como várias placas de rede.  
  
Se copiar manualmente os arquivos, exclua qualquer instantâneos antes da cópia. Se estiver exportando a VM, exclua instantâneos antes de exportar ou excluí-los da nova VM após a importação.  
  
> [!WARNING]  
> Instantâneos são imagem diferencial de discos que podem retornar um controlador de domínio ao estado anterior. Se você fosse clonar um controlador de domínio e, em seguida, restaurar seu previamente clonagem instantâneo, você acabaria com controladores de domínio duplicado na floresta. Não há nenhum valor nos instantâneos anteriores em um controlador de domínio recentemente clonados.  
  
#### <a name="manually-copying-disks"></a>Copiar manualmente os discos  
  
##### <a name="hyper-v-manager-method"></a>Método Gerenciador do Hyper-V  
Use o snap-in Gerenciador do Hyper-V para determinar quais discos estão associados com o controlador de domínio de origem. Use a opção Inspect para validar se o controlador de domínio usa discos diferenciais (que exige que você copie o disco pai também)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVInspect.png)  
  
Para excluir instantâneos, selecione uma VM e excluir a subárvore instantâneo.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDeleteSnapshot.gif)  
  
Você pode copiar manualmente os arquivos VHD ou VHDX usando o Windows Explorer, Xcopy.exe ou Robocopy.exe. Não há etapas especiais são necessárias. É uma prática recomendada para alterar os nomes de arquivo, mesmo se movendo para outra pasta.  
  
> [!NOTE]  
> Se copiar entre os computadores host em uma rede local (1 GB ou superior), o **Xcopy.exe /J** opção copia arquivos VHD/VHDX consideravelmente mais rápido do que qualquer outra ferramenta, às custas muito maior do uso de largura de banda.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para determinar os discos usando o Windows PowerShell, use os módulos do Hyper-V:  
  
```  
Get-vmidecontroller  
Get-vmscsicontroller  
Get-vmfibrechannelhba  
Get-vmharddiskdrive  
```  
  
Por exemplo, você pode retornar todos os discos rígidos IDE de uma VM denominada **DC2** com a amostra a seguir:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_ReturnIDE.png)  
  
Se o caminho do disco aponta para um arquivo AVHD ou AVHDX, é um instantâneo. Para excluir os instantâneos associados a um disco e mesclar no VHD ou VHDX real, use cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por exemplo, para excluir todos os instantâneos de uma VM denominada DC2 SOURCECLONE:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_DelSnapshots.png)  
  
Para copiar os arquivos usando o Windows PowerShell, use o seguinte cmdlet:  
  
```  
Copy-Item  
```  
  
Combine com os cmdlets VM em pipelines para ajudar a automação. O pipeline é um canal usado entre vários cmdlets para transmitir dados. Por exemplo copiar a unidade de um controlador de domínio de origem offline denominado DC2 SOURCECLONE em um novo disco chamado c:\temp\copy.vhd sem a necessidade de saber o caminho exato para sua unidade do sistema:  
  
```  
Get-VMIdeController dc2-sourceclone | Get-VMHardDiskDrive | select-Object {copy-item -path $_.path -destination c:\temp\copy.vhd}  
```  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSCopyDrive.png)  
  
> [!IMPORTANT]  
> Você não pode usar discos de passagem com clonagem, como eles não usem um arquivo de disco virtual em vez disso, mas real em um disco rígido.  
  
> [!NOTE]  
> Para saber mais sobre mais operações do Windows PowerShell com pipelines, consulte [Piping e o Pipeline no Windows PowerShell](https://technet.microsoft.com/en-us/library/ee176927.aspx).  
  
#### <a name="exporting-the-vm"></a>Exportando a VM  
Como alternativa ao copiar os discos, você pode exportar toda a VM do Hyper-V como uma cópia. Exportando automaticamente cria uma pasta denominada para a VM e que contém todos os discos e informações de configuração.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVExport.png)  
  
##### <a name="hyper-v-manager-method"></a>Método Gerenciador do Hyper-V  
Para exportar uma VM com o Gerenciador do Hyper-V:  
  
1.  Clique com botão direito no controlador de domínio de origem e clique em **exportar**.  
  
2.  Selecione uma pasta existente como o contêiner de exportação.  
  
3.  Aguarde até que a coluna Status parar de mostrar **exportando**.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para exportar uma VM usando o módulo do Hyper-V Windows PowerShell, use o cmdlet:  
  
```  
Export-vm  
```  
  
Por exemplo exportar uma VM denominado DC2 SOURCECLONE para uma pasta chamada C:\VM:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSExport.png)  
  
> [!NOTE]  
> Windows Server 2012 Hyper-V dá suporte à nova exportar e importar os recursos que estão fora do escopo deste treinamento. Examine TechNet para obter mais informações.  
  
#### <a name="exporting-merged-disks-using-hyper-v"></a>Exportando discos mesclados, usando o Hyper-V  
A última opção é usar as opções de mesclagem e conversão de disco dentro do Hyper-V. Elas permitem que você faça uma cópia de uma estrutura de disco existente - mesmo quando incluindo arquivos AVHD/AVHDX instantâneo - em um único disco novo. Como o cenário de cópia do manual de disco, isso é basicamente voltado para máquinas virtuais mais simples que usam apenas uma única unidade, por exemplo, C:\\. Sua solitária vantagem é que, diferentemente copiar manualmente, ele não é necessário primeiro exclua instantâneos. Essa operação é necessariamente mais lenta do que simplesmente excluir os instantâneos e copiar discos.  
  
##### <a name="hyper-v-manager-method"></a>Método Gerenciador do Hyper-V  
Para criar um disco mesclado usando o Gerenciador do Hyper-V:  
  
1.  Clique em **Editar disco**.  
  
2.  Procure o disco mais baixo do filho. Por exemplo, se você estiver usando um diferencial de disco, o disco filho é o filho menor. Se a máquina virtual tem um instantâneo (ou várias dessas classes), o instantâneo atualmente selecionado é o disco de filho menor.  
  
3.  Selecione o **mesclar** opção para criar um único disco fora a estrutura de pai-filho inteiro.  
  
4.  Selecione um novo disco rígido virtual e forneça um caminho. Isso sincroniza os arquivos VHD/VHDX existentes em uma única unidade portátil novo que não está em risco de restaurar instantâneos anteriores.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Para criar um disco mesclado em um conjunto complexo de pais usando o módulo do Hyper-V Windows PowerShell, use o cmdlet:  
  
```  
Convert-vm  
```  
  
Por exemplo, para exportar toda a cadeia de instantâneos de disco da VM (incluindo desta vez não discos diferenciais) e pai disco em um único disco novo denominado clonados DC4. VHDX:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSConvertVhd.png)  
  
#### <a name="BKMK_Offline"></a>Adicionando o XML para o disco do sistema Offline  
Se você copiar o Dccloneconfig.xml para o controlador de domínio de origem em execução, você deve copiar o arquivo dccloneconfig.xml atualizado para o disco do sistema offline copiados/exportado agora. Dependendo aplicativos instalados detectados com Get-ADDCCloningExcludedApplicationList anteriormente, talvez também seja necessário copiar o arquivo CustomDCCloneAllowList.xml no disco.  
  
Os seguintes locais podem conter o arquivo DcCloneConfig.xml:  
  
1.  Diretório de trabalho DSA  
  
2.  %windir%\ntds  
  
3.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade  
  
Esses caminhos não são configuráveis. Depois de clonagem começa, as verificações de clonagem esses locais nessa ordem específica e usa a primeira DcCloneConfig.xml arquivos encontrados, independentemente do outro conteúdo da pasta.  
  
Os seguintes locais podem conter o arquivo CustomDCCloneAllowList.xml:  
  
1.  HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters  
  
    AllowListFolder (*REG_SZ*)  
  
2.  Diretório de trabalho DSA  
  
3.  %windir%\ntds  
  
4.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade  
  
Você pode executar ADDCCloneConfigFile de novo com o **-offline** argumento (também conhecido como offline modo) para criar o arquivo DcCloneConfig.xml e coloque-o em um local correto. Os exemplos a seguir mostram como executar a nova ADDCCloneConfigFile em modo offline.  
  
Para criar um controlador de domínio clone denominado CloneDC1 em modo offline, em um site chamado "REDMOND" com endereço IPv4 estático, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio clone denominado Clone2 em modo offline com IPv4 estático e configurações de IPv6 estáticas, tipo:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio clone em modo offline com IPv4 estático e dinâmico IPv6 configurações e especificar vários servidores DNS para as configurações de resolução DNS, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS   
```  
  
Para criar um controlador de domínio clone denominado Clone1 em modo offline com IPv4 dinâmico e configurações de IPv6 estáticas, tipo:  
  
```  
New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS  
```  
  
Para criar um controlador de domínio clone em modo offline com dinâmicas configurações de IPv6 e IPv4 dinâmico, digite:  
  
```  
New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS  
  
```  
  
##### <a name="windows-explorer-method"></a>Método do Windows Explorer  
Windows Server 2012 agora oferece uma opção gráfica para montar arquivos VHD e VHDX. Isso requer a instalação do recurso Experiência Desktop no Windows Server 2012.  
  
1.  Clique no arquivo VHD/VHDX recém-copiada que contém a unidade do sistema ou uma pasta de localização de diretório de trabalho DSA a origem do controlador de domínio e, em seguida, clique em **montar** do **ferramentas de imagem de disco** menu.  
  
2.  Na unidade montada agora, copie os arquivos XML para um local válido. Você pode ser consultado para a pasta permissões.  
  
3.  Clique na unidade montada e clique em **ejetar** do **ferramentas disco** menu.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVClickMountedDrive.png)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVDetailsMountedDrive.gif)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVEjectMountedDrive.gif)  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Como alternativa, você pode montar o disco offline e copie o arquivo XML usando os cmdlets do Windows PowerShell:  
  
```  
mount-vhd  
get-disk  
get-partition  
get-volume  
Add-PartitionAccessPath  
Copy-Item  
```  
  
Isso permite que você controle total sobre o processo. Por exemplo, a unidade pode ser montada com uma letra de unidade específica, o arquivo copiado e desmontar a unidade.  
  
```  
mount-vhd <disk path> -passthru -nodriveletter | get-disk | get -partition | get-volume | get-partition | where {$_.partition number -eq 2} | Add-PartitionAccessPath -accesspath <drive letter>  
  
copy-item <xml file path><destination path>\dccloneconfig.xml  
  
dismount-vhd <disk path>  
```  
  
Por exemplo:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSMountVHD.png)  
  
Como alternativa, você pode usar o novo **DiskImage de montagem** cmdlet para montar um arquivo VHD (ou ISO).  
  
### <a name="step-8---create-the-new-virtual-machine"></a>Etapa 8 - criar a nova máquina Virtual  
A etapa de configuração final antes de iniciar o processo de clonagem está criando uma nova VM que usa os discos do controlador de domínio de origem copiados. Dependendo da seleção feita na fase de discos de cópia, você tem duas opções:  
  
1.  Associar uma nova VM do disco copiado  
  
2.  Importar a VM exportada  
  
#### <a name="associating-a-new-vm-with-copied-disks"></a>Associando uma VM novos discos copiados  
Se você copiou o disco do sistema manualmente, você deve criar uma nova máquina virtual usando o disco copiado. O hipervisor define a ID de geração de VM automaticamente quando uma nova VM é criada; Nenhuma alteração de configuração é necessárias no host de VM ou Hyper-V.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVConnectVHD.gif)  
  
##### <a name="hyper-v-manager-method"></a>Método Gerenciador do Hyper-V  
  
1.  Crie uma nova máquina virtual.  
  
2.  Especifique o nome VM, memória e rede.  
  
3.  Na página de conectar-se do disco rígido Virtual, especifique o disco do sistema copiados.  
  
4.  Conclua o Assistente para criar a VM.  
  
Se houvesse vários discos, adaptadores de rede ou outras personalizações, configurá-los antes de iniciar o controlador de domínio. O método "Export-Import" de copiar os discos é recomendado para VMs complexas.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode usar o módulo do Hyper-V Windows PowerShell para automatizar a criação de VM no Windows Server 2012, usando o seguinte cmdlet:  
  
```  
New-VM  
```  
  
Por exemplo, aqui a VM DC4 CLONEDFROMDC2 é criada, usando 1GB de RAM, inicializar do arquivo c:\vm\dc4-systemdrive-clonedfromdc2.vhd e usando a rede virtual 10.0:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSNewVM.png)  
  
#### <a name="import-vm"></a>Importar VM  
Se você já tiver exportado seu VM, agora é necessário importá-lo novamente como uma cópia. Isso usa o XML exportado para recriar o computador usando todas as configurações anteriores, unidades, redes e configurações de memória.  
  
Se você pretende criar cópias adicionais da mesma VM exportado, faça quantas cópias da VM exportado conforme necessário. Em seguida, use importar para cada cópia.  
  
> [!IMPORTANT]  
> É importante usar o **cópia** opção, como exportar preserva todas as informações de origem; o servidor com a importação **mover** ou **no lugar** faz com que a colisão de saber se feito no mesmo servidor de host do Hyper-V.  
  
##### <a name="hyper-v-manager-method"></a>Método Gerenciador do Hyper-V  
Para importar usando o snap-in Gerenciador do Hyper-V:  
  
1.  Clique em **importar a máquina Virtual**  
  
2.  Sobre o **localizar pasta** página, selecione o arquivo de definição de VM exportado usando o botão Procurar  
  
3.  Sobre o **Selecione Máquina Virtual** página, clique no computador de origem.  
  
4.  No **escolher o tipo de importação** página, clique em **copiar a máquina virtual (criar uma nova ID exclusiva)**, em seguida, clique em **concluir**.  
  
5.  Renomeie a VM importada se importando no mesmo host do Hyper-V; ele terá o mesmo nome que o controlador de domínio de origem exportados.  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportLocateFolder.png)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportSelectVM.png)  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportChooseType.gif)  
  
Lembre-se de remover qualquer instantâneos importados, usando o snap-in de gerenciamento do Hyper-V:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_HyperVImportDelSnap.gif)  
  
> [!WARNING]  
> Excluir qualquer instantâneos importados é extremamente importante; Se aplicados, eles retornaria o controlador de domínio clonados do estado de um controlador de domínio anterior - e possivelmente dinâmico -, levando a falha na replicação, informações duplicadas de IP e outras interrupções.  
  
##### <a name="windows-powershell-method"></a>Método do Windows PowerShell  
Você pode usar o módulo do Hyper-V Windows PowerShell para automatizar a importação VM no Windows Server 2012, usando os seguintes cmdlets:  
  
```  
Import-VM  
Rename-VM  
```  
  
Por exemplo, aqui a exportados clonados DC2 VM é importado usando seu arquivo XML determinado automaticamente e renomeado imediatamente para o novo nome VM DC5 CLONEDFROMDC2:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSImportVM.png)  
  
Lembre-se de remover qualquer instantâneos importados, usando os seguintes cmdlets:  
  
```  
Get-VMSnapshot  
Remove-VMSnapshot  
```  
  
Por exemplo:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSGetVMSnap.png)  
  
> [!WARNING]  
> Certifique-se de que, ao importar o computador, endereços MAC estáticos não foram atribuídos ao controlador de domínio de origem. Se um computador de origem com um MAC estático é clonado, esses computadores copiadas corretamente não irá enviar ou receber qualquer tráfego de rede. Defina um novo exclusivo estático ou dinâmico endereço MAC se esse for o caso. Você pode ver se uma VM usa endereços MAC estáticos com o comando:  
>   
> **Get-VM - VMName**   
>  ***teste-vm* | Get-VMNetworkAdapter | Flórida \ ***  
  
### <a name="step-9---clone-the-new-virtual-machine"></a>Etapa 9 - Clone a nova máquina Virtual  
Opcionalmente, antes de começar clonagem, reinicie o controlador de domínio de origem clone offline. Verifique se o emulador do PDC está online, independentemente.  
  
Para começar a clonagem, simplesmente comece a nova máquina virtual. O processo inicia automaticamente e o controlador de domínio é reinicializado automaticamente após a conclusão da clonagem.  
  
> [!IMPORTANT]  
> Não é recomendável manter os controladores de domínio estiver desativado por um longo período de tempo e se o clone é ingressar no mesmo site como seu controlador de domínio de origem, o intra inicial e a topologia de duplicação entre sites podem demorar mais para compilar se o controlador de domínio de origem estiver offline.  
  
Se usando o Windows PowerShell para iniciar uma VM, o novo cmdlet do módulo do Hyper-V é:  
  
```  
Start-VM  
```  
  
Por exemplo:  
  
![Implantação virtualizada DC](media/Virtualized-Domain-Controller-Deployment-and-Configuration/ADDS_VDC_PSStartVM.png)  
  
Depois que o computador é reiniciado após a conclusão de clonagem, é um controlador de domínio e você pode fazer logon em normalmente para confirmar a operação normal. Se houver erros, o servidor está definido para iniciar no modo de restauração de serviços de diretório para a investigação.  
  
## <a name="BKMK_VDCSafeRestore"></a>Proteções de virtualização  
Diferentemente da clonagem de controlador de domínio virtualizado, proteções de virtualização do Windows Server 2012 não têm nenhuma etapa de configuração. O recurso funciona sem a intervenção desde que você conheça algumas condições simples:  
  
-   O hipervisor dá suporte a ID de geração de VM  
  
-   Há um controlador de domínio do parceiro válido que um controlador de domínio restaurado pode replicar as alterações de sem autoridade.  
  
### <a name="validate-the-hypervisor"></a>Validar o hipervisor  
Certifique-se de que o controlador de domínio de origem está em execução em um hipervisor com suporte, analisando a documentação do fornecedor. Controladores de domínio virtualizado sejam independentes do hipervisor e não exigem Hyper-V.  
  
Examine a anterior [requisitos de plataforma](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_PlatformReqs) seção para suporte a ID de geração de VM conhecido.  
  
Se você estiver migrando VMs do hipervisor origem para um hipervisor de destino diferente, proteções de virtualização podem ou não podem ser disparadas dependendo dos hipervisores dar suporte a ID de geração de VM, conforme explicado na tabela a seguir.  
  
|Fonte hipervisor|Hipervisor de destino|Resultado|  
|---------------------|---------------------|----------|  
|Dá suporte à geração de VM ID|Não oferece suporte a ID de geração de VM|Proteções não disparadas (se houver um DCCloneConfigFile.xml, DC será inicializado no DSRM)|  
|Não oferece suporte a ID de geração de VM|Dá suporte à geração de VM ID|Proteções disparadas|  
|Dá suporte à geração de VM ID|Dá suporte à geração de VM ID|Proteções não disparadas porque a definição de VM não foi alterado, o que significa para que geração de VM ID permanece o mesmo|  
  
### <a name="validate-the-replication-topology"></a>Validar a topologia de replicação  
Proteções de virtualização iniciam a duplicação não autorizada de entrada para o delta de replicação do Active Directory, bem como não-autorizada nova sincronização de todo o conteúdo SYSVOL. Isso garante o controlador de domínio retorna de um instantâneo, com funcionalidade completa e, eventualmente, consistente com o restante do ambiente.  
  
Esse novo recurso traz vários requisitos e limitações:  
  
-   Um controlador de domínio restaurado deve ser capaz de entrar em contato com um controlador de domínio gravável  
  
-   Todos os controladores de domínio em um domínio não devem ser restaurados simultaneamente  
  
-   Quaisquer alterações proveniente de um controlador de domínio restaurado que ainda não foram replicados saída desde que o instantâneo foi tirado serão perdidas para sempre  
  
Enquanto a seção solução de problemas aborda estes cenários, detalhes abaixo garantir que você não crie uma topologia que pode causar problemas.  
  
#### <a name="writable-domain-controller-availability"></a>Disponibilidade de controlador de domínio gravável  
Se restaurado, um controlador de domínio deve ter conectividade com um controlador de domínio gravável; um controlador de domínio somente leitura não pode enviar o delta de atualizações. A topologia é provável correto para que isso já, como um controlador de domínio gravável sempre necessário um parceiro gravável. No entanto, se todos os controladores de domínio gravável estiver restaurando simultaneamente, nenhum deles pode encontrar uma origem válida. O mesmo vale se os controladores de domínio gravável estiverem offline para manutenção ou caso contrário inacessível através da rede.  
  
#### <a name="simultaneous-restore"></a>Restauração simultânea  
Não restaure todos os controladores de domínio em um único domínio simultaneamente. Se todos os instantâneos de uma vez, restaurar funciona a replicação do Active Directory normalmente, mas para de replicação de SYSVOL. A arquitetura de restauração do FRS e DFSR requer a definição de sua instância de réplica para o modo de sincronização não autorizada. Se todos os controladores de domínio restauração ao mesmo tempo, e cada controlador de domínio marcas em si não autorizada para SYSVOL, todos eles, em seguida, tentar sincronizar políticas de grupo e os scripts de um parceiro autorizado; Nesse ponto, porém, todos os parceiros também são não autorizada.  
  
> [!IMPORTANT]  
> Se todos os controladores de domínio são restaurados ao mesmo tempo, use os artigos a seguir para definir um controlador de domínio - normalmente o emulador do PDC - como autoritativo, para que os controladores de domínio podem retornar à operação normal:  
>   
> [Usando a chave de Registro BurFlags para reinicializar o serviço de replicação de arquivo define réplica](https://support.microsoft.com/kb/290762)  
>   
> [Como forçar uma sincronização autorizada e não autorizados para SYSVOL replicados DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556)  
  
> [!WARNING]  
> Não execute todos os controladores de domínio em um domínio ou floresta no mesmo hipervisor host. Isso gera um único ponto de falha que atrapalha o AD DS, Exchange, SQL e outras operações empresariais cada vez que o hipervisor fica offline. Isso não é diferente de usar apenas um controlador de domínio para todo um domínio ou floresta. Vários controladores de domínio em várias plataformas ajudam a fornecer redundância e tolerância.  
  
#### <a name="post-snapshot-replication"></a>Pós-instantâneo replicação  
Não restaure instantâneos até que todas as alterações localmente originador feitas desde a criação de instantâneo foram replicados saídas. Quaisquer alterações de origem serão perdidas para sempre se outros controladores de domínio não já recebem-los por meio de replicação.  
  
Use Repadmin.exe para mostrar as alterações de saída cancelou replicadas entre um controlador de domínio e seus parceiros:  
  
1.  Retorne parceiro do DC nomes e os GUIDs de objeto DSA com:  
  
    ```  
    Repadmin.exe /showrepl <DC Name of the partner> /repsto  
    ```  
  
2.  Retorne a pendente a duplicação de entrada do controlador de domínio do parceiro para o controlador de domínio a serem restauradas:  
  
    ```  
    Repadmin.exe /showchanges < Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare>  
    ```  
  
Como alternativa, apenas para ver o número de alterações cancelou replicadas:  
  
```  
Repadmin.exe /showchanges <Name of partner DC><DSA Object GUID of the domain controller being restored><naming context to compare> /statistics  
```  
  
Por exemplo (com a saída modificada para facilitar a leitura e entradas importantes ***em itálico***), aqui você olhar as parcerias de replicação de DC4:  
  
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
  
Agora você sabe que ele está replicando com DC2 e DC3. Em seguida, mostrar a lista de alterações que DC2 estados ele ainda não tiver da DC4 e ver que há um novo grupo:  
  
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
  
Você também pode testar o outros parceiros para garantir que ele tinha não já replicados.  
  
Como alternativa, se você não se preocupar com os objetos que não tinham replicados e somente se importaria que todos os objetos foram pendentes, você pode usar o **/statistics** opção:  
  
```  
C:\>repadmin /showchanges dc2.corp.contoso.com 5d083398-4bd3-48a4-a80d-fb2ebafb984f dc=corp,dc=contoso,dc=com /statistics  
  
***********************************************  
********* Grand total *************************  
Packets:              1  
Objects:              1Object Additions:     1Object Modifications: 0Object Deletions:     0Object Moves:         0Attributes:           12Values:               13  
```  
  
> [!IMPORTANT]  
> Teste todos os parceiros graváveis se você vir quaisquer falhas ou replicação pendente. Desde que pelo menos um é convergido, é geralmente segura para restaurar o instantâneo, pois replicação transitiva eventualmente sincroniza os outros servidores.  
>   
> Certifique-se observar quaisquer erros de replicação mostrado por /showchanges e não continua até que eles sejam corrigidos.  
  
### <a name="windows-powershell-snapshot-cmdlets"></a>Cmdlets do Windows PowerShell instantâneo  
Os seguintes cmdlets do módulo Hyper-V do Windows PowerShell oferecem recursos de captura de tela no Windows Server 2012:  
  
```  
Checkpoint-VM  
Export-VMSnapshot  
Get-VMSnapshot  
Remove-VMSnapshot  
Rename-VMSnapshot  
Restore-VMSnapshot  
```  
  


