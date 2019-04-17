---
title: Configurar o modelo de certificado do servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>Configurar o modelo de certificado do servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar o modelo de certificado que do Active Directory&reg; serviços de certificado (AD CS) usa como base para os certificados de servidor que são registrados nos servidores em sua rede.  
  
Ao configurar esse modelo, você pode especificar os servidores por grupo do Active Directory que devem receber automaticamente um certificado de servidor do AD CS.   
  
O procedimento a seguir inclui instruções para configurar o modelo para emitir certificados para todos os seguintes tipos de servidor:  
  
- Servidores que executam o serviço de acesso remoto, incluindo Gateway RAS servidores, que são membros do **servidores RAS e IAS** grupo.  
- Servidores que executam o serviço de servidor de política de rede (NPS) que são membros do **servidores RAS e IAS** grupo.  
  
A associação ao grupo ambos **administradores corporativos** e o domínio raiz **Admins. do domínio** grupo é o requisito mínimo para concluir este procedimento.  
  
### <a name="to-configure-the-certificate-template"></a>Para configurar o modelo de certificado  
  
1.  No CA1, no Gerenciador do servidor, clique em **ferramentas**e clique em **autoridade de certificação**. O Console de gerenciamento do certificação autoridade Microsoft (MMC) é aberta.  
  
2.  No MMC, clique duas vezes o nome de autoridade de certificação, clique com botão direito **modelos de certificado**e clique em **gerenciar**.  
  
3.  O console de modelos de certificado é aberto. Todos os modelos de certificado são exibidos no painel de detalhes.  
  
4.  No painel de detalhes, clique no **RAS e o servidor IAS** modelo.  
  
5.  Clique no **ação** menu e, em seguida, clique **modelo duplicado**. O modelo **propriedades** caixa de diálogo é aberta.  
  
6.  Clique no **segurança** guia.   
  
7.  Sobre o **segurança** guia, **nomes de usuário ou grupo**, clique em **servidores RAS e IAS**.  
  
8.  Em **permissões para servidores RAS e IAS**, em **permitir**, certifique-se de que **registrar** está marcada e, em seguida, selecione o **registro automático** caixa de seleção. Clique em **Okey**e feche o MMC de modelos de certificado.  
  
9.  No MMC de autoridade de certificação, clique em **modelos de certificado**. Sobre o **ação**, aponte para **nova**e clique em **Certificate Template to Issue**. O **Ativar modelos de certificado** caixa de diálogo é aberta.  
  
10. Em **Ativar modelos de certificado**, clique no nome do modelo de certificado que você acabou configurado e, em seguida, clique em **Okey**. Por exemplo, se você não alterou o nome padrão do modelo de certificado, clique em **cópia do RAS e o servidor IAS**e clique em **Okey**.  
  


