---
title: Habilitar o modo Sempre Offline para acesso mais rápido aos arquivos
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
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401961"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar o modo Sempre Offline para acesso mais rápido aos arquivos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (Canal Semestral)

Este documento descreve como usar o modo sempre offline do Arquivos Offline para fornecer acesso mais rápido a arquivos armazenados em cache e pastas redirecionadas. Sempre offline também fornece menor uso de largura de banda porque os usuários estão sempre trabalhando offline, mesmo quando estão conectados por meio de uma conexão de rede de alta velocidade.

## <a name="prerequisites"></a>Pré-requisitos

Para habilitar o modo sempre offline, seu ambiente deve cumprir os pré-requisitos a seguir.

- Um domínio AD DS (Active Directory Domain Services) com computadores cliente ingressados no domínio. Não há requisitos em nível funcional de domínio ou floresta nem requisitos de esquema.
- Computadores cliente que executam o Windows 10, o Windows 8.1, o Windows 8, o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012. (Os computadores cliente que executam versões anteriores do Windows podem continuar fazendo a transição para o modo online em conexões de rede de alta velocidade.)
- Um computador com Gerenciamento de Política de Grupo instalado.

## <a name="enable-always-offline-mode"></a>Habilitar o modo Sempre Offline

Para habilitar o modo Sempre Offline, use Política de Grupo para habilitar a configuração de política de **Configurar o modo de vínculo lento** e defina a latência como **1** (milissegundo). Isso faz com que os computadores cliente que executam o Windows 8 ou o Windows Server 2012 usem automaticamente o modo sempre offline.

>[!NOTE]
>Os computadores que executam o Windows 7, o Windows Vista, o Windows Server 2008 R2 ou o Windows Server 2008 poderão continuar fazendo a transição para o modo online se a latência da conexão de rede ficar abaixo de um milissegundo.

1. Abra **Gerenciamento de Política de Grupo**.
2. Para, opcionalmente, criar um GPO (Objeto de Política de Grupo) para configurações de Arquivos Offline, clique com o botão direito do mouse no domínio ou na UO (unidade organizacional) apropriada e, em seguida, selecione **Criar um GPO nesse domínio e vinculá-lo aqui**.
3. Na árvore de console, clique com o botão direito do mouse no GPO para o qual você deseja definir as configurações de Arquivos Offline e, em seguida, selecione **Editar**. O **Editor de Gerenciamento de Política de Grupo** é exibido.
4. Na árvore de console, em **Configuração do Computador**, expanda **Políticas**, expanda **Modelos Administrativos**, expanda **Rede** e expanda **Arquivos Offline**.
5. Clique com o botão direito do mouse em **Configurar o modo de vínculo lento** e, em seguida, selecione **Editar**. A janela **Configurar o modo de vínculo lento** será exibida.
6. Selecione **Habilitado**.
7. Na caixa **Opções**, selecione **Mostrar**. A **janela Mostrar Conteúdo** aparecerá.
8. Na caixa **Nome do valor**, especifique o compartilhamento de arquivo para o qual você deseja habilitar o modo Sempre Offline.
9. Para habilitar o modo Sempre Offline em todos os compartilhamentos de arquivos, insira **\*** .
10. Na caixa **Valor**, insira **Latency=1** para definir o limite de latência como um milissegundo e, em seguida, selecione **OK**.

>[!NOTE]
>Por padrão, no modo sempre offline, o Windows sincroniza arquivos no cache Arquivos Offline em segundo plano a cada duas horas. Para alterar esse valor, use a configuração de política **Definir Sincronizador em Segundo Plano**.

## <a name="more-information"></a>Mais informações

* [Visão geral de Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](folder-redirection-rup-overview.md)
* [Implantar Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md)