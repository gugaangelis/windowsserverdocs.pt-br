---
title: Configurações de segurança da máquina virtual do Hyper-V geração 1
description: Descreve as configurações de segurança disponíveis no Gerenciador do Hyper-V para máquinas virtuais de geração 1
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 923216142a45071bc3623e3f37b37cc6a2361f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812137"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Configurações de segurança de máquina virtual de geração 1

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use as configurações de segurança de máquina virtual de geração 1 no Gerenciador do Hyper-V para ajudar a proteger os dados e o estado de uma máquina virtual.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configurações de suporte de criptografia no Gerenciador do Hyper-V

Você pode ajudar a proteger os dados e o estado da máquina virtual selecionando a opção de suporte de criptografia a seguir.

- **Criptografar o tráfego de migração de estado e a máquina virtual** -criptografa o estado salvo da máquina virtual quando ela é gravada em disco e o tráfego da migração ao vivo.

Para habilitar essa opção, você deve adicionar uma unidade de armazenamento de chaves para a máquina virtual.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unidade de armazenamento de chaves no Gerenciador do Hyper-V

Uma unidade de armazenamento de chave fornece uma unidade pequena para a máquina virtual para uma chave do BitLocker a ser armazenado. Isso permite que a máquina virtual criptografar o disco do sistema operacional sem exigir um chip Trusted Platform Module (TPM) virtualizado. O conteúdo da unidade de armazenamento de chaves é criptografado usando um protetor de chave. O host do protetor de chave authories Hyper-V para executar a máquina virtual. O conteúdo da unidade de armazenamento de chaves e o protetor de chave é armazenado como parte do estado de tempo de execução da máquina virtual.

Para descriptografar o conteúdo da unidade de armazenamento de chaves e iniciar a máquina virtual, o host Hyper-V precisa estar:

- Parte de uma malha protegida autorizada para essa máquina virtual, ou
- Tem a chave privada de um dos guardiões da máquina virtual.

Para saber mais sobre malhas protegidas, consulte a seção de Introdução ao VMs Blindadas no [garantia e segurança](../../../security/Security-and-Assurance.md).

Você pode adicionar uma unidade de armazenamento de chaves em um slot vazio em um dos controladores IDE da máquina virtual. Para fazer isso, clique em **adicionar a unidade de armazenamento de chave** para adicionar uma unidade de armazenamento de chaves para o primeiro slot de controlador IDE gratuito dessa máquina virtual.

##<a name="see-also"></a>Consulte também

- [Configurações de segurança de máquina virtual de geração 2 no Gerenciador do Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Garantia e segurança](../../../security/Security-and-Assurance.md)