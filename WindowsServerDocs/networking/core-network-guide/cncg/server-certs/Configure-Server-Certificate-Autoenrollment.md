---
title: Configurar o registro automático do certificado do servidor
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 502e0542d326fd46a1736c08b3f34fea178b4198
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955943"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar o registro automático do certificado

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

> [!NOTE]
> Antes de executar este procedimento, você deve configurar um modelo de certificado de servidor usando o snap-in Console de Gerenciamento Microsoft para Modelos de Certificado em uma autoridade de certificação que execute AD CS.
A associação aos grupos **Administradores de Empresa** e **Admins. do Domínio** do domínio raiz é o mínimo exigido para executar este procedimento.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar o registro automático do certificado do servidor

1. No computador em que o AD DS está instalado, abra o Windows PowerShell &reg; , digite **MMC**e pressione Enter. O Console de Gerenciamento Microsoft será aberto.
2. No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.
3. Em **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de política de grupo**. A caixa de diálogo **selecionar política de grupo objeto** é aberta.

     > [!IMPORTANT]
     > Certifique-se de selecionar **Editor de gerenciamento de política de grupo** e não **política de grupo Gerenciamento**. Se você selecionar **política de grupo Gerenciamento**, sua configuração usando essas instruções falhará e um certificado do servidor não será registrado automaticamente em seu NPSs.

4. Em **Selecionar Objeto de Diretiva de Grupo**, clique em **Procurar**. A caixa de diálogo **Procurar um Objeto de Diretiva de Grupo** é aberta.
5. Em **Domínios, OUs e Objetos de Diretiva de Grupo conectados**, clique em **Diretiva de Domínio Padrão**, e em **OK**.
6. Clique em **Concluir** e em **OK**.
7. Clique duas vezes em **Diretiva de Domínio Padrão**. No console do, expanda o seguinte caminho: **configuração do computador**, **políticas**, **configurações do Windows**, configurações de **segurança**e, em seguida, **políticas de chave pública**.
8. Clique em **Diretivas de Chave Pública**. No painel de detalhes, clique duas vezes em **Cliente dos Serviços de Certificados - Inscrição Automática**. A caixa de diálogo **Propriedades** é aberta. Configure os seguintes itens e clique em **OK**:

     1. Em **Modelo de Configuração**, selecione **Habilitado**.
     2. Marque a caixa de seleção **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**.
     3. Marque a caixa de seleção **Atualizar certificados que usam modelos de certificados**.

9. Clique em **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar registro automático de certificado de usuário

1. No computador em que o AD DS está instalado, abra o Windows PowerShell &reg; , digite **MMC**e pressione Enter. O Console de Gerenciamento Microsoft será aberto.
2. No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.
3. Em **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de política de grupo**. A caixa de diálogo **selecionar política de grupo objeto** é aberta.

     > [!IMPORTANT]
     > Certifique-se de selecionar **Editor de gerenciamento de política de grupo** e não **política de grupo Gerenciamento**. Se você selecionar **política de grupo Gerenciamento**, sua configuração usando essas instruções falhará e um certificado do servidor não será registrado automaticamente em seu NPSs.

4. Em **Selecionar Objeto de Diretiva de Grupo**, clique em **Procurar**. A caixa de diálogo **Procurar um Objeto de Diretiva de Grupo** é aberta.
5. Em **Domínios, OUs e Objetos de Diretiva de Grupo conectados**, clique em **Diretiva de Domínio Padrão**, e em **OK**.
6. Clique em **Concluir** e em **OK**.
7. Clique duas vezes em **Diretiva de Domínio Padrão**. No console do, expanda o seguinte caminho **: configuração do usuário**, **políticas**, configurações do **Windows**, configurações de **segurança**.
8. Clique em **Diretivas de Chave Pública**. No painel de detalhes, clique duas vezes em **Cliente dos Serviços de Certificados - Inscrição Automática**. A caixa de diálogo **Propriedades** é aberta. Configure os seguintes itens e clique em **OK**:

     1. Em **Modelo de Configuração**, selecione **Habilitado**.
     2. Marque a caixa de seleção **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**.
     3. Marque a caixa de seleção **Atualizar certificados que usam modelos de certificados**.

9. Clique em **OK**.

## <a name="next-steps"></a>Próximas etapas

[Atualizar Política de Grupo](refresh-group-policy.md)
