---
title: cleanmgr
description: Configure a ferramenta de limpeza de disco (cleanmgr. exe) para limpar automaticamente determinados arquivos.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 49d85fe0c8ec1bbba810a502724fd7aac0c2f55d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712846"
---
# <a name="cleanmgr"></a>cleanmgr

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semestral)

Limpa os arquivos desnecessários do disco rígido do seu computador. Você pode usar opções de linha de comando para especificar que **cleanmgr** Limpe arquivos temporários, arquivos de Internet, arquivos baixados e arquivos de lixeira. Em seguida, você pode agendar a tarefa para ser executada em um horário específico usando a ferramenta **tarefas agendadas** .

## <a name="syntax"></a>Sintaxe

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /d`<driveletter>` | Especifica a unidade que você deseja que a limpeza de disco limpe.<p>**Observação:** A opção **/d** não é utilizada com `/sagerun:n`. |
| /sageset: n | Exibe a caixa de diálogo **configurações de limpeza de disco** e também cria uma chave do registro para armazenar as configurações selecionadas. O `n` valor, que é armazenado no registro, permite que você especifique tarefas para que a limpeza de disco seja executada. O `n` valor pode ser qualquer valor inteiro de 0 a 65535. |
| /sagerun: n | Executa as tarefas especificadas que são atribuídas ao valor n se você usar a opção **\sageset** . Todas as unidades no computador são enumeradas e o perfil selecionado é executado em cada unidade. |
| /TuneUp: n | Execute **/sageset** e **/sagerun** para o mesmo `n` . |
| /lowdisk | Execute com as configurações padrão. |
| /verylowdisk | Execute com as configurações padrão, sem prompts do usuário. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="options"></a>Opções

As opções para os arquivos que você pode especificar para limpeza de disco usando **/sageset** e **/sagerun** incluem:

- **Arquivos de instalação temporários** – são arquivos que foram criados por um programa de instalação que não está mais sendo executado.

- **Arquivos de programas baixados** -arquivos de programas baixados são controles ActiveX e programas Java que são baixados automaticamente da Internet quando você exibe determinadas páginas. Esses arquivos são armazenados temporariamente na pasta arquivos de programas baixados no disco rígido. Essa opção inclui um botão exibir arquivos para que você possa ver os arquivos antes que a limpeza de disco os remova. O botão abre a pasta arquivos de programas do C:\Winnt\Downloaded.

- **Arquivos temporários da Internet** -a pasta Temporary Internet Files contém páginas da Web que são armazenadas no disco rígido para exibição rápida. A limpeza de disco remove essas páginas, mas deixa as configurações personalizadas para páginas da Web intactas. Essa opção também inclui um botão View files, que abre a pasta C:\Documents and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5.

- **Arquivos chkdsk antigos** – quando o chkdsk verifica se há erros no disco, o chkdsk pode salvar fragmentos de arquivos perdidos como arquivos na pasta raiz no disco. Esses arquivos são desnecessários.

- **Lixeira** -a lixeira contém os arquivos que você excluiu do computador. Esses arquivos não são removidos permanentemente até que você Esvazie a lixeira. Essa opção inclui um botão exibir arquivos que abre a lixeira.<p>**Observação:** Uma lixeira pode aparecer em mais de uma unidade, por exemplo, não apenas em% SystemRoot%.

- **Arquivos temporários – os** programas às vezes armazenam informações temporárias em uma pasta temporária. Antes de um programa ser fechado, o programa geralmente exclui essas informações. Você pode excluir com segurança os arquivos temporários que não foram modificados na última semana.

- **Temporários arquivos offline** -arquivos offline temporários são cópias locais de arquivos de rede usados recentemente. Esses arquivos são armazenados em cache automaticamente para que você possa usá-los depois de se desconectar da rede. Um botão **Exibir arquivos** abre a pasta arquivos offline.

- **Arquivos offline** -arquivos offline são cópias locais de arquivos de rede que você deseja que estejam disponíveis offline para que você possa usá-los depois de se desconectar da rede. Um botão **Exibir arquivos** abre a pasta arquivos offline.

- **Compactar arquivos antigos** – o Windows pode compactar arquivos que você não usou recentemente. Compactar arquivos economiza espaço em disco, mas você ainda pode usar os arquivos. Nenhum arquivo é excluído. Como os arquivos são compactados em taxas diferentes, a quantidade exibida de espaço em disco que você obterá será aproximada. Um botão Opções permite que você especifique o número de dias de espera antes que a limpeza de disco compacte um arquivo não utilizado.

- **Arquivos de catálogo para o indexador de conteúdo** -o serviço de indexação acelera e melhora as pesquisas de arquivo mantendo um índice dos arquivos que estão no disco. Esses arquivos de catálogo permanecem de uma operação de indexação anterior e podem ser excluídos com segurança.<p>**Observação:** O arquivo de catálogo pode aparecer em mais de uma unidade, por exemplo, não `%SystemRoot%`apenas no.

>[!NOTE]
> Se você especificar a limpeza da unidade que contém a instalação do Windows, todas essas opções estarão disponíveis na guia **limpeza de disco** . Se você especificar qualquer outra unidade, apenas a lixeira e os arquivos de catálogo para opções de índice de conteúdo estarão disponíveis na guia **limpeza de disco** .

## <a name="examples"></a>Exemplos

Para executar o aplicativo de limpeza de disco para que você possa usar sua caixa de diálogo para especificar opções para uso posterior, salvando as configurações no conjunto **1**, digite o seguinte:

```
cleanmgr /sageset:1
```

Para executar a limpeza de disco e incluir as opções que você especificou com o comando cleanmgr/sageset: 1, digite:

```
cleanmgr /sagerun:1
```

Para executar `cleanmgr /sageset:1` e `cleanmgr /sagerun:1` em conjunto, digite:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Referências adicionais

- [Liberar espaço em disco no Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
