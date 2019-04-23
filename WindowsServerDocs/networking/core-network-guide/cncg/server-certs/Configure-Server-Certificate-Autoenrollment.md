---
title: Configurar o registro automático de certificados de servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea7b7efe01525f4ecfb35200463c3f221d92d62d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888557"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar o registro automático de certificados

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

> [!NOTE]
> Antes de executar este procedimento, você deve configurar um modelo de certificado do servidor usando o snap-in do Console de gerenciamento do Microsoft de modelos de certificado em uma autoridade de certificação que está executando o AD CS.
A associação aos grupos **Administradores de Empresa** e **Admins. do Domínio** do domínio raiz é o mínimo exigido para executar este procedimento.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar o registro automático de certificados de servidor

1. No computador em que o AD DS é instalado, abra o Windows PowerShell&reg;, digite **mmc**, e pressione ENTER. O Console de Gerenciamento Microsoft será aberto.
2. No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.
3. Na **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de diretiva de grupo**. O **Selecionar objeto de diretiva de grupo** caixa de diálogo é aberta.

     > [!IMPORTANT]
     > Verifique se você selecionou **Editor de gerenciamento de diretiva de grupo** e não **Group Policy Management**. Se você selecionar **gerenciamento de política de grupo**, a configuração usando essas instruções falhará e um certificado de servidor não será registrado automaticamente para seu NPSs.

4. Em **Selecionar Objeto de Diretiva de Grupo**, clique em **Procurar**. A caixa de diálogo **Procurar um Objeto de Diretiva de Grupo** é aberta.
5. Em **Domínios, OUs e Objetos de Diretiva de Grupo conectados**, clique em **Diretiva de Domínio Padrão**, e em **OK**.
6. Clique em **Concluir**e em **OK**.
7. Clique duas vezes em **Diretiva de Domínio Padrão**. No console, expanda este caminho: **Configuração do Computador**, **Diretivas**, **Configurações do Windows**, **Configurações de Segurança** e **Diretivas de Chave Pública**.
8. Clique em **Diretivas de Chave Pública**. No painel de detalhes, clique duas vezes em **Cliente de Serviços de Certificado - Registro Automático**. O **propriedades** caixa de diálogo é aberta. Configure os seguintes itens e clique em **OK**:

     1. Em **Modelo de Configuração**, selecione **Habilitado**.
     2. Marque a caixa de seleção **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**.
     3. Marque a caixa de seleção **Atualizar certificados que usam modelos de certificados**.

9. Clique em **OK**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar o registro automático de certificados de usuário

1. No computador em que o AD DS é instalado, abra o Windows PowerShell&reg;, digite **mmc**, e pressione ENTER. O Console de Gerenciamento Microsoft será aberto.
2. No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.
3. Na **snap-ins disponíveis**, role para baixo e clique duas vezes em **Editor de gerenciamento de diretiva de grupo**. O **Selecionar objeto de diretiva de grupo** caixa de diálogo é aberta.

     > [!IMPORTANT]
     > Verifique se você selecionou **Editor de gerenciamento de diretiva de grupo** e não **Group Policy Management**. Se você selecionar **gerenciamento de política de grupo**, a configuração usando essas instruções falhará e um certificado de servidor não será registrado automaticamente para seu NPSs.

4. Em **Selecionar Objeto de Diretiva de Grupo**, clique em **Procurar**. A caixa de diálogo **Procurar um Objeto de Diretiva de Grupo** é aberta.
5. Em **Domínios, OUs e Objetos de Diretiva de Grupo conectados**, clique em **Diretiva de Domínio Padrão**, e em **OK**.
6. Clique em **Concluir**e em **OK**.
7. Clique duas vezes em **Diretiva de Domínio Padrão**. No console, expanda este caminho: **Configuração do usuário**, **diretivas**, **configurações do Windows**, **as configurações de segurança**.
8. Clique em **Diretivas de Chave Pública**. No painel de detalhes, clique duas vezes em **Cliente de Serviços de Certificado - Registro Automático**. O **propriedades** caixa de diálogo é aberta. Configure os seguintes itens e clique em **OK**:

     1. Em **Modelo de Configuração**, selecione **Habilitado**.
     2. Marque a caixa de seleção **Renovar certificados expirados, atualizar certificados pendentes e remover certificados revogados**.
     3. Marque a caixa de seleção **Atualizar certificados que usam modelos de certificados**.

9. Clique em **OK**.

## <a name="next-steps"></a>Próximas etapas

[Atualizar diretiva de grupo](refresh-group-policy.md)
