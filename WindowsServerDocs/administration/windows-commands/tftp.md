---
title: tftp
description: Transferir arquivos para e de um computador remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 772f19a8-dafe-45cd-878a-f5691f6568ef vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad195409076840fda0e8d6bf5cd0c295a62cdede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845897"
---
# <a name="tftp"></a>tftp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfere arquivos e para um computador remoto, geralmente um computador executando o UNIX, que está executando o daemon ou o serviço Trivial FTP (tftp). o TFTP é normalmente usado por dispositivos incorporados ou sistemas que recuperam firmware, informações de configuração ou uma imagem do sistema durante o processo de inicialização de um servidor tftp.   

## <a name="syntax"></a>Sintaxe  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|-i|Especifica o modo de transferência de imagem binária (também chamado de modo de octeto). No modo de imagem binária, o arquivo é transferido em unidades de um byte. Use este modo quando transferir arquivos binários. Se **-i** é omitido, o arquivo é transferido em modo ASCII. Isso é o modo de transferência padrão. Nesse modo converte os caracteres do final de linha (EOL) em um formato apropriado para o computador especificado. Use este modo quando transferir arquivos de texto. Se uma transferência de arquivos for bem-sucedida, a taxa de transferência de dados é exibida.|  
|\<Host\>|Especifica o computador local ou remoto.|  
|colocar|Transfere o arquivo *fonte* no computador local para o arquivo *destino* no computador remoto. Porque o protocolo tftp não oferece suporte a autenticação do usuário, o usuário deve estar conectado ao computador remoto e os arquivos devem ser graváveis no computador remoto.|  
|obter|Transfere o arquivo *destino* no computador remoto para o arquivo *origem* no computador local.|  
|\<Origem\>|Especifica o arquivo para transferir.|  
|\<Destino\>|Especifica o local transferir o arquivo.|  

## <a name="remarks"></a>Comentários  
-   Você pode instalar o cliente tftp usando o Assistente de recursos para adicionar.  
-   O protocolo tftp não oferece suporte a qualquer mecanismo de autenticação ou criptografia e assim, pode oferecer um risco de segurança quando presente. Instalação do cliente tftp não é recomendado para sistemas conectados à Internet.  
-   O cliente tftp é um software opcional e sinalizado como desaprovado no Windows Vista e versões posteriores do sistema operacional Windows. Um serviço do servidor tftp não é fornecido pela Microsoft por motivos de segurança.  

## <a name="BKMK_Examples"></a>Exemplos  
Copie o arquivo **boot.img** do computador remoto **Host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
