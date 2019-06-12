---
title: Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?
description: Fornece considerações, como suporte para métodos de inicialização e outras diferenças de recursos para ajudá-lo a escolher qual geração atende às suas necessidades.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 95ececde8a1b8c591ea2baf367a93f63ee55a6e3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811990"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Se você planeja nunca carregar uma máquina virtual do Windows (VM) do local para o Microsoft Azure, geração 1 e geração 2 VMs em um formato de arquivo VHD e tiver um disco de tamanho fixo têm suporte. Ver [VMs da geração 2 no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) para saber mais sobre os recursos de geração 2 tem suportados no Azure. Para obter mais informações sobre como carregar um VHD do Windows ou VHDX, consulte [preparar um VHD do Windows ou o VHDX para carregar no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

Sua escolha para criar uma geração 1 ou a máquina virtual de geração 2 depende do sistema operacional convidado em que você deseja instalar e o método de inicialização que você deseja usar para implantar a máquina virtual. É recomendável que você crie uma máquina virtual de geração 2 para tirar proveito dos recursos como inicialização segura, a menos que uma das instruções a seguir for verdadeira:  

- O VHD que você deseja inicializar a partir de não está [compatível com o UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- Geração 2 não oferece suporte para o sistema operacional que você deseja executar na máquina virtual.  
- Geração 2 não oferece suporte para o método de inicialização que você deseja usar.  

Para obter mais informações sobre quais recursos estão disponíveis com as máquinas virtuais de geração 2, consulte [compatibilidade de recursos do Hyper-V por geração e convidado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

Você não pode alterar a geração de uma máquina virtual depois que você criou. Portanto, é recomendável que você examine as considerações aqui, bem como escolher o sistema operacional, o método de inicialização e a recursos que você deseja usar antes de escolher uma geração.  

## <a name="which-guest-operating-systems-are-supported"></a>Há suporte para quais sistemas operacionais de convidados?

A maioria dos sistemas operacionais convidados do suporte a máquinas virtuais de geração 1. Máquinas virtuais de geração 2 oferecem suporte a maioria das versões de 64 bits do Windows e versões mais atuais dos sistemas operacionais Linux e FreeBSD. Use as seções a seguir para ver qual geração de máquina virtual dá suporte ao sistema operacional convidado que deseja instalar.  

- [Suporte de sistema operacional de convidado do Windows](#windows-guest-operating-system-support)  

- [CentOS e Red Hat Enterprise Linux suporte do sistema operacional de convidado](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Suporte do sistema operacional convidado Debian](#debian-guest-operating-system-support)  

- [Suporte de sistema operacional convidado FreeBSD](#freebsd-guest-operating-system-support)  

- [Suporte do sistema operacional de convidado do Oracle Linux](#oracle-linux-guest-operating-system-support)  

- [Suporte de sistema operacional convidado SUSE](#suse-guest-operating-system-support)  

- [Suporte de sistema operacional de convidado do Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Suporte de sistema operacional de convidado do Windows

A tabela a seguir mostra quais versões de 64 bits do Windows você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.  

|versões de 64 bits do Windows|1ª geração|2ª geração|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

A tabela a seguir mostra quais versões de 32 bits do Windows você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|versões de 32 bits do Windows|1ª geração|2ª geração|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS e Red Hat Enterprise Linux suporte do sistema operacional de convidado

A tabela a seguir mostra quais versões do Red Hat Enterprise Linux \(RHEL\) e CentOS você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série do RHEL/CentOS 7. x|&#10004;|&#10004;|  
|Série do RHEL/CentOS 6. x|&#10004;|&#10004;<br />**Observação:** Só tem suporte no Windows Server 2016 e posterior.|  
|Série de 5. x do RHEL/CentOS|&#10004;| &#10006;|  

Para obter mais informações, consulte [CentOS e Red Hat Enterprise Linux máquinas virtuais no Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Suporte do sistema operacional convidado Debian  

A tabela a seguir mostra quais versões do Debian você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série de Debian 7. x|&#10004;| &#10006;|  
|Série de Debian 8. x|&#10004;|&#10004;|  

Para obter mais informações, consulte [máquinas virtuais do Debian no Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Suporte de sistema operacional convidado FreeBSD

A tabela a seguir mostra quais versões do FreeBSD você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.  

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 e 10.1|&#10004;| &#10006;|  
|9.1 FreeBSD e 9.3|&#10004;| &#10006;|  
|FreeBSD 8.4|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas de virtuais FreeBSD no Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Suporte do sistema operacional de convidado do Oracle Linux  

A tabela a seguir mostra quais versões de série de Kernel compatível do Red Hat você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.  

|Versões de série de Kernel compatível do Red Hat|1ª geração|2ª geração|  
|---------------------------------------------|----------------|----------------|  
|Série do Oracle Linux 7.x|&#10004;|&#10004;|
|Série do Oracle Linux 6. x|&#10004;| &#10006;|  

A tabela a seguir mostra quais versões do Unbreakable Enterprise Kernel você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|Versões de Unbreakable Enterprise Kernel (UEK)|1ª geração|2ª geração|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas de virtuais do Oracle Linux no Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Suporte de sistema operacional convidado SUSE

A tabela a seguir mostra quais versões do SUSE você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série do SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Série do SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Open SUSE 12.3|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas virtuais SUSE do Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Suporte de sistema operacional de convidado do Ubuntu

A tabela a seguir mostra quais versões do Ubuntu você pode usar como um sistema operacional convidado para a geração 1 e máquinas virtuais de geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 e versões posteriores|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas virtuais do Ubuntu no Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>Como é possível inicializar a máquina virtual?

A tabela a seguir mostra quais métodos são suportados pela geração 1 e máquinas virtuais de geração 2 de inicialização.  

|Método de inicialização|1ª geração|2ª geração|  
|---------------|----------------|----------------|  
|Inicialização PXE usando um adaptador de rede padrão| &#10006;|&#10004;|  
|Inicialização PXE com um adaptador de rede herdado|&#10004;| &#10006;|  
|Inicialização de um disco rígido virtual de SCSI (. VHDX) ou de DVD virtual (. ISO)| &#10006;|&#10004;|  
|Inicialização a partir do controlador IDE do disco rígido virtual (. VHD) ou de DVD virtual (. ISO)|&#10004;| &#10006;|  
|Inicialização a partir do disquete (. VFD)|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>Quais são as vantagens de usar as máquinas virtuais de 2ª geração?

Aqui estão algumas das vantagens que você obtém ao usar uma máquina virtual de 2ª geração:  
- **Inicialização segura** esse é um recurso que verifica se o carregador de inicialização é assinado por uma autoridade confiável no banco de dados UEFI para ajudar a impedir o firmware não autorizado, sistemas operacionais ou drivers UEFI em execução no momento da inicialização. A Inicialização Segura é habilitada por padrão em máquinas virtuais da 2ª geração. Se você precisar executar um sistema operacional convidado que não é suportado pela inicialização segura, você pode desabilitá-lo após a criação da máquina virtual.  Para saber mais, confira [Inicialização Segura](https://technet.microsoft.com/library/dn486875.aspx).  

    Para máquinas de virtuais de Linux de 2ª geração de inicialização segura, você precisa escolher o modelo de inicialização segura de autoridade de certificação UEFI quando você cria a máquina virtual.  

- **Volume de inicialização maior** o volume de inicialização de máximo de máquinas virtuais de geração 2 é 64 TB. Esse é o tamanho máximo em disco com suporte por um. VHDX. Para máquinas virtuais de geração 1, o volume de inicialização máximo é de 2TB para um. VHDX e 2040GB para um. VHD. Para obter mais informações, consulte [Hyper-V Virtual Hard Disk Format Overview](https://technet.microsoft.com/library/hh831446.aspx).  

  Você também pode ver uma ligeira melhoria nos tempos de inicialização e instalação de máquina virtual com máquinas virtuais de geração 2.

## <a name="whats-the-difference-in-device-support"></a>O que é a diferença no suporte a dispositivos?

A tabela a seguir compara os dispositivos disponíveis entre a geração 1 e máquinas virtuais de geração 2.  

|Dispositivo da 1ª geração|Substituto da 2ª geração|Aprimoramentos da 2ª geração|  
|-----------------------|----------------------------|-----------------------------|  
|Controlador IDE|Controlador virtual SCSI|Inicialização com .vhdx (tamanho máximo de 64 TB e funcionalidade de redimensionamento online)|  
|CD-ROM IDE|CD-ROM virtual SCSI|Suporte para até 64 dispositivos de DVD SCSI por controlador SCSI.|  
|BIOS herdado|Firmware UEFI|Inicialização Segura|  
|Adaptador de rede herdado|Adaptador de rede sintético|Inicialização de rede com IPv4 e IPv6|  
|Controlador DMA (acesso direto à memória) e de disquete|Sem suporte a controlador de disquete|N/D|  
|UART (universal asynchronous receiver-transmitter) para portas COM (Component Object Model)|UART opcional para depuração|Mais rápido e mais confiável|  
|Controlador de teclado i8042|Entrada baseada em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Teclado PS/2|Teclado baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Mouse PS/2|Mouse baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Vídeo S3|Vídeo baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Barramento PCI|Não é mais necessário|N/D|  
|PIC (controlador de interrupção programável)|Não é mais necessário|N/D|  
|PIT (temporizador de intervalo programável)|Não é mais necessário|N/D|  
|Superdispositivo de E/S|Não é mais necessário|N/D|  

## <a name="more-about-generation-2-virtual-machines"></a>Mais sobre máquinas virtuais de geração 2

Aqui estão algumas dicas adicionais sobre como usar máquinas virtuais de geração 2.

### <a name="attach-or-add-a-dvd-drive"></a>Anexar ou adicionar uma unidade de DVD

- Você não pode anexar uma unidade de CD ou DVD física a uma máquina virtual de geração 2. A unidade de DVD virtual de máquinas virtuais da 2ª geração tem suporte apenas para arquivos de imagem ISO. Para criar um arquivo de imagem ISO de um ambiente Windows, você pode usar a ferramenta de linha de comando Oscdimg. Para obter mais informações, veja [Opções de linha de comando de Oscdimg‎](https://msdn.microsoft.com/library/hh824847.aspx).
- Quando você cria uma nova máquina virtual com o cmdlet New-VM do Windows PowerShell, a máquina virtual geração 2 não tem uma unidade de DVD. Você pode adicionar uma unidade de DVD enquanto a máquina virtual está em execução.

### <a name="use-uefi-firmware"></a>Use o firmware UEFI

- Inicialização segura ou firmware UEFI não é necessária no host físico do Hyper-V. Hyper-V fornece o firmware virtual para máquinas virtuais, o que é independente do que está no host Hyper-V.
- Firmware UEFI em uma máquina virtual de geração 2 não suporta o modo de instalação da inicialização segura.
- Não damos suporte a execução de um shell UEFI ou outros aplicativos UEFI em uma máquina virtual de geração 2. Será tecnicamente possível usar um shell UEFI ou aplicativos UEFI que não sejam da Microsoft, se eles forem compilados diretamente das fontes. Se esses aplicativos não são adequadamente assinados digitalmente, você deve desabilitar a inicialização segura para a máquina virtual.

### <a name="work-with-vhdx-files"></a>Trabalhar com arquivos VHDX

- Você pode redimensionar um arquivo VHDX que contém o volume de inicialização para uma máquina virtual de 2ª geração enquanto a máquina virtual está em execução.
- Não oferecer suporte nem recomendável que você crie um arquivo VHDX inicializável por geração 1 e máquinas virtuais de geração 2.  
- A geração da máquina virtual é uma propriedade da máquina, não do disco rígido virtual. Portanto, você não pode determinar se um arquivo VHDX foi criado por uma geração 1 ou uma máquina virtual de geração 2.  
- Um arquivo VHDX criado com uma geração 2 máquina de virtual pode ser anexado ao controlador IDE ou ao controlador de SCSI de uma máquina virtual de geração 1. No entanto, quando se trata de um arquivo VHDX inicializável, não inicializar a máquina virtual de geração 1.

### <a name="use-ipv6-instead-of-ipv4"></a>Use o IPv6, em vez de IPv4

Por padrão, máquinas virtuais da 2ª geração usam IPv4. Para usar IPv6, em vez disso, execute as [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet do Windows PowerShell. Por exemplo, o comando a seguir define o protocolo preferencial para IPv6 para uma máquina virtual chamada TestVM:  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Adicionar uma porta COM para depuração de kernel

As portas COM não estão disponíveis em máquinas virtuais de geração 2 até você adicioná-los. Você pode fazer isso com o Windows PowerShell ou o Windows Management Instrumentation (WMI). Estas etapas mostram como fazer isso com o Windows PowerShell.

Para adicionar uma porta COM:  

1. Desabilite a Inicialização Segura. Depuração de kernel não é compatível com a inicialização segura. Verifique se a máquina virtual está desligada e usar o [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet. Por exemplo, o comando a seguir desabilita a inicialização segura na máquina virtual TestVM:  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Adicione uma porta COM. Use o [Set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) para fazer isso. Por exemplo, o comando a seguir configura a primeira porta COM na máquina virtual TestVM, para se conectar ao pipe nomeado, TestPipe, no computador local:  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> As portas COM configuradas não estão listadas nas configurações de uma máquina virtual no Gerenciador do Hyper-V.

## <a name="see-also"></a>Consulte também  

- [Linux e máquinas de virtuais FreeBSD no Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usar os recursos locais na máquina virtual de Hyper-V com VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Planejar a escalabilidade do Hyper-V no Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
