---
title: Configurar o modelo de certificado do servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886117"
---
# <a name="configure-the-server-certificate-template"></a>Configurar o modelo de certificado do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar o modelo de certificado que o Active Directory&reg; serviços de certificados (AD CS) usa como base para certificados de servidor que são registrados em servidores em sua rede.  
  
Ao configurar este modelo, você pode especificar os servidores de grupo do Active Directory que devem receber automaticamente um certificado de servidor do AD CS.   
  
O procedimento a seguir inclui instruções para configurar o modelo para emitir certificados para todos os tipos de servidor a seguir:  
  
- Servidores que estão executando o serviço de acesso remoto, incluindo servidores de Gateway de RAS, que são membros de **servidores RAS e IAS** grupo.  
- Servidores que estão executando o serviço servidor de diretivas de rede (NPS) que são membros do **servidores RAS e IAS** grupo.  
  
A associação aos grupos **Administradores de Empresa** e **Admins. do Domínio** do domínio raiz é o mínimo exigido para executar este procedimento.  
  
### <a name="to-configure-the-certificate-template"></a>Para configurar o modelo de certificado  
  
1.  No CA1, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **autoridade de certificação**. A certificação autoridade Microsoft Management Console (MMC) é aberto.  
  
2.  No MMC, clique duas vezes o nome da autoridade de certificação, clique com botão direito **modelos de certificado**e, em seguida, clique em **gerenciar**.  
  
3.  Abre o console de modelos de certificado. Todos os modelos de certificado são exibidos no painel de detalhes.  
  
4.  No painel de detalhes, clique no **servidores RAS e IAS** modelo.  
  
5.  Clique o **ação** menu e clique **Duplicar modelo**. O modelo **propriedades** caixa de diálogo é aberta.  
  
6.  Clique na guia **Segurança**.   
  
7.  Sobre o **segurança** guia **nomes de grupo ou usuário**, clique em **servidores RAS e IAS**.  
  
8.  Na **permissões para servidores RAS e IAS**, em **permitir**, certifique-se de que **registrar** está selecionado e, em seguida, selecione o **registrar automaticamente** verificar caixa. Clique em **Okey**e feche o MMC de modelos de certificado.  
  
9.  No MMC de autoridade de certificação, clique em **modelos de certificado**. Sobre o **ação** , aponte para **New**e, em seguida, clique em **modelo de certificado a ser emitido**. A caixa de diálogo **Ativar Modelos de Certificado** é aberta.  
  
10. Na **Ativar modelos de certificado**, clique no nome do modelo de certificado que você acabou a configurado e, em seguida, clique em **Okey**. Por exemplo, se você não alterou o nome padrão do modelo de certificado, clique em **cópia dos servidores RAS e IAS**e, em seguida, clique em **Okey**.  
  


