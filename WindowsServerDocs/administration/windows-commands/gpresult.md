---
title: gpresult
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0acd4563225bc1c413b2096387ad8c14f6009a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835967"
---
# <a name="gpresult"></a>gpresult

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe as informações de conjunto de diretivas resultante (RSoP) para um usuário remoto e o computador.
Para usar relatórios RSoP para computadores de destino remoto através do firewall, você deve ter regras de firewall que permitem o tráfego de rede nas portas.
## <a name="syntax"></a>Sintaxe
```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```
## <a name="parameters"></a>Parâmetros
> [!NOTE]
> Exceto quando você usar **/?**, você deve incluir uma opção de saída, tanto **/r**, **/v**, **/z.**, **/x**, ou **/h**.

|Parâmetro|Descrição|
|-------|--------|
|/s \<system\>|Especifica o nome ou endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local.|
|/u \<nome de usuário\>|Usa as credenciais do usuário especificado para executar o comando. O usuário padrão é o usuário que fez logon no computador que emite o comando.|
|/p [\<PASSWOrd\>]|Especifica a senha da conta de usuário que é fornecida na **/u** parâmetro. Se **/p** for omitido, **gpresult** solicitará a senha. **/p** não pode ser usado com **/x** ou **/h**.|
|/User [\<TARGETDOMAIN\>\\]\<TARGETUSER\>|Especifica o usuário remoto cujos dados RSoP deve ser exibido.|
|escopo / {usuário &#124; computador}|Exibe dados de RSoP para o usuário ou computador. Se **escopo/** for omitido, **gpresult** exibe dados de RSoP para o usuário e o computador.|
|[/x &#124; /h] <FILENAME>|Salva o relatório no XML (**/x**) ou HTML (**/h**) no local e com o que é especificado pelo nome do arquivo de formato de *FILENAME* parâmetro. Não pode ser usado com **/u**, **/p**, **/r**, **/v**, ou **/z**.|
|/f|força **gpresult** para substituir o nome do arquivo especificado na **/x** ou **/h** opção.|
|/r|Exibe dados de resumo do RSoP.|
|/v|Exibe informações detalhadas de política. Isso inclui configurações detalhadas que foram aplicadas com uma precedência de 1.|
|/z|Exibe todas as informações disponíveis sobre a política de grupo. Isso inclui configurações detalhadas que foram aplicadas com uma precedência de 1 e superior.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Diretiva de grupo é a principal ferramenta administrativa para definir e controlar como os programas, recursos de rede e o sistema operacional operam para usuários e computadores em uma organização. Em um ambiente do Active Directory, diretiva de grupo é aplicada a usuários ou computadores com base em sua participação em sites, domínios ou unidades organizacionais.
-   Porque você pode aplicar configurações de política de sobreposição para qualquer computador ou usuário, o recurso de diretiva de grupo gera um conjunto resultante das configurações de política quando o usuário fizer logon. **gpresult** exibe o conjunto resultante das configurações de política foram aplicadas no computador para o usuário especificado quando o usuário conectado.
-   Porque **/v** e **/z.** geram grandes quantidades de informações, é útil redirecionar a saída para um arquivo de texto (por exemplo, **gpresult/z > txt**).
-   O **gpresult** comando está disponível no Windows Server 2012, Windows Server 2008 R2, Server 2008 do Windows, Windows 8, Windows 7 e Windows Vista.
## <a name="BKMK_Examples"></a>Exemplos
O exemplo a seguir recupera dados de RSoP para o usuário remoto **nome_usuário_destino** do computador **srvmain**e exibe dados sobre o usuário apenas sobre o RSoP. O comando é executado com as credenciais do usuário **maindom\hiropln**, e **p@ssW23** for inserida como a senha para esse usuário.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```
O exemplo a seguir salva todas as informações disponíveis sobre a política de grupo para o usuário remoto **nome_usuário_destino** do computador **srvmain** em um arquivo chamado **txt**. Nenhum dado é incluído sobre o computador. O comando é executado com as credenciais do usuário **maindom\hiropln**, e **p@ssW23** for inserida como a senha para esse usuário.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```
O exemplo a seguir exibe dados de RSoP para o computador **srvmain** e o usuário conectado. Dados são incluídos sobre o usuário e o computador. O comando é executado com as credenciais do usuário **maindom\hiropln**, e **p@ssW23** for inserida como a senha para esse usuário.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```
## <a name="additional-references"></a>Referências adicionais
-   [Group Policy TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
