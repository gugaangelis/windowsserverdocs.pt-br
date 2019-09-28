---
title: cmstp
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd2e8dbb08b41875335b35dd802007a0bd1fbd41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379272"
---
# <a name="cmstp"></a>cmstp

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala ou remove um perfil de serviço do Gerenciador de conexões. Usado sem parâmetros opcionais, o **cmstp** instala um perfil de serviço com as configurações padrão apropriadas para o sistema operacional e para as permissões do usuário. 
## <a name="syntax"></a>Sintaxe
Sintaxe 1:
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
Sintaxe 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|< Nome_de_Arquivo_do_Perfil_de_Serviço >. exe|Especifica, por nome, o pacote de instalação que contém o perfil que você deseja instalar.<br /><br />Necessário para a sintaxe 1, mas não é válido para a sintaxe 2.|
|/q:a|Especifica que o perfil deve ser instalado sem avisar o usuário. A mensagem de verificação de que a instalação foi bem-sucedida ainda será exibida.<br /><br />Necessário para a sintaxe 1, mas não é válido para a sintaxe 2.|
|[Unidade:] [caminho] <ServiceProfileFileName>. inf|Obrigatório. Especifica, por nome, o arquivo de configuração que determina como o perfil deve ser instalado.<br /><br />O parâmetro [unidade:] [caminho] não é válido para a sintaxe 1.|
|/nf|Especifica que os arquivos de suporte não devem ser instalados.|
|/ni|Especifica que um ícone de área de trabalho não deve ser criado. Esse parâmetro só é válido para computadores que executam o Windows 95, Windows 98, Windows NT 4,0 ou Windows Millennium Edition.|
|/ns|Especifica que um atalho de área de trabalho não deve ser criado. Esse parâmetro só é válido para computadores que executam um membro da família Windows Server 2003, Windows 2000 ou Windows XP.|
|/s|Especifica que o perfil de serviço deve ser instalado ou desinstalado silenciosamente (sem solicitar a resposta do usuário ou exibir a mensagem de verificação).|
|/su|Especifica que o perfil de serviço deve ser instalado para um único usuário em vez de para todos os usuários. Esse parâmetro só é válido para computadores que executam um Windows Server 2003, Windows 2000 ou Windows XP.|
|/au|Especifica que o perfil de serviço deve ser instalado para todos os usuários. Esse parâmetro só é válido para computadores que executam o Windows Server 2003, Windows 2000 ou Windows XP.|
|/u|Especifica que o perfil de serviço deve ser desinstalado.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
**/s** é o único parâmetro que você pode usar em combinação com **/u**.
A sintaxe 1 é a sintaxe típica usada em um aplicativo de instalação personalizada. Para usar essa sintaxe, você deve executar o **cmstp** a partir do diretório que contém o arquivo <ServiceProfileFileName>. exe.
## <a name="BKMK_Examples"></a>Disso
Para instalar o perfil de serviço ficção sem nenhum arquivo de suporte, digite:
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
Para instalar silenciosamente o perfil de serviço de ficção para um único usuário, digite:
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
Para desinstalar silenciosamente o perfil de serviço ficção, digite:
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
