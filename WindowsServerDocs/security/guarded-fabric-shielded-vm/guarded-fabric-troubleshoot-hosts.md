---
title: Solucionando problemas do serviço guardião de host
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: ec885670ca6808e89c63848781c4ff3dc27799b8
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371356"
---
# <a name="troubleshooting-guarded-hosts"></a>Solucionando problemas de hosts protegidos

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico descreve as resoluções para problemas comuns encontrados ao implantar ou operar um host Hyper-V protegido em sua malha protegida.
Se você não tiver certeza da natureza do seu problema, primeiro tente executar o [diagnóstico de malha protegida](guarded-fabric-troubleshoot-diagnostics.md) em seus hosts Hyper-V para restringir as possíveis causas.

## <a name="guarded-host-feature"></a>Recurso de host protegido

Se você estiver tendo problemas com o host Hyper-V, primeiro verifique se o recurso de **suporte do Hyper-v do guardião de host** está instalado.
Sem esse recurso, o host Hyper-V não terá algumas definições de configuração críticas e o software que permitirá que ele passe o atestado e provisione VMs blindadas.

Para verificar se o recurso está instalado, use Gerenciador do Servidor ou execute o seguinte comando em uma janela do PowerShell com privilégios elevados:

```powershell
Get-WindowsFeature HostGuardian
```

Se o recurso não estiver instalado, instale-o com o seguinte comando do PowerShell:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Falhas de atestado

Se um host não passar no atestado com o serviço guardião de host, ele não poderá executar VMs blindadas.
A saída de [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) nesse host mostrará as informações sobre o motivo da falha do atestado do host.

A tabela a seguir explica os valores que podem aparecer no campo **AttestationStatus** e próximas etapas possíveis, se apropriado.

AttestationStatus         | Explicação
--------------------------|------------
Expirado                   | O host passou pelo atestado anteriormente, mas o certificado de integridade que ele foi emitido expirou. Verifique se o host e a hora do HGS estão em sincronia.
InsecureHostConfiguration | O host não passou no atestado porque ele não estava em conformidade com as políticas de atestado configuradas no HGS. Consulte a tabela AttestationSubStatus para obter mais informações.
NotConfigured             | O host não está configurado para usar um HGS para atestado e proteção de chave. Ele é configurado para o modo local, em vez disso. Se esse host estiver em uma malha protegida, use [set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) para fornecê-lo com as URLs para seu servidor HgS.
Aprovado                    | O host passou por atestado.
TransientError            | A última tentativa de atestado falhou devido a uma rede, serviço ou outro erro temporário. Repita a última operação.
TpmError                  | O host não pôde concluir sua última tentativa de atestado devido a um erro com o TPM. Consulte os logs do TPM para obter mais informações.
UnauthorizedHost          | O host não passou no atestado porque não foi autorizado a executar VMs blindadas. Verifique se o host pertence a um grupo de segurança confiável pelo HGS para executar VMs blindadas.
Desconhecido                   | O host ainda não tentou atestar com o HGS.

Quando **AttestationStatus** é relatado como **InsecureHostConfiguration**, um ou mais motivos serão preenchidos no campo **AttestationSubStatus** .
A tabela a seguir explica os possíveis valores para AttestationSubStatus e dicas sobre como resolver o problema.

