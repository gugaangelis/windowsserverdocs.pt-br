---
title: tftp
description: Transferir arquivos de e para um computador remoto.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66f729d090a78b74bc0334cd9b7276219a980e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370215"
---
# <a name="tftp"></a>tftp

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfere arquivos de e para um computador remoto, normalmente um computador executando UNIX, que executa o serviço ou daemon trivial protocolo FTP (TFTP). o TFTP normalmente é usado por dispositivos ou sistemas inseridos que recuperam firmware, informações de configuração ou uma imagem do sistema durante o processo de inicialização de um servidor TFTP.   

## <a name="syntax"></a>Sintaxe  
```  
tftp [-i] [<Host>] [{get | put}] <Source> [<Destination>]  
```  

### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|-i|Especifica o modo de transferência de imagem binária (também chamado de modo de octeto). No modo de imagem binária, o arquivo é transferido em unidades de um byte. Use esse modo ao transferir arquivos binários. Se **-i** for omitido, o arquivo será transferido no modo ASCII. Esse é o modo de transferência padrão. Esse modo converte os caracteres de fim de linha (EOL) em um formato apropriado para o computador especificado. Use esse modo ao transferir arquivos de texto. Se uma transferência de arquivo for bem-sucedida, a taxa de transferência de dados será exibida.|  
|\> \<host|Especifica o computador local ou remoto.|  
|Posicione|Transfere a *origem* do arquivo no computador local para o *destino* do arquivo no computador remoto. Como o protocolo TFTP não oferece suporte à autenticação de usuário, o usuário deve estar conectado ao computador remoto e os arquivos devem ser graváveis no computador remoto.|  
|obter|Transfere o *destino* do arquivo no computador remoto para a *origem* do arquivo no computador local.|  
|\<Origem\>|Especifica o arquivo a ser transferido.|  
|\<de destino\>|Especifica para onde transferir o arquivo.|  

## <a name="remarks"></a>Comentários  
-   Você pode instalar o cliente tftp usando o assistente para adicionar recursos.  
-   O protocolo TFTP não oferece suporte a nenhum mecanismo de autenticação ou criptografia e, como tal, pode introduzir um risco de segurança quando presente. A instalação do cliente tftp não é recomendada para sistemas conectados à Internet.  
-   O cliente TFTP é um software opcional e marcado como preterido no Windows Vista e em versões posteriores do sistema operacional Windows. Um serviço de servidor TFTP não é mais fornecido pela Microsoft por motivos de segurança.  

## <a name="BKMK_Examples"></a>Disso  
Copie o arquivo **boot. img** do computador remoto **Host1**.  
```  
tftp  -i Host1 get boot.img  
```  

## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
