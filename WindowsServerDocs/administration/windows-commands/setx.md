---
title: setx
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b2caceed6962bef22e7d546fa3b4469c9682b39
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441249"
---
# <a name="setx"></a>setx



Cria ou modifica variáveis de ambiente no ambiente do sistema, ou usuário sem a necessidade de programação ou de script. O **Setx** comando também recupera os valores das chaves do registro e os grava em arquivos de texto.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>Parâmetros

|         Parâmetro          |                                                                                                                                              Descrição                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<Computer>       |                                                                                  Especifica o nome ou endereço IP de um computador remoto. Não use barras invertidas. O valor padrão é o nome do computador local.                                                                                  |
| /u [\<Domain>\]<User name> |                                                                                           Executa o script com as credenciais da conta de usuário especificado. O valor padrão é que as permissões do sistema.                                                                                            |
|      /p [\<Password>]      |                                                                                                         Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                                                                         |
|        \<Variable>         |                                                                                                                 Especifica o nome da variável de ambiente que você deseja definir.                                                                                                                  |
|          \<valor >          |                                                                                                                Especifica o valor para o qual você deseja definir a variável de ambiente.                                                                                                                 |
|         /k \<Path>         | Especifica que a variável é definida com base nas informações de uma chave do registro. O p*ath* usa a seguinte sintaxe:</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Por exemplo, você pode especificar o caminho a seguir:</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nome do arquivo >       |                                                                                                                               Especifica o arquivo que você deseja usar.                                                                                                                                |
|        /a \<X>,<Y>         |                                                                                                                    Especifica as coordenadas absolutas e o deslocamento como parâmetros de pesquisa.                                                                                                                    |
|   /r \<X>,<Y> "<String>"   |                                                                                                            Especifica as coordenadas relativas e o deslocamento a partir **cadeia de caracteres** como parâmetros de pesquisa.                                                                                                            |
|             /m             |                                                                                                Especifica para definir a variável no ambiente do sistema. A configuração padrão é o ambiente local.                                                                                                 |
|             /x             |                                                                                                       Exibe arquivos coordenadas, ignorando os **/a**, **/r**, e **/d** opções de linha de comando.                                                                                                        |
|      /d \<Delimiters>      |                    Especifica os delimitadores, como " **,** "ou" **\\** " para ser usado em conjunto com os quatro delimitadores internos — espaço, TAB, ENTER e avanço de linha. Delimitadores válidos incluem qualquer caractere ASCII. O número máximo de delimitadores é 15, incluindo os delimitadores internos.                    |
|             /?             |                                                                                                                                 Exibe a ajuda no prompt de comando.                                                                                                                                  |

## <a name="remarks"></a>Comentários

-   O **Setx** comando é semelhante ao utilitário UNIX SETENV.
-   **SETX** fornece a maneira apenas de linha de comando ou através de programação diretamente e permanentemente definir sistema valores de ambiente. Variáveis de ambiente do sistema são manualmente configuráveis por meio **painel de controle** ou por meio de um editor do registro. O **definir** comando, que é interno ao interpretador de comandos (Cmd.exe), define variáveis de ambiente para a janela de console atual somente.
-   Você pode usar o **setx** comando para definir variáveis de ambiente de valores para o usuário e do sistema de uma das três fontes (modos): O modo de linha de comando, o modo de registro ou modo de arquivo.
-   **SETX** grava variáveis de ambiente mestre no registro. Variáveis definidas com **setx** variáveis estão disponíveis comando futuras somente no windows, não na janela de comando atual.
-   **HKEY_CURRENT_USER** e **HKEY_LOCAL_MACHINE** são os hives do únicos com suporte. REG_DWORD, REG_EXPAND_SZ, REG_SZ e REG_MULTI_SZ são válidos **RegKey** tipos de dados.
-   Quando você obtém acesso a **REG_MULTI_SZ** valores no registro, somente o primeiro item é extraído e usado.
-   Não é possível usar o **setx** comando para remover os valores que foram adicionados aos ambientes local ou do sistema. Você pode usar **definir** com um nome de variável e nenhum valor para remover um valor correspondente do ambiente local.
-   Valores do Registro REG_DWORD são extraídos e usados no modo hexadecimal.
-   Modo de arquivo oferece suporte à análise de retorno de carro e alimentação de linha apenas arquivos de texto (CRLF).

## <a name="BKMK_examples"></a>Exemplos

Para definir a variável de ambiente de máquina no ambiente local com o valor Brand1, digite:
```
setx MACHINE Brand1
```
Para definir a variável de ambiente de máquina no ambiente do sistema para o valor Brand1 Computer, digite:
```
setx MACHINE "Brand1 Computer" /m
```
Para definir a variável de ambiente MYPATH no ambiente local para usar o caminho de pesquisa definido na variável de ambiente PATH, digite:
```
setx MYPATH %PATH%
```
Para definir a variável de ambiente MYPATH no ambiente local para usar o caminho de pesquisa definido na variável de ambiente PATH depois de substituir **~** com **%** , tipo:
```
setx MYPATH ~PATH~ 
```
Para definir a variável de ambiente de máquina no ambiente local com o valor Brand1 em um computador remoto chamado Computer1, digite:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Para definir a variável de ambiente MYPATH no ambiente local para usar o caminho de pesquisa definido na variável de ambiente PATH em um computador remoto chamado Computer1, digite:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Para definir a variável de ambiente TZONE no ambiente local para o valor encontrado na **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** tipo de chave do registro:
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para definir a variável de ambiente TZONE no ambiente local de um computador remoto chamado Computer1 com o valor encontrado na **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** registro tipo de chave:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Para definir a variável de ambiente de compilação no ambiente do sistema para o valor encontrado na **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** tipo de chave do registro:
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
Para definir a variável de ambiente de compilação no ambiente do sistema de um computador remoto chamado Computer1 com o valor encontrado na **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber** chave do registro , digite:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
Para exibir o conteúdo de um arquivo chamado ipconfig. out, juntamente com as coordenadas correspondentes do conteúdo, digite:
```
setx /f ipconfig.out /x
```
Para definir a variável de ambiente IPADDR no ambiente local para o valor encontrado na coordenada 5,11 no arquivo ipconfig. out, digite:
```
setx IPADDR /f ipconfig.out /a 5,11
```
Para definir a variável de ambiente OCTET1 no ambiente local para o valor encontrado na coordenada 5,3 no arquivo ipconfig. out com delimitadores **"#$\*."** , tipo:
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
Para definir a variável de ambiente IPGATEWAY no ambiente local para o valor encontrado na coordenada 0,7 em relação a coordenada do "Gateway" no arquivo ipconfig. out, digite:
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Para exibir o conteúdo de um arquivo chamado ipconfig. out — junto com as coordenadas de correspondentes dos conteúdo — em um computador chamado Computer1, digite:
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)