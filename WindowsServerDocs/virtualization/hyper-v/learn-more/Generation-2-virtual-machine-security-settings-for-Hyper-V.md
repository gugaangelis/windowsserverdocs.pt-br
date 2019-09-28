---
title: Configurações de segurança de máquina virtual de geração 2 para Hyper-V
description: Descreve as configurações de segurança disponíveis no Gerenciador do Hyper-V para máquinas virtuais de geração 2
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 82544a58a8d46b3063605557be3c63cfa799e4fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364241"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configurações de segurança de máquina virtual de geração 2 para Hyper-V

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use as configurações de segurança de máquina virtual no Gerenciador do Hyper-V para ajudar a proteger os dados e o estado de uma máquina virtual. Você pode proteger máquinas virtuais contra inspeção, roubo e adulteração de ambos os malwares que podem ser executados no host e administradores do datacenter. O nível de segurança que você obtém depende do hardware do host que você executa, da geração da máquina virtual e se você configurou o serviço, chamado de serviço guardião de host, que autoriza os hosts a iniciarem máquinas virtuais blindadas.  

O serviço guardião de host é uma nova função no Windows Server 2016. Ele identifica hosts Hyper-V legítimos e permite que eles executem uma determinada máquina virtual. Normalmente, você configurou o serviço guardião de host para um datacenter. Mas você pode criar uma máquina virtual blindada para executá-la localmente sem configurar um serviço guardião de host. Posteriormente, você poderá distribuir a máquina virtual blindada para uma malha guardião de host.  

Se você ainda não configurou o serviço guardião de host ou o está executando no modo local no host Hyper-V e o host tem a chave guardião do proprietário da máquina virtual, você pode alterar as configurações descritas neste tópico.   Um proprietário de uma chave do Guardião é uma organização que cria e compartilha uma chave privada ou pública para possuir todas as máquinas virtuais criadas com essa chave.  

Para saber como você pode tornar suas máquinas virtuais mais seguras com o serviço guardião de host, consulte os recursos a seguir.  

- [Aumentar a proteção da malha: Protegendo segredos de locatário no Hyper-V (vídeo Ignite) ](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Malha protegida e VMs blindadas](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Configuração de inicialização segura no Gerenciador do Hyper-V  

A inicialização segura é um recurso disponível com máquinas virtuais de geração 2 que ajuda a impedir que o firmware não autorizado, sistemas operacionais ou drivers de Unified Extensible Firmware Interface (UEFI) (também conhecidos como ROMs de opção) sejam executados no momento da inicialização. A inicialização segura é habilitada por padrão. Você pode usar a inicialização segura com máquinas virtuais de geração 2 que executam sistemas operacionais de distribuição do Windows ou Linux.  

Os modelos descritos na tabela a seguir referem-se aos certificados de que você precisa para verificar a integridade do processo de inicialização.  

|Nome do modelo|Descrição|  
|-----------------|---------------|  
|Microsoft Windows|Selecione para a inicialização segura da máquina virtual para um sistema operacional Windows.|  
|Autoridade de certificação UEFI da Microsoft|Selecione para a inicialização segura da máquina virtual para um sistema operacional de distribuição do Linux.|  
|VM blindada de código aberto|Este modelo é utilizado para proteger a inicialização para [VMs blindadas baseadas em Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Para obter mais informações, consulte estes tópicos.  

- [Visão geral de segurança do Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Máquinas virtuais Linux e FreeBSD no Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configurações de suporte de criptografia no Gerenciador do Hyper-V

Você pode ajudar a proteger os dados e o estado da máquina virtual selecionando as opções de suporte de criptografia a seguir.  

- **Habilitar Trusted Platform Module** -essa configuração torna um chip de Trusted Platform Module virtualizado (TPM) disponível para sua máquina virtual. Isso permite que o convidado criptografe o disco da máquina virtual usando o BitLocker.
  - Se o host do Hyper-V estiver executando o Windows 10 1511, você precisará habilitar o modo de usuário isolado. 
- **Criptografar o estado e o tráfego de migração da VM** -criptografa o estado salvo da máquina virtual e o tráfego de migração ao vivo.

### <a name="enable-isolated-user-mode"></a>Habilitar modo de usuário isolado

Se você selecionar **habilitar Trusted Platform Module** em hosts Hyper-V que executam versões do Windows anteriores à atualização de aniversário do Windows 10, será necessário habilitar o modo de usuário isolado. Você não precisa fazer isso para hosts Hyper-V que executam a atualização de aniversário do Windows Server 2016 ou do Windows 10 ou posterior.

O modo de usuário isolado é o ambiente de tempo de execução que hospeda aplicativos de segurança dentro do modo de segurança virtual no host do Hyper-V. O modo de segurança virtual é usado para proteger e proteger o estado do chip TPM virtual.  

Para habilitar o modo de usuário isolado no host Hyper-V que executa versões anteriores do Windows 10,  

1.  Abra o Windows PowerShell como administrador.  

2.  Execute os comandos a seguir:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Você pode migrar uma máquina virtual com o TPM virtual habilitado para qualquer host que executa o Windows Server 2016, o Windows 10 Build 10586 ou versões posteriores. Mas se você migrá-lo para outro host, talvez não seja possível iniciá-lo. Você deve atualizar o protetor de chave para essa máquina virtual para autorizar o novo host a executar a máquina virtual. Para obter mais informações, consulte [malha protegida e VMs blindadas](https://go.microsoft.com/fwlink/?LinkId=746381) e [requisitos de sistema para o Hyper-V no Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Política de segurança no Gerenciador do Hyper-V  
Para obter mais segurança de máquina virtual, use a opção **habilitar blindagem** para desabilitar recursos de gerenciamento como conexão de console, PowerShell Direct e alguns componentes de integração. Se você selecionar essa opção, as opções de **inicialização segura**, **habilitar Trusted Platform Module**e **criptografia de estado e de migração de VM** serão selecionadas e impostas.   

Você pode executar a máquina virtual blindada localmente sem configurar um serviço guardião de host. Mas se você migrá-lo para outro host, talvez não seja possível iniciá-lo. Você deve atualizar o protetor de chave para essa máquina virtual para autorizar o novo host a executar a máquina virtual. Para obter mais informações, consulte [Malha Protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381).  

Para obter mais informações sobre segurança no Windows Server, consulte [segurança e garantia](../../../security/Security-and-Assurance.md).  
