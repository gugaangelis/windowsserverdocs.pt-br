---
title: Serviços de área de trabalho remota - armazenamento seguro de dados
description: Informações de planejamento para armazenar com segurança dados por meio de discos de perfil do usuário (UPDs) no RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: c3c7be624e3b093347807a5ee131270d3c802f1a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805165"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Serviços de área de trabalho remota - armazenamento de dados segura com UPDs

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Recursos de business Store, os dados de personalização de usuário e configurações de segurança local ou no Azure. Hosts de sessão de área de trabalho remota é usar a autenticação do AD e capacitar seus usuários com os recursos necessários em um ambiente personalizado, com segurança. 

Garantindo que os usuários tenham uma experiência consistente, independentemente do ponto de extremidade do qual acessar seus recursos remotos, é um aspecto importante do gerenciamento de uma implantação do RDS. Discos de perfil do usuário (UPDs) permitem que os dados de usuário, personalizações e configurações do aplicativo a seguir de um usuário em uma única coleção. Um UPD é uma por usuário, arquivo VHD por coleção salvo em um compartilhamento central que está montado em uma sessão do usuário quando eles entrarem - UPD é tratado como uma unidade local para a duração da sessão em questão. 

Da perspectiva do usuário, o UPD fornece uma experiência de famililar - eles salvem seus documentos em seus documentos de pasta (o que parece ser uma unidade local), alterar suas configurações de aplicativo como de costume e fazer todas as personalizações para o ambiente do Windows. Todos esses dados, incluindo o hive do registro, são armazenados no UPD e persistir em um compartilhamento de rede central. UPDs só estão disponíveis para o usuário quando o usuário está ativamente conectado a um desktop ou o RemoteApp. UPDs só podem fazer roam dentro de uma coleção porque do todo usuário `C:\Users\<username\>` diretório (incluindo AppData\Local) é armazenado no UPD.

Você pode usar [cmdlets do PowerShell](https://technet.microsoft.com/library/jj215443.aspx) para designar o caminho para o compartilhamento central, o tamanho de cada UPD e quais pastas devem ser incluídas ou excluídas do perfil do usuário salvo ao UPD. Como alternativa, você pode habilitar os UPDs por meio do Gerenciador do servidor, vá para **dos serviços de área de trabalho remota** > **coleções** > **a coleção de área de trabalho**  >  **Propriedades de coleção da área de trabalho** > **discos de perfil do usuário**. Observe que você deseja habilita ou desabilitar UPDs para todos os usuários de uma coleção inteira, não para usuários específicos nessa coleção. UPDs devem ser armazenados em um compartilhamento de arquivos central onde os servidores na coleção tem permissões de controle total. 

Você pode obter alta disponibilidade para os UPDs, armazenando-os no Azure com o [espaços de armazenamento diretos](rds-storage-spaces-direct-deployment.md). 