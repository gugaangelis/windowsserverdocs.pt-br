---
title: Configurações de segurança de máquina virtual de geração 1 para Hyper-V
description: Descreve as configurações de segurança disponíveis no Gerenciador do Hyper-V para máquinas virtuais de geração 1
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: d7076056462e7fa3e822c49abfb37278f5ea3482
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745991"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Configurações de segurança de máquina virtual de geração 1

>Aplica-se a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Use as configurações de segurança de máquina virtual de geração 1 no Gerenciador do Hyper-V para ajudar a proteger os dados e o estado de uma máquina virtual.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Configurações de suporte de criptografia no Gerenciador do Hyper-V

Você pode ajudar a proteger os dados e o estado da máquina virtual selecionando a opção de suporte de criptografia a seguir.

- **Criptografar o tráfego de migração de máquina virtual e estado** – criptografa o estado salvo da máquina virtual quando ele é gravado no disco e no tráfego de migração dinâmica.

Para habilitar essa opção, adicione uma unidade de armazenamento de chaves à máquina virtual.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Unidade de armazenamento de chaves no Gerenciador do Hyper-V

Uma unidade de armazenamento de chaves fornece uma pequena unidade para a máquina virtual para que uma chave do BitLocker seja armazenada. Isso permite que a máquina virtual criptografe seu disco do sistema operacional sem exigir um chip TPM (Trusted Platform Module virtualizado). O conteúdo da unidade de armazenamento de chaves é criptografado usando um protetor de chave. O protetor de chave cria o host Hyper-V para executar a máquina virtual. O conteúdo da unidade de armazenamento de chaves e do protetor de chave é armazenado como parte do estado de runtime da máquina virtual.

Para descriptografar o conteúdo da unidade de armazenamento de chaves e iniciar a máquina virtual, o host Hyper-V precisa:

- Fazer parte de uma malha protegida autorizada para a máquina virtual ou
- Ter a chave privada de um dos guardiões da máquina virtual.

Para saber mais sobre malhas protegidas, confira a seção Introdução a VMs blindadas em [Segurança e garantia](../../../security/Security-and-Assurance.yml).

Você pode adicionar uma unidade de armazenamento de chaves a um slot vazio em um dos controladores do IDE da máquina virtual. Para fazer isso, clique em **Adicionar Unidade de Armazenamento de Chaves** para adicionar uma unidade de armazenamento de chaves ao primeiro slot do controlador do IDE gratuito dessa máquina virtual.

## <a name="additional-references"></a>Referências adicionais

- [Configurações de segurança de máquina virtual de geração 2 no gerenciador do Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Segurança e garantia](../../../security/Security-and-Assurance.yml)