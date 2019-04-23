---
title: Coleta de dados de usuário do servidor de diretivas de rede
description: Quais informações são usadas para ajudar a autenticar usuários pelo servidor de diretiva de rede no Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888737"
---
# <a name="network-policy-server-user-data-collection"></a>Coleta de dados de usuário do servidor de diretivas de rede

Este documento explica como localizar informações de usuário coletadas pelo servidor de diretivas de rede (NPS) no caso de você gostaria de removê-lo.

>[!Note]
>Se você estiver interessado em Exibir ou excluir dados pessoais, diretrizes da Microsoft, verifique as [solicitações de entidade de dados do Windows para o GDPR](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) site. Se você estiver procurando informações gerais sobre o GDPR, consulte o [seção GDPR do portal de confiança do serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informações coletadas pelo NPS

- Carimbo de data/hora
- Carimbo de hora do evento
- Nome de usuário
- Nome de usuário qualificado completo
- Endereço IP do Cliente
- Fornecedor do cliente
- Nome amigável do cliente
- Tipo de autenticação
- Vários outros campos sobre o protocolo RADIUS

## <a name="gather-data-from-nps"></a>Coletar dados de NPS

Se os dados de estatísticas estiver habilitados e configurados, os registros de tentativas de autenticação do NPS do usuário podem ser obtidos do SQL Server ou os arquivos de log, dependendo da configuração. 

Se dados de estatísticas estiver configurados para o SQL Server, a consulta para todos os registros onde User_Name = `'<username>'`.

Se dados de estatísticas estiver configurados para um arquivo de log, em seguida, pesquise o arquivo de log para o `<username>` para localizar todas as entradas de log.

Entradas de log de eventos de serviços de acesso e diretiva de rede são consideradas correntes para os dados de estatísticas e não precisam ser coletados.

Se os dados de estatísticas não estão habilitados, então registros de tentativas de autenticação do NPS do usuário podem ser obtidos do log de eventos dos serviços de acesso e política de rede por meio de pesquisa a `<username>`.
