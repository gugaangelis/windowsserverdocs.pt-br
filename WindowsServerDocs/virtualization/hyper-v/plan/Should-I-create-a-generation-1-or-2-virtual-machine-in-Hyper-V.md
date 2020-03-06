---
title: Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?
description: Fornece considerações como métodos de inicialização com suporte e outras diferenças de recursos para ajudá-lo a escolher qual geração atende às suas necessidades.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: fce9b45f538b0d506b621b888d413c99590b1362
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370620"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?

>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Se você planeja carregar as VMs (máquinas virtuais) do Windows locais para Microsoft Azure, as VMs de geração 1 e de geração 2 no formato de arquivo VHD e têm suporte para um disco de tamanho fixo. Consulte [VMs de geração 2 no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) para saber mais sobre os recursos de geração 2 com suporte no Azure. Para obter mais informações sobre como carregar um VHD ou VHDX do Windows, consulte [preparar um VHD do Windows ou vhdx para carregar no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

Sua escolha para criar uma máquina virtual de geração 1 ou geração 2 depende do sistema operacional convidado que você deseja instalar e do método de inicialização que você deseja usar para implantar a máquina virtual. Recomendamos que você crie uma máquina virtual de geração 2 para aproveitar os recursos como a inicialização segura, a menos que uma das seguintes instruções seja verdadeira:  

- O VHD do qual você deseja inicializar não é [compatível com UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- A geração 2 não dá suporte ao sistema operacional que você deseja executar na máquina virtual.  
- A geração 2 não oferece suporte ao método de inicialização que você deseja usar.  

Para obter mais informações sobre quais recursos estão disponíveis com máquinas virtuais de geração 2, consulte [compatibilidade de recursos do Hyper-V por geração e convidado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

Você não pode alterar a geração de uma máquina virtual depois de tê-la criado. Portanto, recomendamos que você examine as considerações aqui, bem como escolha o sistema operacional, o método de inicialização e os recursos que deseja usar antes de escolher uma geração.  

## <a name="which-guest-operating-systems-are-supported"></a>Quais sistemas operacionais convidados têm suporte?

As máquinas virtuais de geração 1 dão suporte à maioria dos sistemas operacionais convidados. As máquinas virtuais de geração 2 oferecem suporte à maioria das versões de 64 bits do Windows e mais versões atuais dos sistemas operacionais Linux e FreeBSD. Use as seções a seguir para ver qual geração de máquina virtual dá suporte ao sistema operacional convidado que você deseja instalar.  

- [Suporte ao sistema operacional convidado do Windows](#windows-guest-operating-system-support)  

- [Suporte ao sistema operacional de convidado CentOS e Red Hat Enterprise Linux](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Suporte ao sistema operacional de convidado Debian](#debian-guest-operating-system-support)  

- [Suporte a sistemas operacionais de convidado FreeBSD](#freebsd-guest-operating-system-support)  

- [Suporte ao sistema operacional convidado Oracle Linux](#oracle-linux-guest-operating-system-support)  

- [Suporte do sistema operacional de convidado SUSE](#suse-guest-operating-system-support)  

- [Suporte ao sistema operacional de convidado Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Suporte ao sistema operacional convidado do Windows

A tabela a seguir mostra quais versões de 64 bits do Windows você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.  

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

A tabela a seguir mostra quais versões de 32 bits do Windows você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|versões de 32 bits do Windows|1ª geração|2ª geração|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>Suporte ao sistema operacional de convidado CentOS e Red Hat Enterprise Linux

A tabela a seguir mostra quais versões do Red Hat Enterprise Linux \(RHEL\) e CentOS você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série RHEL/CentOS 7. x|&#10004;|&#10004;|  
|Série RHEL/CentOS 6. x|&#10004;|&#10004;<br />**Observação:** Com suporte apenas no Windows Server 2016 e posterior.|  
|Série RHEL/CentOS 5. x|&#10004;| &#10006;|  

Para obter mais informações, consulte [CentOS e Red Hat Enterprise Linux máquinas virtuais no Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Suporte ao sistema operacional de convidado Debian  

A tabela a seguir mostra quais versões do Debian você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série Debian 7. x|&#10004;| &#10006;|  
|Série Debian 8. x|&#10004;|&#10004;|  

Para obter mais informações, consulte [máquinas virtuais Debian no Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Suporte a sistemas operacionais de convidado FreeBSD

A tabela a seguir mostra quais versões do FreeBSD você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.  

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 e 10,1|&#10004;| &#10006;|  
|FreeBSD 9,1 e 9,3|&#10004;| &#10006;|  
|FreeBSD 8,4|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas virtuais do FreeBSD no Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Suporte ao sistema operacional convidado Oracle Linux  

A tabela a seguir mostra quais versões da série kernel compatível com Red Hat você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.  

|Versões da série kernel compatíveis com Red Hat|1ª geração|2ª geração|  
|---------------------------------------------|----------------|----------------|  
|Série Oracle Linux 7. x|&#10004;|&#10004;|
|Série Oracle Linux 6. x|&#10004;| &#10006;|  

A tabela a seguir mostra quais versões de um kernel corporativo inquebrável você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|Versões do UEK (inquebrable Enterprise kernel)|1ª geração|2ª geração|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Para obter mais informações, consulte [Oracle Linux máquinas virtuais no Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Suporte do sistema operacional de convidado SUSE

A tabela a seguir mostra quais versões do SUSE você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Série 12 SUSE Linux Enterprise Server|&#10004;|&#10004;|  
|Série SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Abrir o SUSE 12,3|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas virtuais SuSE no Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Suporte ao sistema operacional de convidado Ubuntu

A tabela a seguir mostra quais versões do Ubuntu você pode usar como um sistema operacional convidado para máquinas virtuais de geração 1 e geração 2.

|Versões do sistema operacional|1ª geração|2ª geração|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14, 4 e versões posteriores|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Para obter mais informações, consulte [máquinas virtuais do Ubuntu no Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>Como posso inicializar a máquina virtual?

A tabela a seguir mostra quais métodos de inicialização são suportados pelas máquinas virtuais de geração 1 e geração 2.  

|Método de inicialização|1ª geração|2ª geração|  
|---------------|----------------|----------------|  
|Inicialização PXE usando um adaptador de rede padrão| &#10006;|&#10004;|  
|Inicialização PXE usando um adaptador de rede herdado|&#10004;| &#10006;|  
|Inicialize de um disco rígido virtual SCSI (. VHDX) ou DVD virtual (. ATUALIZALIZAÇÃO| &#10006;|&#10004;|  
|Inicialize do disco rígido virtual do controlador IDE (. VHD) ou DVD virtual (. ATUALIZALIZAÇÃO|&#10004;| &#10006;|  
|Inicialize do disquete (. VFD|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>Quais são as vantagens de usar máquinas virtuais de geração 2?

Aqui estão algumas das vantagens que você obtém ao usar uma máquina virtual de geração 2:  
- **Inicialização segura** Esse é um recurso que verifica se o carregador de inicialização está assinado por uma autoridade confiável no banco de dados UEFI para ajudar a impedir que firmware, sistemas operacionais ou drivers UEFI não autorizados sejam executados no momento da inicialização. A Inicialização Segura é habilitada por padrão em máquinas virtuais da 2ª geração. Se precisar executar um sistema operacional convidado que não tenha suporte da inicialização segura, você poderá desabilitá-lo após a criação da máquina virtual.  Para saber mais, confira [Inicialização Segura](https://technet.microsoft.com/library/dn486875.aspx).  

    Para proteger máquinas virtuais Linux de geração de inicialização, você precisa escolher o modelo de inicialização segura de AC UEFI ao criar a máquina virtual.  

- **Volume de inicialização maior** O volume de inicialização máximo para máquinas virtuais de geração 2 é de 64 TB. Este é o tamanho máximo de disco suportado por um. VHDX. Para máquinas virtuais de geração 1, o volume de inicialização máximo é de 2TB para um. VHDX e 2040GB para um. VHD. Para obter mais informações, consulte [visão geral do formato de disco rígido virtual do Hyper-V](https://technet.microsoft.com/library/hh831446.aspx).  

  Você também pode ver uma pequena melhoria na inicialização da máquina virtual e nos tempos de instalação com máquinas virtuais de geração 2.

## <a name="whats-the-difference-in-device-support"></a>Qual é a diferença no suporte a dispositivos?

A tabela a seguir compara os dispositivos disponíveis entre as máquinas virtuais de geração 1 e de geração 2.  

|Dispositivo da 1ª geração|Substituto da 2ª geração|Aprimoramentos da 2ª geração|  
|-----------------------|----------------------------|-----------------------------|  
|Controlador IDE|Controlador virtual SCSI|Inicialização com .vhdx (tamanho máximo de 64 TB e funcionalidade de redimensionamento online)|  
|CD-ROM IDE|CD-ROM virtual SCSI|Suporte para até 64 dispositivos de DVD SCSI por controlador SCSI.|  
|BIOS herdado|Firmware UEFI|Inicialização Segura|  
|Adaptador de rede herdado|Adaptador de rede sintético|Inicialização de rede com IPv4 e IPv6|  
|Controlador DMA (acesso direto à memória) e de disquete|Sem suporte a controlador de disquete|{1&gt;N/A&lt;1}|  
|UART (universal asynchronous receiver-transmitter) para portas COM (Component Object Model)|UART opcional para depuração|Mais rápido e mais confiável|  
|Controlador de teclado i8042|Entrada baseada em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Teclado PS/2|Teclado baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Mouse PS/2|Mouse baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Vídeo S3|Vídeo baseado em software|Usa menos recursos por não possuir emulação. Também reduz a superfície de ataque do sistema operacional convidado.|  
|Barramento PCI|Não é mais necessário|{1&gt;N/A&lt;1}|  
|PIC (controlador de interrupção programável)|Não é mais necessário|{1&gt;N/A&lt;1}|  
|PIT (temporizador de intervalo programável)|Não é mais necessário|{1&gt;N/A&lt;1}|  
|Superdispositivo de E/S|Não é mais necessário|{1&gt;N/A&lt;1}|  

## <a name="more-about-generation-2-virtual-machines"></a>Mais sobre máquinas virtuais de geração 2

Aqui estão algumas dicas adicionais sobre como usar máquinas virtuais de geração 2.

### <a name="attach-or-add-a-dvd-drive"></a>Anexar ou adicionar uma unidade de DVD

- Não é possível anexar uma unidade de CD ou DVD física a uma máquina virtual de geração 2. A unidade de DVD virtual de máquinas virtuais da 2ª geração tem suporte apenas para arquivos de imagem ISO. Para criar um arquivo de imagem ISO de um ambiente Windows, você pode usar a ferramenta de linha de comando Oscdimg. Para obter mais informações, veja [Opções de linha de comando de Oscdimg‎](https://msdn.microsoft.com/library/hh824847.aspx).
- Quando você cria uma nova máquina virtual com o cmdlet New-VM do Windows PowerShell, a máquina virtual de geração 2 não tem uma unidade de DVD. Você pode adicionar uma unidade de DVD enquanto a máquina virtual está em execução.

### <a name="use-uefi-firmware"></a>Usar o firmware UEFI

- O firmware de inicialização segura ou UEFI não é necessário no host físico do Hyper-V. O Hyper-V fornece o firmware virtual para máquinas virtuais que é independente do que está no host do Hyper-V.
- O firmware UEFI em uma máquina virtual de geração 2 não dá suporte ao modo de instalação para inicialização segura.
- Não há suporte para a execução de um shell UEFI ou outros aplicativos UEFI em uma máquina virtual de geração 2. Será tecnicamente possível usar um shell UEFI ou aplicativos UEFI que não sejam da Microsoft, se eles forem compilados diretamente das fontes. Se esses aplicativos não forem assinados digitalmente de forma adequada, você deverá desabilitar a inicialização segura para a máquina virtual.

### <a name="work-with-vhdx-files"></a>Trabalhar com arquivos VHDX

- Você pode redimensionar um arquivo VHDX que contém o volume de inicialização para uma máquina virtual de geração 2 enquanto a máquina virtual está em execução.
- Não damos suporte ou recomendamos que você crie um arquivo VHDX que seja inicializável para máquinas virtuais de geração 1 e de geração 2.  
- A geração da máquina virtual é uma propriedade da máquina, não do disco rígido virtual. Portanto, você não pode saber se um arquivo VHDX foi criado por uma máquina virtual de geração 1 ou de geração 2.  
- Um arquivo VHDX criado com uma máquina virtual de geração 2 pode ser anexado ao controlador IDE ou ao controlador SCSI de uma máquina virtual de geração 1. No entanto, se esse for um arquivo VHDX inicializável, a máquina virtual de geração 1 não será inicializada.

### <a name="use-ipv6-instead-of-ipv4"></a>Usar IPv6 em vez de IPv4

Por padrão, máquinas virtuais da 2ª geração usam IPv4. Para usar o IPv6 em vez disso, execute o cmdlet [set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) do Windows PowerShell. Por exemplo, o comando a seguir define o protocolo preferencial como IPv6 para uma máquina virtual chamada TestVM:  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Adicionar uma porta COM para depuração de kernel

As portas COM não estão disponíveis em máquinas virtuais de geração 2 até você adicioná-las. Você pode fazer isso com o Windows PowerShell ou Instrumentação de Gerenciamento do Windows (WMI). Estas etapas mostram como fazer isso com o Windows PowerShell.

Para adicionar uma porta COM:  

1. Desabilite a Inicialização Segura. A depuração de kernel não é compatível com a inicialização segura. Verifique se a máquina virtual está em um estado desligado e, em seguida, use o cmdlet [set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) . Por exemplo, o comando a seguir desabilita a inicialização segura na máquina virtual TestVM:  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Adicione uma porta COM. Use o cmdlet [set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) para fazer isso. Por exemplo, o comando a seguir configura a primeira porta COM na máquina virtual, TestVM, para se conectar ao pipe nomeado, TestPipe, no computador local:  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> As portas COM configuradas não estão listadas nas configurações de uma máquina virtual no Gerenciador do Hyper-V.

## <a name="see-also"></a>Consulte também  

- [Máquinas Virtuais do Linux e FreeBSD no Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usar recursos locais na máquina virtual do Hyper-V com VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Planejar a escalabilidade do Hyper-V no Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
