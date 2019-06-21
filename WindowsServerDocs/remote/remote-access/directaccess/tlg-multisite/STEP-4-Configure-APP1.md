---
title: Etapa 4 configurar o APP1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 208839b827965d5fdbef4927f25a2477e117999b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283189"
---
# <a name="step-4-configure-app1"></a>Etapa 4 configurar o APP1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Configurar o endereçamento IPv6 estático e configurações de gateway para habilitar o APP1 acesso à sub-rede Corpnet 2.  
  
- Para configurar o gateway padrão e o servidor DNS. A configuração de vários sites usa o computador de roteador 1 como um gateway padrão. Configure o gateway padrão no APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Para configurar o gateway padrão e o servidor DNS  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  No **conexões de rede** janela, clique com botão direito **Conexão de Ethernet com fio**e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **propriedades da Conexão Ethernet com fio** caixa de diálogo, clique em **Internet Protocol versão 4 (TCP/IPv4)** e, em seguida, clique em **propriedades**.  
  
4.  Na **gateway padrão**, digite **10.0.0.254**e, na **servidor DNS alternativo**, tipo **10.2.0.1**e, em seguida, clique em **Okey** .  
  
5.  Sobre o **propriedades da Conexão Ethernet com fio** caixa de diálogo, clique em **protocolo IP versão 6 (TCP/IPv6)** e, em seguida, clique em **propriedades**.  
  
6.  Na **gateway padrão**, digite **2001:db8:1::fe**. Na **servidor DNS alternativo**, digite **2001:db8:2::1**e, em seguida, clique em **Okey**.  
  
7.  Sobre o **propriedades da Conexão Ethernet com fio** caixa de diálogo, clique em **fechar**e, em seguida, feche o **conexões de rede** janela.  
  


