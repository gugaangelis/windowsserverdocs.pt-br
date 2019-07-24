---
title: Habilitar o modo sempre Offline para acesso mais rápido aos arquivos
description: Como usar o modo sempre Offline de arquivos Offline para fornecer acesso mais rápido aos arquivos armazenados em cache e as pastas redirecionadas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: ddf6a816e417c2eddff090df8dba841a894a3255
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447677"
---
# <a name="enable-always-offline-mode-for-faster-access-to-files"></a>Habilitar o modo sempre Offline para acesso mais rápido aos arquivos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2 e Windows (canal semestral)

Este documento descreve como usar o modo sempre Offline de arquivos Offline para fornecer acesso mais rápido aos arquivos armazenados em cache e as pastas redirecionadas. Sempre Offline também fornece o uso de largura de banda menor porque os usuários sempre trabalhando offline, mesmo quando eles estão conectados por meio de uma conexão de rede de alta velocidade.

## <a name="prerequisites"></a>Pré-requisitos

Para habilitar o modo sempre Offline, seu ambiente deve cumprir os pré-requisitos a seguir.

- Um domínio de serviços de domínio Active Directory (AD DS) com os computadores cliente ingressados no domínio. Não há nenhum requisito de esquema ou requisitos de nível funcional de floresta ou domínio.
- Computadores de cliente que executam o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Os computadores cliente que executam versões anteriores do Windows poderá continuar para fazer a transição para o modo Online em conexões de rede de alta velocidade.)
- Um computador com o gerenciamento de diretiva de grupo instalado.

## <a name="enable-always-offline-mode"></a>Habilitar o modo sempre Offline

Para habilitar o modo sempre Offline, use a diretiva de grupo para habilitar o **configurar o modo de vínculos lentos** política de configuração e defina a latência para **1** (milissegundo). Isso faz com que os computadores cliente que executam o Windows 8 ou Windows Server 2012 para usar o modo sempre Offline automaticamente.

>[!NOTE]
>Computadores que executam o Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 pode continuar a transição para o modo Online se a latência de conexão de rede cair abaixo de um milissegundo.

1. Abra **gerenciamento de diretiva de grupo**.
2. Para, opcionalmente, criar um novo grupo de política de GPO (objeto) para configurações de arquivos Offline, clique no domínio apropriado ou a unidade organizacional (UO) e, em seguida, selecione **criar um GPO neste domínio e vinculá-lo aqui**.
3. Na árvore de console, clique com botão direito no GPO para o qual você deseja definir as configurações de arquivos Offline e, em seguida, selecione **editar**. O **Editor de gerenciamento de diretiva de grupo** é exibida.
4. Na árvore de console, sob **configuração do computador**, expanda **diretivas**, expanda **modelos administrativos**, expanda **rede**, e expanda **arquivos off-line**.
5. Clique com botão direito **configurar o modo de vínculos lentos**e, em seguida, selecione **editar**. O **configurar o modo de vínculos lentos** janela será exibida.
6. Selecione **Habilitado**.
7. No **opções** caixa, selecione **Mostrar**. O **janela Mostrar conteúdo** será exibida.
8. No **nome do valor** , especifique o compartilhamento de arquivos para o qual você deseja habilitar o modo sempre Offline.
9. Para habilitar o modo sempre Offline em todos os compartilhamentos de arquivos, digite **\\** *.
10. No **valor** , digite **latência = 1** para definir o limite de latência de um milissegundo e, em seguida, selecione **Okey**.

>[!NOTE]
>Por padrão, quando no modo sempre Offline, Windows sincroniza arquivos no cache de arquivos Offline em segundo plano a cada duas horas. Para alterar esse valor, use o **configurar a sincronização de plano de fundo** configuração de política.

## <a name="more-information"></a>Mais informações

* [Visão geral do redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
* [Implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)