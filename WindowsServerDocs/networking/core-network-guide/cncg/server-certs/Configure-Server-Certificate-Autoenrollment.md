---
title: Configurar o registro automático de certificado de servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ddf8a905fdb68bbc474b10f526b32f3d8b83af46
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar o registro automático de certificados

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

> [!NOTE]
> Para executar este procedimento, você deve configurar um modelo de certificado do servidor, usando o snap-in do Console de gerenciamento do Microsoft de modelos de certificado em uma autoridade de certificação que esteja executando o AD CS.
A associação ao grupo ambos **administradores corporativos** e o domínio raiz **Admins. do domínio** grupo é o requisito mínimo para concluir este procedimento.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar o registro automático de certificado de servidor

1. No computador onde o AD DS é instalado, abra o Windows PowerShell&reg;, tipo **mmc**, e pressione ENTER. Abre o Console de gerenciamento Microsoft.
2. Sobre o **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.
3. Em **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de política de grupo**. O **Selecionar objeto de diretiva de grupo** caixa de diálogo é aberta.

     > [!IMPORTANT]
     > Certifique-se de que você selecione **Editor de gerenciamento de política de grupo** e não **Group Policy Management**. Se você selecionar **Group Policy Management**, a configuração usando estas instruções falhará e um certificado de servidor não serão registrados automaticamente para seus servidores NPS.

4. Em **objeto de política de grupo**, clique em **procurar**. O **procurar um objeto de política de grupo** caixa de diálogo é aberta.
5. Em **domínios, UOs e objetos de política de grupo vinculados,** clique **política de domínio padrão**e clique em **Okey**.
6. Clique em **concluir**e clique em **Okey**.
7. Clique duas vezes em **diretiva de domínio padrão**. No console, expanda o caminho a seguir: **configuração do computador**, **políticas**, **configurações do Windows**, **configurações de segurança**e depois **políticas de chave pública**.
8. Clique em **políticas de chave pública**. No painel de detalhes, clique duas vezes em **cliente de serviços de certificado - inscrição automática**. O **propriedades** caixa de diálogo é aberta. Configurar os itens a seguir e clique em **Okey**:

     1. Em **configuração modelo**, selecione **Enabled**.
     2. Selecione o **renovar expirados certificados, atualizar certificados pendentes e remover certificados revogados** caixa de seleção.
     3. Selecione o **atualizar certificados que usam modelos de certificado** caixa de seleção.

9. Clique em **Okey**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar o registro automático de certificados de usuário

1. No computador onde o AD DS é instalado, abra o Windows PowerShell&reg;, tipo **mmc**, e pressione ENTER. Abre o Console de gerenciamento Microsoft.
2. Sobre o **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.
3. Em **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de política de grupo**. O **Selecionar objeto de diretiva de grupo** caixa de diálogo é aberta.

     > [!IMPORTANT]
     > Certifique-se de que você selecione **Editor de gerenciamento de política de grupo** e não **Group Policy Management**. Se você selecionar **Group Policy Management**, a configuração usando estas instruções falhará e um certificado de servidor não serão registrados automaticamente para seus servidores NPS.

4. Em **objeto de política de grupo**, clique em **procurar**. O **procurar um objeto de política de grupo** caixa de diálogo é aberta.
5. Em **domínios, UOs e objetos de política de grupo vinculados,** clique **política de domínio padrão**e clique em **Okey**.
6. Clique em **concluir**e clique em **Okey**.
7. Clique duas vezes em **diretiva de domínio padrão**. No console, expanda o caminho a seguir: **configuração do usuário**, **políticas**, **configurações do Windows**, **configurações de segurança**e depois **políticas de chave pública**.
8. Clique em **políticas de chave pública**. No painel de detalhes, clique duas vezes em **cliente de serviços de certificado - inscrição automática**. O **propriedades** caixa de diálogo é aberta. Configurar os itens a seguir e clique em **Okey**:

     1. Em **configuração modelo**, selecione **Enabled**.
     2. Selecione o **renovar expirados certificados, atualizar certificados pendentes e remover certificados revogados** caixa de seleção.
     3. Selecione o **atualizar certificados que usam modelos de certificado** caixa de seleção.

9. Clique em **Okey**.

## <a name="next-steps"></a>Próximas etapas

[Política de grupo de atualização](refresh-group-policy.md)