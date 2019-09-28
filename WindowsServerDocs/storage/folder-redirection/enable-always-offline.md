---
title: Habilitar o modo sempre offline para acesso mais rápido aos arquivos
description: Como usar o modo sempre offline do Arquivos Offline para fornecer acesso mais rápido a arquivos armazenados em cache e pastas redirecionadas.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 389fdd26a7e1d9824f1eaf0136a544547f08eb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401961"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar o modo sempre offline para acesso mais rápido aos arquivos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canal semianual)

Este documento descreve como usar o modo sempre offline do Arquivos Offline para fornecer acesso mais rápido a arquivos armazenados em cache e pastas redirecionadas. Sempre offline também fornece menor uso de largura de banda porque os usuários estão sempre trabalhando offline, mesmo quando estão conectados por meio de uma conexão de rede de alta velocidade.

## <a name="prerequisites"></a>Pré-requisitos

Para habilitar o modo sempre offline, seu ambiente deve atender aos seguintes pré-requisitos.

- Um domínio Active Directory Domain Services (AD DS) com computadores cliente ingressados no domínio. Não há requisitos de nível funcional de floresta ou de domínio ou requisitos de esquema.
- Computadores cliente que executam o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Os computadores cliente que executam versões anteriores do Windows podem continuar a fazer a transição para o modo online em conexões de rede de alta velocidade.)
- Um computador com gerenciamento de Política de Grupo instalado.

## <a name="enable-always-offline-mode"></a>Habilitar o modo sempre offline

Para habilitar o modo sempre offline, use Política de Grupo para habilitar a configuração de política **Configurar modo de link lento** e defina a latência como **1** (milissegundo). Isso faz com que os computadores cliente que executam o Windows 8 ou o Windows Server 2012 usem automaticamente o modo sempre offline.

>[!NOTE]
>Os computadores que executam o Windows 7, o Windows Vista, o Windows Server 2008 R2 ou o Windows Server 2008 podem continuar a fazer a transição para o modo online se a latência da conexão de rede cair abaixo de um milissegundo.

1. Abra o **Gerenciamento de política de grupo**.
2. Para, opcionalmente, criar um novo objeto de Política de Grupo (GPO) para Arquivos Offline configurações, clique com o botão direito do mouse no domínio ou na UO (unidade organizacional) apropriada e, em seguida, selecione **criar um GPO nesse domínio e vincule-o aqui**.
3. Na árvore de console, clique com o botão direito do mouse no GPO para o qual você deseja definir as configurações de Arquivos Offline e, em seguida, selecione **Editar**. O **Editor de gerenciamento de política de grupo** é exibido.
4. Na árvore de console, em **configuração do computador**, expanda **políticas**, expanda **modelos administrativos**, expanda **rede**e expanda **arquivos offline**.
5. Clique com o botão direito do mouse em **Configurar modo de vínculo lento**e selecione **Editar**. A janela **Configurar modo de vínculo lento** será exibida.
6. Selecione **Habilitado**.
7. Na caixa **Opções** , selecione **Mostrar**. A **janela Mostrar conteúdo** será exibida.
8. Na caixa **nome do valor** , especifique o compartilhamento de arquivos para o qual você deseja habilitar o modo sempre offline.
9. Para habilitar o modo sempre offline em todos os compartilhamentos de arquivos, digite **\*** .
10. Na caixa **valor** , digite **latência = 1** para definir o limite de latência como um milissegundo e, em seguida, selecione **OK**.

>[!NOTE]
>Por padrão, quando estiver no modo sempre offline, o Windows sincronizará arquivos no cache Arquivos Offline em segundo plano a cada duas horas. Para alterar esse valor, use a configuração de política **definir sincronizador em segundo plano** .

## <a name="more-information"></a>Mais informações

* [Visão geral dos perfis de usuário de redirecionamento de pasta, Arquivos Offline e roaming](folder-redirection-rup-overview.md)
* [Implantar redirecionamento de pasta com Arquivos Offline](deploy-folder-redirection.md)