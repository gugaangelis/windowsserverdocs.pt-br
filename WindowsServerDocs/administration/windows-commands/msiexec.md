---
title: msiexec
description: Artigo de referência para o comando msiexec, que fornece os meios para instalar, modificar e executar operações no Windows Installer da linha de comando.
ms.topic: reference
ms.assetid: 122eb0ce-ecbc-4909-a52a-15c3413619af
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 93ce1de1f75ff03bc7bb5f79d2046502c2d81bc4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639613"
---
# <a name="msiexec"></a>msiexec

Fornece os meios para instalar, modificar e executar operações no Windows Installer da linha de comando.

## <a name="install-options"></a>Opções de instalação

Defina o tipo de instalação para iniciar um pacote de instalação.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe [/i][/a][/j{u|m|/g|/t}][/x] <path_to_package>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /i | Especifica a instalação normal. |
| /a | Especifica a instalação administrativa. |
| /ju | Anuncie o produto ao usuário atual. |
| /jm | Anuncie o produto a todos os usuários. |
| /j/g | Especifica o identificador de idioma usado pelo pacote anunciado. |
| /j/t | Aplica a transformação ao pacote anunciado. |
| /x | Desinstala o pacote. |
| `<path_to_package>` | Especifica o local e o nome do arquivo do pacote de instalação. |

#### <a name="examples"></a>Exemplos

Para instalar um pacote chamado *example.msi* da unidade C:, usando um processo de instalação normal, digite:

```
msiexec.exe /i "C:\example.msi"
```

## <a name="display-options"></a>Opções de exibição

Você pode configurar o que um usuário vê durante o processo de instalação, com base no seu ambiente de destino. Por exemplo, se você estiver distribuindo um pacote para todos os clientes para instalação manual, deve haver uma interface do usuário completa. No entanto, se você estiver implantando um pacote usando Política de Grupo, o que não requer interação do usuário, não deverá haver nenhuma interface de usuário envolvida.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe /i <path_to_package> [/quiet][/passive][/q{n|b|r|f}]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| `<path_to_package>` | Especifica o local e o nome do arquivo do pacote de instalação. |
| /quiet | Especifica o modo silencioso, o que significa que não há interação do usuário necessária. |
| /passive | Especifica o modo autônomo, o que significa que a instalação mostra apenas uma barra de progresso. |
| /qn | Especifica que não há interface do usuário durante o processo de instalação. |
| /qn + | Especifica que não há interface do usuário durante o processo de instalação, exceto para uma caixa de diálogo final no final. |
| /qb | Especifica que há uma interface do usuário básica durante o processo de instalação. |
| /QB + | Especifica que há uma interface do usuário básica durante o processo de instalação, incluindo uma caixa de diálogo final no final. |
| /qr | Especifica uma experiência de interface do usuário reduzida durante o processo de instalação. |
| /qf | Especifica uma experiência de interface do usuário completa durante o processo de instalação. |

##### <a name="remarks"></a>Comentários

- A caixa modal não será mostrada se a instalação for cancelada pelo usuário. Você pode usar **QB +!** ou **QB! +** para ocultar o botão **Cancelar** .

#### <a name="examples"></a>Exemplos

Para instalar o pacote *C:\example.msi*, usando um processo de instalação normal e nenhuma interface do usuário, digite:

```
msiexec.exe /i "C:\example.msi" /qn
```

## <a name="restart-options"></a>Opções de reinicialização

Se o pacote de instalação substituir arquivos ou tentar alterar arquivos que estão em uso, uma reinicialização poderá ser necessária antes da conclusão da instalação.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe /i <path_to_package> [/norestart][/promptrestart][/forcerestart]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| `<path_to_package>` | Especifica o local e o nome do arquivo do pacote de instalação. |
| /norestart | Interrompe a reinicialização do dispositivo após a conclusão da instalação. |
| /promptrestart | Solicita o usuário se uma reinicialização for necessária. |
| /forcerestart | Reinicia o dispositivo após a conclusão da instalação. |

#### <a name="examples"></a>Exemplos

Para instalar o pacote *C:\example.msi*, usando um processo de instalação normal sem reinicialização no final, digite:

```
msiexec.exe /i "C:\example.msi" /norestart
```

## <a name="logging-options"></a>Opções de log

