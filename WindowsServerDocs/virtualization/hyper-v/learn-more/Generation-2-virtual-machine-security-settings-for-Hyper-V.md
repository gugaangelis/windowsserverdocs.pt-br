---
title: Configurações de segurança de máquina virtual de geração 2 para Hyper-V
description: Descreve as configurações de segurança disponíveis no Gerenciador do Hyper-V para máquinas virtuais de geração 2
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 7eb867529d38ab21ee21c19f92c89ed4128b0ea4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860799"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configurações de segurança de máquina virtual de geração 2 para Hyper-V

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use as configurações de segurança de máquina virtual no Gerenciador do Hyper-V para ajudar a proteger os dados e o estado de uma máquina virtual. Você pode proteger máquinas virtuais contra inspeção, roubo e adulteração de malware que pode ser executado no host e administradores do datacenter. O nível de segurança que você obtém depende do hardware do host que você executa, da geração da máquina virtual e de se você configurou o serviço, chamado de Serviço Guardião de Host, que autoriza os hosts a iniciarem máquinas virtuais blindadas.  

O Serviço Guardião de Host é uma nova função no Windows Server 2016. Ele identifica hosts Hyper-V legítimos e permite que eles executem uma determinada máquina virtual. Normalmente, você configurou o Serviço Guardião de Host para um datacenter. Mas você pode criar uma máquina virtual blindada para executá-la localmente sem configurar um Serviço Guardião de Host. Posteriormente, você poderá distribuir a máquina virtual blindada para uma Malha de Guardião de Host.  

Se você ainda não configurou o Serviço Guardião de Host ou está executando-o no modo local no host Hyper-V e o host tem a chave guardião do proprietário da máquina virtual, é possível alterar as configurações descritas neste tópico.   Um proprietário de uma chave do Guardião é uma organização que cria e compartilha uma chave privada ou pública para ser proprietária de todas as máquinas virtuais criadas com essa chave.  

Para saber como você pode tornar suas máquinas virtuais mais seguras com o Serviço Guardião de Host, confira os recursos a seguir.  

- [Aumentar a proteção da malha: Proteção de segredos de locatário no Hyper-V (vídeo do Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Malha Protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Configuração de Inicialização Segura no Gerenciador do Hyper-V  

A Inicialização Segura é um recurso disponível com máquinas virtuais de geração 2 que ajuda a impedir que firmware, sistemas operacionais ou drivers UEFI (Unified Extensible Firmware Interface) (também conhecidos como ROMs de opção) não autorizados sejam executados no momento da inicialização. A Inicialização Segura está habilitada por padrão. Você pode usar a inicialização segura com máquinas virtuais de geração 2 que executam sistemas operacionais de distribuição do Windows ou do Linux.  

Os modelos descritos na tabela a seguir referem-se aos certificados de que você precisa para verificar a integridade do processo de inicialização.  

|Nome do modelo|Descrição|  
|-----------------|---------------|  
|Microsoft Windows|Selecione para a inicialização segura da máquina virtual para um sistema operacional Windows.|  
|Autoridade de Certificação UEFI da Microsoft|Selecione para a inicialização segura da máquina virtual para um sistema operacional de distribuição Linux.|  
|VM Blindada de software livre|Este modelo é utilizado para proteger a inicialização para [VMs blindadas baseadas em Linux](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Para obter mais informações, consulte estes tópicos.  

- [Visão Geral de Segurança do Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Máquinas Virtuais do Linux e FreeBSD no Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configurações de suporte de criptografia no Gerenciador do Hyper-V

Você pode ajudar a proteger os dados e o estado da máquina virtual selecionando as opções de suporte de criptografia a seguir.  

- **Habilitar o Trusted Platform Module** – essa configuração torna um chip de TPM (Trusted Platform Module) virtualizado disponível para sua máquina virtual. Isso permite que o participante criptografe o disco da máquina virtual usando o BitLocker.
  - Se o host do Hyper-V estiver executando o Windows 10 1511, você precisará habilitar o modo de usuário isolado. 
- **Criptografar o Estado e o tráfego de migração de VM** – criptografa o estado salvo da máquina virtual e o tráfego de migração dinâmica.

### <a name="enable-isolated-user-mode"></a>Habilitar Modo de Usuário Isolado

Se você selecionar **Habilitar o Trusted Platform Module** em hosts do Hyper-V que executam versões do Windows anteriores à atualização de aniversário do Windows 10, será necessário habilitar o modo de usuário isolado. Você não precisa fazer isso para hosts Hyper-V que executam a atualização de aniversário do Windows Server 2016 ou do Windows 10 ou posterior.

O Modo de Usuário isolado é o ambiente de runtime que hospeda aplicativos de segurança dentro do Modo de Segurança Virtual no host do Hyper-V. O Modo de Segurança Virtual é usado para proteger o estado do chip do TPM virtual.  

Para habilitar o Modo de Usuário Isolado no host Hyper-V que executa versões anteriores do Windows 10,  

1.  Abra o Windows PowerShell como administrador.  

2.  Execute os comandos a seguir:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Você pode migrar uma máquina virtual com o TPM virtual habilitado para qualquer host que execute o Windows Server 2016, o Windows 10 Build 10586 ou versões posteriores. Porém, se você migrá-lo para outro host, talvez não seja possível iniciá-lo. Você deve atualizar o protetor de chave para essa máquina virtual para autorizar o novo host a executar a máquina virtual. Para obter mais informações, confira [Malha Protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381) e [Requisitos do sistema para o Hyper-V no Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Política de Segurança no Gerenciador do Hyper-V  
Para obter mais segurança de máquina virtual, use a opção **Habilitar a Blindagem** para desabilitar recursos de gerenciamento como conexão de console, PowerShell Direct e alguns componentes de integração. Se você selecionar essa opção, as opções **Inicialização Segura**, **Habilitar Trusted Platform Module** e **Criptografar Estado e tráfego de migração de VM** serão selecionadas e impostas.   

Você pode executar a máquina virtual blindada localmente sem configurar um Serviço Guardião de Host. Porém, se você migrá-lo para outro host, talvez não seja possível iniciá-lo. Você deve atualizar o protetor de chave para essa máquina virtual para autorizar o novo host a executar a máquina virtual. Para obter mais informações, confira [Malha Protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381).  

Para obter mais informações sobre a segurança no Windows Server, confira [Garantia e segurança](../../../security/Security-and-Assurance.md).  
