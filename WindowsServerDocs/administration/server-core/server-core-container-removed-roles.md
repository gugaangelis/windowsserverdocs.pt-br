---
title: Funções, serviços de função e recursos não na versão de contêineres - Windows Server, Server Core 1803
description: Saiba mais sobre as funções e recursos que são removidos da imagem de contêiner do núcleo do servidor para o Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859888"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Funções, serviços de função e recursos não na versão de contêineres - Windows Server, Server Core 1803

> Aplica-se a: Windows Server, versão 1803

No Windows Server, versão 1803, terminarmos [reduziu o tamanho geral da imagem, contêiner Server Core **1,58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). A maneira que fizemos isso é otimizar a arquitetura e removendo coisas que você não precisa em um [contêiner de núcleo do servidor](https://docs.microsoft.com/virtualization/windowscontainers/about/). Algumas coisas que não funcionam nos contêineres de foram, alguns eram funções e recursos que ninguém vem usando. 

> [!IMPORTANT]
> Removemos essas da imagem de **contêiner** do núcleo do servidor, [núcleo do servidor propriamente dito](server-core-roles-and-services.md). 

Aqui está uma lista completa das funções e recursos removidos da imagem do contêiner de núcleo do servidor:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilitários de disco BitLocker
<br>BitLocker
<br>BITS
<br>Carregamento de BITSExtensions
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>Infraestrutura de ClientForNFS
<br>Contêineres
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>Dedup-Core
<br>DeviceHealthAttestationService
<br>DFSN-servidor
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>Serviços de arquivo
<br>FileServerVSSAgent
<br>Infraestrutura de FRS
<br>Serviços de infra-estrutura de FSRM
<br>Infraestrutura de FSRM
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>Pacote HostGuardianService
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licenciamento
<br>LightweightServer
<br>Hyper-V-Management-clientes da Microsoft
<br>Microsoft-Hyper-V-Offline
<br>Microsoft Hyper-V-Online
<br>Microsoft-Hyper-V.
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-diretiva de grupo-ServerAdminTools-Update
<br>Microsoft-Windows-subsistema-Linux
<br>Infraestrutura de MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Gui do cliente de impressão
<br>Impressão-LPDPrintService
<br>Impressão-Server-Foundation-recursos
<br>Função de servidor de impressão
<br>QWAVE
<br>RasRoutingProtocols
<br>Serviços de área de trabalho remota
<br>Acesso remoto
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>Federação do RMS
<br>Interface do usuário SBMgr
<br>Drivers ServerCore-WOW64 gerais
<br>ServerCore-Drivers-geral
<br>Infraestrutura de ServerForNFS
<br>ServerManager-Core-RSAT-Feature-Tools
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-servidor
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Armazenamento de réplica-AdminPack
<br>Réplica de armazenamento
<br>Cmdlets de PSH TPM
<br>UpdateServices banco de dados
<br>Serviços de UpdateServices
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Proxy de aplicativo Web
<br>Acesso
<br>WebEnrollmentServices
<br>O Windows Defender
<br>Backup do Windows Server
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-servidor
<br>Pacote de produto do WSS

</div>