AttestationSubStatus       | O que significa e o que fazer
---------------------------|-------------------------------
BitLocker                  | O volume do sistema operacional do host não é criptografado pelo BitLocker. Para resolver isso, [habilite o BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) no volume do sistema operacional ou [desabilite a política do BitLocker no HgS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | O host não está configurado para usar uma política de integridade de código ou não está usando uma política confiável para o servidor HGS. Verifique se uma política de integridade de código foi configurada, se o host foi reiniciado e se a política está registrada com o servidor HGS. Para obter mais informações, consulte [criar e aplicar uma política de integridade de código](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | O host está configurado para permitir despejos ou despejos de memória ao vivo, o que não é permitido por suas políticas HGS. Para resolver isso, desabilite os despejos no host.
DumpEncryption             | O host é configurado para permitir despejos ou despejos de memória ao vivo, mas não criptografa esses despejos. Desabilite os despejos no host ou [Configure a criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | O host é configurado para permitir e criptografar despejos, mas não está usando um certificado conhecido por HGS para criptografá-los. Para resolver isso, [atualize a chave de criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) no host ou [Registre a chave com o HgS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | O host retomou-se de um estado de suspensão ou hibernação. Reinicie o host para permitir uma inicialização limpa e completa.
HibernationEnabled         | O host está configurado para permitir a hibernação sem criptografar o arquivo de hibernação, o que não é permitido por suas políticas HGS. Desabilite a hibernação e reinicie o host ou [Configure a criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | O host não está configurado para usar uma política de integridade de código imposta por hipervisor. Verifique se a integridade do código está habilitada, configurada e imposta pelo hipervisor. Consulte o [Guia de implantação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) para obter mais informações.
IOMMU                      | Os recursos de segurança baseados em virtualização do host não estão configurados para exigir um dispositivo IOMMU para proteção contra ataques de acesso direto à memória, conforme exigido pelas suas políticas HGS. Verifique se o host tem uma IOMMU, se está habilitado e se o Device Guard está [configurado para exigir proteções DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) ao iniciar o vbs.
PagefileEncryption         | A criptografia do arquivo de paginação não está habilitada no host. Para resolver isso, execute `fsutil behavior set encryptpagingfile 1` para habilitar a criptografia de arquivo de paginação. Para obter mais informações, consulte [fsutil Behavior](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | A inicialização segura não está habilitada neste host ou não está usando o modelo de inicialização segura da Microsoft. [Habilite a inicialização segura](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) com o modelo de inicialização segura da Microsoft para resolver esse problema.
SecureBootSettings         | A linha de base do TPM neste host não corresponde a nenhuma das confianças do HGS. Isso pode ocorrer quando suas autoridades de inicialização de UEFI, a variável do DBX, o sinalizador de depuração ou as políticas de inicialização segura personalizadas são alteradas pela instalação de novo hardware ou software. Se você confiar na configuração atual de hardware, firmware e software deste computador, poderá [capturar uma nova linha de base do TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) e [registrá-la com o HgS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | O log do TCG (linha de base do TPM) não pode ser obtido ou verificado. Isso pode indicar um problema com o firmware do host, o TPM ou outros componentes de hardware. Se o host estiver configurado para tentar a inicialização PXE antes de inicializar o Windows, um NBP (programa de inicialização líquida) desatualizado também poderá causar esse erro. Verifique se todos os NBPs estão atualizados quando a inicialização PXE está habilitada.
VirtualSecureMode          | Os recursos de segurança baseados em virtualização não estão em execução no host. Verifique se o VBS está habilitado e se o seu sistema atende aos [recursos de segurança da plataforma](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)configurada. Consulte a [documentação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) para obter mais informações sobre os requisitos de vbs.

## <a name="modern-tls"></a>TLS moderno

Se você tiver implantado uma política de grupo ou configurado seu host Hyper-V para evitar o uso do TLS 1,0, poderá encontrar "o cliente do serviço guardião de host não pôde desencapsular um protetor de chave em nome de um processo de chamada" ao tentar iniciar uma VM blindada.
Isso ocorre devido a um comportamento padrão no .NET 4,6 em que a versão do TLS padrão do sistema não é considerada ao negociar versões de TLS com suporte com o servidor HGS.

Para contornar esse comportamento, execute os dois comandos a seguir para configurar o .NET para usar as versões padrão do TLS do sistema para todos os aplicativos .NET.

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> A configuração de versões padrão do TLS do sistema afetará todos os aplicativos .NET em seu computador. Certifique-se de testar as chaves do registro em um ambiente isolado antes de implantá-las em suas máquinas de produção.

Para obter mais informações sobre o .NET 4,6 e o TLS 1,0, consulte [resolvendo o problema de tls 1,0, 2ª edição](https://docs.microsoft.com/security/solving-tls1-problem).
