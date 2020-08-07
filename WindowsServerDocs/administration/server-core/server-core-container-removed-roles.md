---
title: Funções, serviços de função e recursos que não estão em contêineres do Server Core-Windows Server, versão 1803
description: Saiba mais sobre as funções e os recursos que removemos da imagem de contêiner do Server Core para o Windows Server.
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: f577ebd805e5373a10dd43a3d5054f92d4881c7d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895911"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Funções, serviços de função e recursos que não estão em contêineres do Server Core-Windows Server, versão 1803

> Aplica-se a: Windows Server, versão 1803

No Windows Server, versão 1803, [reduzimos o tamanho geral da imagem de contêiner do Server Core para **1,58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). A maneira como fizemos isso é otimizando a arquitetura e removendo as coisas que você não precisa em um [contêiner do Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Alguns eram coisas que não funcionavam em contêineres, alguns eram funções e recursos que ninguém está usando.

> [!IMPORTANT]
> Nós os removemos da imagem de **contêiner** do Server Core, e não do [próprio Server Core](server-core-roles-and-services.md).

Aqui está a lista completa de recursos e funções removidas da imagem de contêiner do Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>BitLocker-utilitários
<br>BitLocker
<br>BITS
<br>BITSExtensions-carregar
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>Certificadosservices
<br>ClientForNFS-infraestrutura
<br>Contêineres
<br>CoreFileServer
<br>DataCenterBridging-LLDP-ferramentas
<br>DataCenterBridging
<br>Eliminação de duplicatas-núcleo
<br>DeviceHealthAttestationService
<br>DFSN-servidor
<br>DFSR-Infrastructure-edição
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>Serviços de arquivos
<br>FileServerVSSAgent
<br>FRS-infraestrutura
<br>FSRM-infraestrutura-serviços
<br>FSRM – infraestrutura
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-Package
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licenças
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-clientes
<br>Microsoft-Hyper-V-offline
<br>Microsoft-Hyper-V-online
<br>Microsoft-Hyper-V
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-Subsystem-Linux
<br>MSRDC-infraestrutura
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Imprimindo-cliente-gui
<br>Imprimindo-LPDPrintService
<br>Imprimindo-Server-Foundation-Features
<br>Imprimindo-servidor-função
<br>QWAVE
<br>RasRoutingProtocols
<br>Serviços de área de trabalho remota
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-função
<br>RightsManagementServices
<br>RMS-Federação
<br>SBMgr-interface do usuário
<br>ServerCore-drivers-geral-WOW64
<br>ServerCore-drivers-geral
<br>ServerForNFS-infraestrutura
<br>ServerManager-Core-RSAT-recurso-ferramentas
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
<br>Armazenamento-réplica-AdminPack
<br>Armazenamento-réplica
<br>TPM-PSH-cmdlets
<br>Updateservices-banco de dados
<br>Updateservices-Services
<br>Updateservices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-função completa
<br>Aplicativo Web-proxy
<br>WebAccess
<br>Enrollservices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-servidor
<br>WSS-pacote-produto

</div>