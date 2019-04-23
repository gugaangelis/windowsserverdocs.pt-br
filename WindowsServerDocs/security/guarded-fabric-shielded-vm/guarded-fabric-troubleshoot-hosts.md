---
title: Solução de problemas de serviço guardião de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851367"
---
# <a name="troubleshooting-guarded-hosts"></a>Solução de problemas de Hosts protegidos

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico descreve as soluções de problemas comuns encontrados durante a implantação ou operando de um host Hyper-V protegido em sua malha protegida.
Se você não tiver certeza da natureza do problema, primeiro tente executar o [protegidos do diagnóstico do fabric](guarded-fabric-troubleshoot-diagnostics.md) em seus hosts do Hyper-V para reduzir o potencial faz com que.

## <a name="guarded-host-feature"></a>Protegido de recurso de Host

Se você estiver tendo problemas com seu host do Hyper-V, primeiro certifique-se de que o **suporte de Hyper-V de guardião de Host** recurso é instalado.
Sem esse recurso, o host Hyper-V poderá perder algumas definições de configuração crítica e software que permitem que ele passe Atestado e provisionamento de VMs blindadas.

Para verificar se o recurso está instalado, use o Gerenciador do servidor ou execute o seguinte comando em uma janela elevada do PowerShell:

```powershell
Get-WindowsFeature HostGuardian
```

Se o recurso não estiver instalado, instale-o com o seguinte comando do PowerShell:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Falhas de Atestado

Se um host não passar o Atestado com o serviço guardião de Host, será possível executar VMs blindadas.
A saída de [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) nesse host mostrará informações sobre o motivo que hospedam falha Atestado.

A tabela a seguir explica os valores que podem aparecer na **AttestationStatus** campo e próximas etapas possíveis, se apropriado.

AttestationStatus         | Explicação
--------------------------|------------
Expirado                   | O host passado Atestado anteriormente, mas o certificado de integridade que ele foi emitido expirou. Verifique se o host e o tempo HGS estão em sincronia.
InsecureHostConfiguration | O host não passou Atestado porque ele não é compatível com as políticas de Atestado configuradas no HGS. Consulte a tabela de AttestationSubStatus para obter mais informações.
NotConfigured             | O host não está configurado para usar um HGS de Atestado e proteção de chave. Ele é configurado para o modo local, em vez disso. Se esse host está em uma malha protegida, use [Set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) para fornecê-la com as URLs para o servidor HGS.
Passado                    | O host passado Atestado.
TransientError            | A última tentativa de Atestado falhou devido a uma rede, serviço ou outro erro temporário. Repita a última operação.
TpmError                  | O host não foi possível concluir a sua última tentativa de Atestado devido a um erro com o TPM. Consulte os logs TPM para obter mais informações.
UnauthorizedHost          | O host não passou Atestado porque não foi autorizado a executar VMs blindadas. Certifique-se de que o host pertence a um grupo de segurança confiável pelo HGS para executar VMs blindadas.
Desconhecido                   | O host não tentou fazer atestar ainda com HGS.


Quando **AttestationStatus** será relatado como **InsecureHostConfiguration**, um ou mais motivos serão preenchidos na **AttestationSubStatus** campo.
A tabela a seguir explica os valores possíveis para AttestationSubStatus e dicas sobre como resolver o problema.

AttestationSubStatus       | O que significa que e o que fazer
---------------------------|-------------------------------
BitLocker                  | Volume do sistema operacional do host não é criptografado pelo BitLocker. Para resolver o problema, [habilitar o BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) no volume do sistema operacional ou [desabilitar a política do BitLocker em HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | O host não está configurado para usar uma política de integridade de código ou não está usando uma política confiável pelo servidor HGS. Certifique-se de uma política de integridade de código tiver sido configurada, que o host foi reiniciado, e a política é registrada com o servidor HGS. Para obter mais informações, consulte [criar e aplicar uma política de integridade de código](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | O host está configurado para permitir que os despejos de memória ou ao vivo despejos de memória, que não é permitido por suas políticas HGS. Para resolver esse problema, desabilite os despejos de memória no host.
DumpEncryption             | O host está configurado para permitir que os despejos de memória ou despejos de memória em tempo real, mas não criptografa os despejos. Desabilite os despejos de memória no host ou [configurar a criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | O host está configurado para permitir e criptografar os despejos de memória, mas não está usando um certificado conhecido para HGS para criptografá-los. Para resolver o problema, [atualizar a chave de criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) no host ou [registre a chave com o HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | O host de retomada de um estado de suspensão ou hibernação. Reinicie o host para permitir uma inicialização limpa e completa.
HibernationEnabled         | O host está configurado para permitir que o modo de hibernação sem criptografar o arquivo de hibernação, o que não é permitido por suas políticas HGS. Desabilite a hibernação e reinicie o host, ou [configurar a criptografia de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | O host não está configurado para usar uma política de integridade de código aplicadas pelo hipervisor. Verifique se que a integridade do código é habilitada, configurada e imposta pelo hipervisor. Consulte a [guia de implantação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) para obter mais informações.
Iommu                      | Recursos de segurança baseada em virtualização do host não estão configurados para exigir um dispositivo IOMMU para proteção contra ataques de acesso direto à memória, conforme exigido por suas políticas HGS. Verifique se o host tem um IOMMU, o que ele está habilitado e que o Device Guard é [configurado para exigir as proteções de DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) ao iniciar o VBS.
PagefileEncryption         | Criptografia de arquivo de página não está habilitada no host. Para resolver esse problema, execute `fsutil behavior set encryptpagingfile 1` para habilitar a criptografia de arquivo da página. Para obter mais informações, consulte [comportamento do fsutil](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | Inicialização segura é qualquer um que não são habilitados neste host ou não usando o modelo de inicialização segura do Microsoft. [Habilitar a inicialização segura](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) com o modelo de inicialização segura do Microsoft para resolver esse problema.
SecureBootSettings         | A linha de base TPM neste host não corresponde a qualquer um dos confiável pelo HGS. Isso pode ocorrer quando seu UEFI autoridades de lançamento, DBX variável, o sinalizador de depuração ou políticas personalizadas de inicialização segura são alteradas por meio da instalação de novo hardware ou software. Se você confia que o hardware atual, firmware e configuração de software desse computador, você pode [capturar uma nova linha de base TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) e [registrá-lo com o HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | O log do TCG (linha de base TPM) não pode ser obtido ou verificado. Isso pode indicar um problema com o firmware do host, o TPM ou outros componentes de hardware. Se o host está configurado para tentar fazer a inicialização PXE antes de inicializar o Windows, um desatualizados Net inicialização NBP (programa) também podem causar esse erro. Certifique-se de que todos os NBPs estão atualizados quando a inicialização PXE está habilitada.
VirtualSecureMode          | Recursos de segurança com base em virtualização não estão em execução no host. Certifique-se de VBS está habilitado e o sistema atende configurado [recursos de segurança de plataforma](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features). Consulte a [documentação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) para obter mais informações sobre os requisitos de VBS.