Se você precisar depurar o pacote de instalação, poderá definir os parâmetros para criar um arquivo de log com informações específicas.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe [/i][/x] <path_to_package> [/L{i|w|e|a|r|u|c|m|o|p|v|x+|!|*}] <path_to_log>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /i | Especifica a instalação normal. |
| /x | Desinstala o pacote. |
| `<path_to_package>` | Especifica o local e o nome do arquivo do pacote de instalação. |
| /Li | Ativa o registro em log e inclui mensagens de status no arquivo de log de saída. |
| /lw | Ativa o registro em log e inclui avisos não fatais no arquivo de log de saída. |
| /le | Ativa o registro em log e inclui todas as mensagens de erro no arquivo de log de saída. |
| /la | Ativa o registro em log e inclui informações sobre quando uma ação foi iniciada no arquivo de log de saída. |
| /lr | Ativa o registro em log e inclui registros específicos da ação no arquivo de log de saída. |
| /lu | Ativa o registro em log e inclui informações de solicitação do usuário no arquivo de log de saída. |
| /lc | Ativa o registro em log e inclui os parâmetros iniciais da interface do usuário no arquivo de log de saída. |
| /LM | Ativa o registro em log e inclui informações de saída fatal ou de memória insuficiente no arquivo de log de saída. |
| /lo | Ativa o registro em log e inclui mensagens de espaço em disco insuficiente no arquivo de log de saída. |
| /lp | Ativa o registro em log e inclui propriedades de terminal no arquivo de log de saída. |
| /lp | Ativa o registro em log e inclui propriedades de terminal no arquivo de log de saída. |
| /lv | Ativa o registro em log e inclui a saída detalhada no arquivo de log de saída. |
| /lp | Ativa o registro em log e inclui propriedades de terminal no arquivo de log de saída. |
| /lx | Ativa o registro em log e inclui informações adicionais de depuração no arquivo de log de saída. |
| /l + | Ativa o registro em log e anexa as informações a um arquivo de log existente. |
| /l! | Ativa o registro em log e libera cada linha para o arquivo de log. |
| /l | Ativa o registro em log e registra todas as informações, exceto informações detalhadas (**/LV**) ou informações adicionais de depuração (**/LX**). |
| `<path_to_logfile>` | Especifica o local e o nome do arquivo de log de saída. |

#### <a name="examples"></a>Exemplos

Para instalar o pacote *C:\example.msi*, usando um processo de instalação normal com todas as informações de log fornecidas, incluindo a saída detalhada e armazenando o arquivo de log de saída em *C:\package.log*, digite:

```
msiexec.exe /i "C:\example.msi" /L*V "C:\package.log"
```

## <a name="update-options"></a>Opções de atualização

Você pode aplicar ou remover atualizações usando um pacote de instalação.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe [/p][/update][/uninstall[/package<product_code_of_package>]] <path_to_package>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /p | Instala um patch. Se estiver instalando silenciosamente, você também deverá definir a propriedade REINSTALLMODE como *ecmus* e reinstalar em *todos*. Caso contrário, o patch atualizará apenas o MSI armazenado em cache no dispositivo de destino. |
| /update | Opção instalar patches. Se você estiver aplicando várias atualizações, deverá separá-las usando um ponto-e-vírgula (;). |
| /pacote | Instala ou configura um produto. |

#### <a name="examples"></a>Exemplos

```
msiexec.exe /p "C:\MyPatch.msp"
msiexec.exe /p "C:\MyPatch.msp" /qb REINSTALLMODE="ecmus" REINSTALL="ALL"
msiexec.exe /update "C:\MyPatch.msp"
```

```
msiexec.exe /uninstall {1BCBF52C-CD1B-454D-AEF7-852F73967318} /package {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

Onde o primeiro GUID é o GUID do patch e o segundo é o código do produto MSI para o qual o patch foi aplicado.

## <a name="repair-options"></a>Opções de reparo

Você pode usar esse comando para reparar um pacote instalado.

### <a name="syntax"></a>Sintaxe

```
msiexec.exe [/f{p|o|e|d|c|a|u|m|s|v}] <product_code>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /fp | Repara o pacote se um arquivo estiver ausente. |
| /Fo | Repara o pacote se um arquivo estiver ausente ou se uma versão mais antiga estiver instalada. |
| /Fe | Repara o pacote se o arquivo estiver ausente ou se uma versão igual ou anterior estiver instalada. |
| /FD | Repara o pacote se o arquivo estiver ausente ou se uma versão diferente estiver instalada. |
| /fc | Repara o pacote se o arquivo estiver ausente ou se a soma de verificação não corresponder ao valor calculado. |
| /FA | Força todos os arquivos a serem reinstalados. |
| /Fu | Repara todas as entradas de Registro necessárias específicas do usuário. |
| /FM | Repara todas as entradas de Registro necessárias específicas do computador. |
| /FS | Repara todos os atalhos existentes. |
| /fc | Executa a partir da origem e armazena novamente em cache o pacote local. |

#### <a name="examples"></a>Exemplos

Para forçar a reinstalação de todos os arquivos com base no código de produto MSI a ser reparado, *{AAD3D77A-7476-469F-ADF4-04424124E91D}*, digite:

```
msiexec.exe /fa {AAD3D77A-7476-469F-ADF4-04424124E91D}
```

## <a name="set-public-properties"></a>Definir propriedades públicas

Você pode definir propriedades públicas por meio deste comando. Para obter informações sobre as propriedades disponíveis e como defini-las, consulte [Propriedades públicas](/windows/win32/msi/public-properties).

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [ Opções de linha de comandoMsiexec.exe](/windows/win32/msi/command-line-options)

- [Opções de linha de comando do instalador padrão](/windows/win32/msi/standard-installer-command-line-options)
