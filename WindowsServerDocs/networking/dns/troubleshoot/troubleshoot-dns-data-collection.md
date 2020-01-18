---
title: Solucionando problemas de DNS (sistema de nomes de domínio)
description: Este artigo apresenta como coletar dados quando ocorrem problemas de DNS.
manager: dcscontentpm
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 11c52b3beca3afcc0a6bfc8cecee2143dce0f023
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265828"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>Solucionando problemas de DNS (sistema de nomes de domínio)
 
Problemas de resolução de nomes de domínio podem ser divididos em problemas do lado do cliente e do servidor. Em geral, você deve começar com a solução de problemas do lado do cliente, a menos que determine durante a fase de escopo que o problema está definitivamente ocorrendo no lado do servidor.

- [Solução de problemas de clientes DNS](troubleshoot-dns-client.md)

- [Solucionando problemas de servidores DNS](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>Coleta de dados
 
É recomendável que você colete dados simultaneamente nos lados do cliente e do servidor quando o problema ocorrer. No entanto, dependendo do problema real, você pode iniciar sua coleção em um único conjunto de dados no cliente DNS ou no servidor DNS.
 
Para coletar um diagnóstico de rede do Windows de um cliente afetado e seu servidor DNS configurado, siga estas etapas:

1. Iniciar capturas de rede no cliente e no servidor:

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. Limpe o cache DNS no cliente DNS executando o seguinte comando:

   ```cmd
   ipconfig /flushdns
   ```

3. Reproduza o problema.

4. Parar e salvar rastreamentos:

   ```cmd
   netsh trace stop
   ```

5. Salve os arquivos NetTrace. cab de cada computador. Essas informações serão úteis quando você entrar em contato com Suporte da Microsoft.