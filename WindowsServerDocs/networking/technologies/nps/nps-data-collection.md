---
title: Coleta de dados de usuário do servidor de políticas de rede
description: Quais informações são usadas para ajudar a autenticar usuários pelo servidor de políticas de rede no Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: d393ad4af81ee1c24fa5f28b8a3b05217e7b34dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396295"
---
# <a name="network-policy-server-user-data-collection"></a>Coleta de dados de usuário do servidor de políticas de rede

Este documento explica como localizar informações de usuário coletadas pelo NPS (servidor de diretivas de rede) no evento que você gostaria de removê-las.

>[!Note]
>Se você estiver interessado em exibir ou excluir dados pessoais, consulte as diretrizes da Microsoft nas [solicitações de entidade de dados do Windows para o site GDPR](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) . Se você estiver procurando informações gerais sobre GDPR, consulte a [seção GDPR do portal de confiança do serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informações coletadas pelo NPS

- Carimbo de data/hora
- Carimbo de hora do evento
- Nome de usuário
- Nome de usuário totalmente qualificado
- Endereço IP do Cliente
- Fornecedor do cliente
- Nome amigável do cliente
- Tipo de autenticação
- Vários outros campos relacionados ao protocolo RADIUS

## <a name="gather-data-from-nps"></a>Coletar dados do NPS

Se os dados de contabilidade estiverem habilitados e configurados, os registros das tentativas de autenticação do NPS de um usuário poderão ser obtidos em SQL Server ou nos arquivos de log, dependendo da configuração. 

Se os dados de contabilidade estiverem configurados para SQL Server, consulte para todos os registros em que User_Name = `'<username>'`.

Se os dados de contabilidade estiverem configurados para um arquivo de log, pesquise o `<username>` arquivo de log para localizar todas as entradas de log.

As entradas de log de eventos de serviços de acesso e política de rede são consideradas duplicadas para os dados de contabilidade e não precisam ser coletadas.

Se os dados de contabilidade não estiverem habilitados, os registros das tentativas de autenticação do NPS de um usuário poderão ser obtidos no log de eventos de serviços de acesso `<username>`e diretiva de rede, pesquisando pelo.
