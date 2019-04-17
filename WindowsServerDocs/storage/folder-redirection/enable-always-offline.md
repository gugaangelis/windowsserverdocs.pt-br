---
title: Habilitar o modo sempre Offline para acesso mais rápido aos arquivos
description: Como usar o modo sempre Offline dos arquivos Offline para fornecer acesso mais rápido aos arquivos armazenados em cache e pastas redirecionadas.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc54b1e33d09e7f2b9eea01e4f09fb83f13dc1af
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831386"
---
# Habilitar o modo sempre Offline para acesso mais rápido aos arquivos

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este documento descreve como usar o modo sempre Offline dos arquivos Offline para fornecer acesso mais rápido aos arquivos armazenados em cache e pastas redirecionadas. Sempre off-line também fornece o uso de largura de banda menor porque os usuários são sempre trabalhar offline, mesmo quando eles são conectados por meio de uma conexão de rede de alta velocidade.

## Pré-requisitos

Para habilitar o modo sempre Offline, seu ambiente deve atender aos seguintes pré-requisitos.

- Um domínio do Active Directory Domain Services (AD DS) com computadores cliente ingressado no domínio. Não existem requisitos de nível funcional de floresta ou domínio ou requisitos do esquema.
- Computadores de clientes que executam o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. (Os computadores cliente que executam versões anteriores do Windows podem continuar a transição para o modo Online em conexões de rede de alta velocidade.)
- Um computador com o gerenciamento de política de grupo instalado.

## Habilitar o modo de sempre Offline

Para habilitar o modo sempre Offline, use a política de grupo para habilitar a configuração de política de **modo de vínculos lentos de configurar** e defina a latência como **1** (milissegundos). Isso faz com que os computadores cliente que executam o Windows 8 ou Windows Server 2012 usam automaticamente o modo sempre Offline.

>[!NOTE]
>Computadores que executam o Windows 7, Windows Vista, Windows Server 2008 R2 ou Windows Server 2008 pode continuar a transição para o modo Online se a latência da conexão de rede cai abaixo de um milésimo de segundo.

1. Abra o **gerenciamento de política de grupo**.
2. Para criar opcionalmente um novo grupo de política de objeto (GPO) para configurações de arquivos Offline, clique com botão direito no domínio apropriado ou unidade organizacional (UO) e, em seguida, selecione **criar um GPO neste domínio e vinculá-lo aqui**.
3. Na árvore de console, clique com botão direito no GPO para o qual você deseja definir as configurações de arquivos Offline e, em seguida, selecione **Editar**. O **Editor de gerenciamento de política de grupo** é exibido.
4. Na árvore de console, em **Configuração do computador**, expanda **políticas**, modelos **Administrativos**, expanda **rede**e expanda **Os arquivos Offline**.
5. Clique com botão direito **Configurar modo de vínculos lentos**e, em seguida, selecione **Editar**. A janela de **modo de vínculos lentos configurar** será exibida.
6. Selecione **Habilitado**.
7. Na caixa **Opções** , selecione **Mostrar**. A **janela de Mostrar conteúdo** será exibida.
8. Na caixa **nome do valor** , especifique o compartilhamento de arquivos para o qual você deseja habilitar o modo de sempre Offline.
9. Para habilitar o modo sempre Offline em todos os compartilhamentos de arquivos, digite **\***.
10. Na caixa de **valor** , insira **latência = 1** para definir o limite de latência como um milésimo de segundo e, em seguida, selecione **Okey**.

>[!NOTE]
>Por padrão, quando estiver em modo sempre Offline, Windows sincroniza arquivos no cache de arquivos Offline em segundo plano a cada duas horas. Para alterar esse valor, use a configuração de política de **Configurar a sincronização em segundo plano** .

## Mais informações

* [Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
* [Implanta o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)