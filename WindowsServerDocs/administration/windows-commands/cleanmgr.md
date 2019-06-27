---
title: cleanmgr
description: Saiba como usar as opções de linha de comando para configurar a ferramenta de limpeza de disco (Cleanmgr.exe) para limpar automaticamente determinados arquivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fd722dda8476891860773126c84ed10f125715f0
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407883"
---
# <a name="cleanmgr"></a>cleanmgr

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semestral)

Limpa os arquivos desnecessários do disco rígido do seu computador. Você pode usar opções de linha de comando para especificar que Cleanmgr limpa arquivos Temp, arquivos de Internet, os arquivos baixados e arquivos binários de reciclagem. Em seguida, você pode agendar a tarefa seja executada em um momento específico, usando a ferramenta tarefas agendadas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Parâmetros

|      Parâmetro      |    Descrição     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | Especifica a unidade que você deseja que a limpeza de disco.<br>**Observação:** A opção /d. não é utilizada com /sagerun:n. |
| /sageset:n | Exibe a caixa de diálogo de configurações de limpeza de disco e também cria uma chave do registro para armazenar as configurações que você selecionar. O `n` valor, que é armazenada no registro, permite que você especifique as tarefas de limpeza de disco para executar. O `n` valor pode ser qualquer valor inteiro de 0 a 65535. Para que todas as opções disponíveis quando você usa a opção /sageset, especifique a unidade em que o Windows está instalado.  |
|  /sagerun:n  |  Executa as tarefas especificadas que recebem o valor n Se você usar a opção \sageset. Todas as unidades no computador são enumeradas e o perfil selecionado é executado em cada uma delas.           |
| /TUNEUP:n    | Executar /sageset e /sagerun para o mesmo `n` . |
| /LOWDISK     | Execute com as configurações padrão. |
| /VERYLOWDISK | Executar com as configurações padrão, nenhum usuário solicita. |
| /?           | Exibir a Ajuda. |

## <a name="options"></a>Opções

As opções para os arquivos que você pode especificar para limpeza de disco usando /sageset e /sagerun incluem:

- **Arquivos de instalação temporária** -esses são arquivos que foram criados por um programa de instalação que não está mais funcionando.

- **Os arquivos de programas baixados** - arquivos de programas baixados são controles ActiveX e Java programas que são baixados automaticamente da Internet quando você exibe determinadas páginas. Esses arquivos são temporariamente armazenados na pasta arquivos de programas baixados no disco rígido. Essa opção inclui um botão Exibir arquivos para que você pode ver os arquivos antes de limpeza de disco remove-los. O botão abre a pasta de arquivos de programas C:\Winnt\Downloaded.

- **Arquivos temporários da Internet** -pasta Temporary Internet Files The contém páginas da Web que são armazenadas no disco rígido para uma exibição mais rápida. Limpeza de disco remove essa página, mas deixa intacto o suas configurações personalizadas para páginas da Web. Essa opção também inclui um botão Exibir arquivos, que abre a pasta C:\Documents and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5. 

- **Arquivos antigos de Chkdsk** -quando Chkdsk verifica um disco para erros, Chkdsk pode salvar fragmentos de arquivos perdidos como arquivos na pasta raiz do disco. Esses arquivos são desnecessários.

- **Lixeira** -o Lixeira contém arquivos que foram excluídos do computador. Esses arquivos não são removidos permanentemente até que você esvazia a Lixeira. Essa opção inclui um botão Exibir arquivos que abre a Lixeira. **Observação:** Uma Lixeira pode aparecer em mais de uma unidade, por exemplo, não apenas em % SystemRoot %.

- **Arquivos temporários** -programas, às vezes, armazenam informações temporárias em uma pasta Temp. Antes de ser fechado, o programa normalmente exclui essas informações. Você pode excluir com segurança arquivos temporários que não foram modificados na última semana.

- **Arquivos Offline temporários** -arquivos off-line temporários são cópias locais dos arquivos de rede usados recentemente. Esses arquivos são automaticamente armazenadas em cache para que você pode usá-los depois de se desconectar da rede. Um botão Exibir arquivos abre a pasta de arquivos Offline.

- **Arquivos offline** -Arquivos Offline são cópias locais dos arquivos de rede que você deseja ter disponível offline para que você pode usá-los depois de se desconectar da rede especificamente. Um botão Exibir arquivos abre a pasta de arquivos Offline.

- **Compactar arquivos antigos** -Windows podem compactar os arquivos que você não usou recentemente. Compactar arquivos economiza espaço em disco, mas você ainda pode usar os arquivos. Nenhum arquivo é excluído. Como eles são compactados em taxas diferentes, a quantidade de espaço em disco que você obterá exibida é aproximada. Um botão de opções permite que você especifique o número de dias de espera antes que a limpeza de disco compacta um arquivo não utilizado.

- **Arquivos de catálogo para o indexador de conteúdo** -o serviço de indexação acelera e melhora as pesquisas de arquivo, mantendo um índice dos arquivos que estão no disco. Esses arquivos de catálogo permanecem de uma operação de indexação anterior e podem ser excluídos com segurança. **Observação:** Arquivo de catálogo pode aparecer em mais de uma unidade, por exemplo, não apenas em % SystemRoot %.

**Observação** se você especificar a limpeza da unidade que contém a instalação do Windows, todas essas opções estão disponíveis na guia de limpeza de disco. Se você especificar qualquer outra unidade, somente a Lixeira e os arquivos de catálogo para opções de índice de conteúdo estão disponíveis na guia de limpeza de disco. 

## <a name="examples"></a>Exemplos

Para executar o aplicativo de limpeza de disco para que você possa usar sua caixa de diálogo para especificar opções para uso posterior, salvar as configurações para o conjunto **1**, digite o seguinte:

```
cleanmgr /sageset:1
```

Para executar a limpeza de disco e incluem as opções que você especificou com o comando de /sageset:1 cleanmgr, digite:

```
cleanmgr /sagerun:1
```

Para executar ```cleanmgr /sageset:1``` e ```cleanmgr /sagerun:1``` juntos, digite:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Referências adicionais

[Libere espaço em disco no Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
