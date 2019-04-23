---
title: Configurações de segurança da máquina virtual do Hyper-V geração 2
description: Descreve as configurações de segurança disponíveis no Gerenciador do Hyper-V para máquinas virtuais de geração 2
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 90a2b7234ee55d8469b6e02ba3de3a0efc080a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889497"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Configurações de segurança da máquina virtual do Hyper-V geração 2

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use as configurações de segurança de máquina virtual no Gerenciador do Hyper-V para ajudar a proteger os dados e o estado de uma máquina virtual. Você pode proteger as máquinas virtuais contra inspeção, roubo e violação de malware que pode ser executado no host e os administradores do datacenter. O nível de segurança que você obtenha depende do hardware do host que você executa, a geração de máquina virtual, e se você configurar o serviço chamado serviço guardião de Host, que autoriza hosts para iniciar as máquinas virtuais blindadas.  

Serviço guardião de Host é uma nova função no Windows Server 2016. Ele identifica legítimos hosts do Hyper-V e permite a execução de uma determinada máquina virtual. Geralmente você configuraria o serviço guardião de Host para um data center. Mas você pode criar uma máquina virtual blindada para executá-lo localmente sem configurar um serviço guardião de Host. Posteriormente, você pode distribuir a máquina virtual blindada a uma malha de guardião de Host.  

Se você não configurou o serviço guardião de Host ou que esteja executando-o no modo local no host Hyper-V e o host tem a chave de guardião do proprietário da máquina virtual, você pode alterar as configurações descritas neste tópico.   Um proprietário de uma chave de guardião é uma organização que cria e compartilhamentos de uma chave pública ou privada seja o proprietário de todas as máquinas virtuais criada com essa chave.  

Para saber como você pode fazer suas máquinas virtuais mais segura com o serviço guardião de Host, consulte os seguintes recursos.  

- [Proteção da malha: Proteger segredos do locatário no Hyper-V (vídeo do Ignite)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [Malha protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Proteger a configuração de inicialização no Gerenciador do Hyper-V  

Inicialização segura é um recurso disponível com máquinas virtuais de geração 2 que ajuda a impedir o firmware não autorizado, sistemas operacionais ou drivers UEFI Unified Extensible Firmware Interface () (também chamados de option ROMs) em execução no momento da inicialização. Inicialização segura está habilitada por padrão. Você pode usar a inicialização segura com máquinas virtuais de geração 2 que executam sistemas operacionais de distribuição Windows ou Linux.  

Os modelos descritos na tabela a seguir se referem aos certificados que você precisa verificar a integridade do processo de inicialização.  

|Nome do modelo|Descrição|  
|-----------------|---------------|  
|Microsoft Windows|Selecione a máquina virtual para um sistema de operacional do Windows para a inicialização segura.|  
|Autoridade de certificação UEFI da Microsoft|Selecione a máquina virtual para um sistema de operacional de distribuição do Linux para a inicialização segura.|  
|Abrir fonte de VM Blindada|Esse modelo é utilizado para a inicialização segura para [baseados em Linux de VMs blindadas](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template).|

Para obter mais informações, consulte estes tópicos.  

- [Visão geral de segurança do Windows 10](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [Deve criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Linux e máquinas de virtuais FreeBSD no Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configurações de suporte de criptografia no Gerenciador do Hyper-V

Você pode ajudar a proteger os dados e o estado da máquina virtual, selecionando as seguintes opções de suporte de criptografia.  

- **Habilitar o Trusted Platform Module** -essa configuração faz um chip Trusted Platform Module (TPM) virtualizados disponíveis para sua máquina virtual. Isso permite que o convidado criptografar o disco de máquina virtual usando o BitLocker.
  - Se seu host do Hyper-V estiver executando o Windows 10 1511, você precisa habilitar o modo de usuário isolado. 
- **Criptografar o tráfego de migração de estado e VM** - criptografa o estado salvo da máquina virtual e o tráfego de migração ao vivo.

### <a name="enable-isolated-user-mode"></a>Habilitar o modo de usuário isolado

Se você selecionar **habilitar o Trusted Platform Module** em hosts do Hyper-V que executam versões do Windows anteriores à atualização de aniversário do Windows 10, você deve habilitar o modo de usuário isolado. Você não precisa fazer isso para os hosts do Hyper-V que executar o Windows Server 2016 ou atualização de aniversário do Windows 10 ou posterior.

Modo de usuário isolado é o ambiente de tempo de execução que hospeda os aplicativos de segurança dentro de modo seguro Virtual no host Hyper-V. O modo virtual seguro é usado para proteger o e o estado do chip do TPM virtual.  

Para habilitar o modo de usuário isolado no host Hyper-V que executam versões anteriores do Windows 10  

1.  Abra o Windows PowerShell como administrador.  

2.  Execute os comandos a seguir:  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

Você pode migrar uma máquina virtual com o TPM virtual habilitado para qualquer host que executa o Windows Server 2016, Windows 10 build 10586 ou superior de versões. Mas se você migrá-la para outro host, você não poderá iniciá-lo. Você deve atualizar o protetor de chave para a máquina virtual para autorizar o novo host para executar a máquina virtual. Para obter mais informações, consulte [malha protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381) e [requisitos de sistema do Hyper-V no Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).  

## <a name="security-policy-in-hyper-v-manager"></a>Política de segurança no Gerenciador do Hyper-V  
Para obter mais segurança de máquina virtual, use o **habilitar a blindagem** opção para desabilitar os recursos de gerenciamento, como conexão de console, o PowerShell Direct e alguns componentes de integração. Se você selecionar essa opção, **inicialização segura**, **habilitar Trusted Platform Module**, e **tráfego de migração de estado de criptografia e a VM** opções são selecionadas e impostas.   

Você pode executar na máquina virtual blindada localmente sem configurar um serviço guardião de Host. Mas se você migrá-la para outro host, você não poderá iniciá-lo. Você deve atualizar o protetor de chave para a máquina virtual para autorizar o novo host para executar a máquina virtual. Para obter mais informações, consulte [Malha Protegida e VMs Blindadas](https://go.microsoft.com/fwlink/?LinkId=746381).  

Para obter mais informações sobre a segurança no Windows Server, consulte [garantia e segurança](../../../security/Security-and-Assurance.md).  
