---
title: 'Serviços de Área de Trabalho Remota: armazenamento de dados seguro'
description: Informações de planejamento para armazenar com segurança dados por meio de discos de perfil do usuário (UPDs) no RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 934aab380f9e58f4fe9567921623279a1893af4b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860289"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Serviços de Área de Trabalho Remota: armazenamento de dados seguro com UPDs

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Recursos de negócios do Store, dados de personalização de usuário e configurações de segurança no local ou no Azure. Hosts de sessão do RD usam a autenticação do AD e capacitam os usuários com os recursos necessários em um ambiente personalizado, com segurança. 

Garantir que os usuários tenham uma experiência consistente, independentemente do ponto de extremidade do qual acessam os recursos remotos, é um aspecto importante do gerenciamento de uma implantação do RDS. Os UPDs (discos de perfil do usuário) permitem que os dados de usuário, personalizações e configurações do aplicativo sigam um usuário em uma única coleção. Um UPD é um arquivo VHD por coleção e por usuário salvo em um compartilhamento central montado em uma sessão do usuário quando eles entram, o UPD é tratado como uma unidade local durante a sessão em questão. 

Da perspectiva do usuário, o UPD fornece uma experiência de familiar, eles salvem os documentos na pasta Documentos (no que parece ser uma unidade local), alteram as configurações do aplicativo como de costume e fazer todas as personalizações no seu ambiente do Windows. Todos esses dados, incluindo o hive do registro, são armazenados no UPD e persistem em um compartilhamento de rede central. Os UPDs só estão disponíveis para o usuário quando ele está conectado ativamente a uma área de trabalho ou o RemoteApp. Os UPDs só podem usar perfil móvel dentro de uma coleção porque todo o diretório `C:\Users\<username\>` do usuário (incluindo AppData\Local) é armazenado no UPD.

Você pode usar [cmdlets do PowerShell](https://technet.microsoft.com/library/jj215443.aspx) para designar o caminho para o compartilhamento central, o tamanho de cada UPD e quais pastas devem ser incluídas ou excluídas do perfil do usuário salvo no UPD. Como alternativa, você pode habilitar os UPDs por meio do Gerenciador do Servidor, acesse **Serviços de Área de Trabalho Remota** > **Coleções** > **Coleção de área de trabalho** > **Propriedades de coleção da área de trabalho** > **Discos de perfil do usuário**. Observe que você habilita ou desabilitar os UPDs para todos os usuários de uma coleção inteira, não para usuários específicos nessa coleção. Os UPDs devem ser armazenados em um compartilhamento de arquivos central em que os servidores na coleção têm permissões de controle total. 

Você pode obter alta disponibilidade para os UPDs, armazenando-os no Azure com os [Espaços de Armazenamento Diretos](rds-storage-spaces-direct-deployment.md). 