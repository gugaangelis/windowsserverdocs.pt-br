---
title: gpresult
description: Artigo de referência do comando gpresult, que exibe o conjunto resultante de informações de política (RSoP) para um usuário e computador remotos.
ms.topic: reference
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ef5de0c8e4e4c4f75d8ccd680e20b8cf00385f5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025670"
---
# <a name="gpresult"></a>gpresult

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o conjunto resultante de informações de política (RSoP) para um usuário e computador remoto. Para usar relatórios RSoP para computadores de destino remoto por meio do firewall, você deve ter regras de firewall que habilitem o tráfego de rede de entrada nas portas.

## <a name="syntax"></a>Sintaxe

```
gpresult [/s <system> [/u <username> [/p [<password>]]]] [/user [<targetdomain>\]<targetuser>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <filename> [/f] | /?}
```

> [!NOTE]
> Exceto ao usar **/?**, você deve incluir uma opção de saída, **/r**, **/v**, **/z**, **/x**ou **/h**.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /s `<system>` | Especifica o nome ou o endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local. |
| /u `<username>` | Usa as credenciais do usuário especificado para executar o comando. O usuário padrão é o usuário que fez logon no computador que emite o comando. |
| /p `[<password>]` | Especifica a senha da conta de usuário que é fornecida no parâmetro **/u** . Se **/p** for omitido, **GPResult** solicitará a senha. O parâmetro **/p** não pode ser usado com **/x** ou **/h**. |
| / `[<targetdomain>\]<targetuser>]` | Especifica o usuário remoto cujos dados RSoP serão exibidos. |
| /Scope `{user | computer}` | Exibe dados RSoP para o usuário ou o computador. Se **/Scope** for omitido, **GPResult** exibirá dados RSoP para o usuário e o computador. |
| `[/x | /h] <filename>` | Salva o relatório em formato XML (**/x**) ou HTML (**/h**) no local e com o nome do arquivo especificado pelo parâmetro *filename* . Não pode ser usado com **/u**, **/p**, **/r**, **/v**ou **/z**. |
| /f | Força o **GPResult** a substituir o nome de arquivo especificado na opção **/x** ou **/h** . |
| /r | Exibe dados de Resumo de RSoP. |
| /v | Exibe informações detalhadas da política. Isso inclui configurações detalhadas que foram aplicadas com uma precedência de 1. |
| /z | Exibe todas as informações disponíveis sobre Política de Grupo. Isso inclui configurações detalhadas que foram aplicadas com uma precedência de 1 e superior. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Política de Grupo é a ferramenta administrativa principal para definir e controlar como programas, recursos de rede e o sistema operacional operam para usuários e computadores em uma organização. Em um ambiente do Active Directory, Política de Grupo é aplicada a usuários ou computadores com base em sua associação em sites, domínios ou unidades organizacionais.

- Como você pode aplicar configurações de política sobrepostas a qualquer computador ou usuário, o recurso Política de Grupo gera um conjunto resultante de configurações de política quando o usuário faz logon. O comando **GPResult** exibe o conjunto resultante de configurações de política que foram impostas no computador para o usuário especificado quando o usuário fez logon.

- Como **/v** e **/z** produzem muitas informações, é útil redirecionar a saída para um arquivo de texto (por exemplo, `gpresult/z >policy.txt` ).

### <a name="examples"></a>Exemplos

Para recuperar dados RSoP somente para o usuário remoto, *maindom\hiropln* com a senha *p@ssW23* , que está no computador *srvmain*, digite:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```

Para salvar todas as informações disponíveis sobre Política de Grupo em um arquivo chamado, *policy.txt*, somente para o usuário remoto *maindom\hiropln* com a senha *p@ssW23* , no computador *srvmain*, digite:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```

Para exibir dados do RSoP para o usuário conectado, *maindom\hiropln* com a senha *p@ssW23* , para o computador *srvmain*, digite:

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
