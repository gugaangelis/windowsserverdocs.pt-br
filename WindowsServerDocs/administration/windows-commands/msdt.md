---
title: msdt
description: Artigo de referência do comando MSDT, que invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado, e habilita opções adicionais sem entrada do usuário.
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8ef856cd54b93c77d4e260a5e433c67407d9611
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886203"
---
# <a name="msdt"></a>msdt

Invoca um pacote de solução de problemas na linha de comando ou como parte de um script automatizado e habilita opções adicionais sem entrada do usuário.

## <a name="syntax"></a>Sintaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /ID`<packagename>` | Especifica qual pacote de diagnóstico deve ser executado. Para obter uma lista de pacotes disponíveis, consulte [pacotes de solução de problemas disponíveis](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs). |
| /Path`<directory|.diagpkg file|.diagcfg file>` | Especifica o caminho completo para um pacote de diagnóstico. Se você especificar um diretório, o diretório deverá conter um pacote de diagnóstico. Você não pode usar o parâmetro **/path** em conjunto com os parâmetros * */ID * *, **/DCI**ou **/cab** . |                                                                                   |
| /dci`<passkey>` | Popula o campo de chave de acesso. Esse parâmetro só é usado quando um provedor de suporte forneceu uma chave de acesso. |
| /DT`<directory>` | Exibe o histórico de solução de problemas no diretório especificado. Os resultados de diagnóstico são armazenados nos diretórios **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** do usuário. |
| /af`<answerfile>` | Especifica um arquivo de resposta em formato XML que contém respostas a uma ou mais interações de diagnóstico. |
| /modal`<ownerHWND>` | Torna o pacote de solução de problemas modal para uma janela designada pelo HWND (identificador de janela do console) pai. Esse parâmetro é normalmente usado por aplicativos que iniciam um pacote de solução de problemas. Para obter mais informações sobre como obter identificadores de janela de console, consulte [como obter um identificador de janela de console (HWND)](https://support.microsoft.com/help/124103/how-to-obtain-a-console-window-handle-hwnd). |
| /moreoptions`<true|false>` | Habilita (true) ou suprime (false) a tela final de solução de problemas que pergunta se o usuário deseja explorar opções adicionais. Esse parâmetro é normalmente usado quando o pacote de solução de problemas é iniciado por um solucionador de problemas que não faz parte do sistema operacional. |
| /param`<parameters>` | Especifica um conjunto de respostas de interação na linha de comando, semelhante a um arquivo de resposta. Esse parâmetro normalmente não é usado dentro do contexto de pacotes de solução de problemas criados com o designer do TSP. Para obter mais informações sobre como desenvolver parâmetros personalizados, consulte [plataforma de solução de problemas do Windows](/previous-versions/windows/desktop/wintt/windows-troubleshooting-toolkit-portal). |
| /Advanced | Expande o link avançado na página de boas-vindas por padrão quando o pacote de solução de problemas é iniciado. |
| /custom | Solicita que o usuário confirme cada resolução possível antes de ser aplicada. |

### <a name="return-codes"></a>Códigos de retorno

Os pacotes de solução de problemas compreendem um conjunto de causas raiz, cada uma delas descreve um problema técnico específico. Depois de concluir as tarefas do pacote de solução de problemas, cada causa raiz retorna um estado de fixo, não fixo, detectado (mas não corrigível) ou não encontrado. Além dos resultados específicos relatados na interface do usuário do solucionador de problemas, o mecanismo de solução de problemas retorna um código nos resultados que descrevem, em termos gerais, se a solução de problemas corrigiu ou não o problema original. Os códigos são:

| Código | Descrição |
| ---- | ----------- |
| -1 | **Interrupção:** A solução de problemas foi fechada antes que as tarefas de solução de problemas fossem concluídas. |
| 0 | **Corrigido:** O solucionador de problemas identificou e corrigiu pelo menos uma causa raiz e nenhuma causa raiz permanece em um estado não fixo. |
| 1 | **Presente, mas não corrigido:** O solucionador de problemas identificou uma ou mais causas raiz que permanecem em um estado não fixo. Esse código é retornado mesmo que outra causa raiz tenha sido corrigida. |
| 2 | **Não encontrado:** A solução de problemas não identificou nenhuma causa raiz. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Pacotes de solução de problemas disponíveis](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/ee424379(v=ws.11)#available-troubleshooting-packs)

- [Referência do PowerShell do TroubleshootingPack](/powershell/module/troubleshootingpack/?view=win10-ps)
