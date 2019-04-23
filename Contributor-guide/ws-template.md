---
title:
- TÍTULO DO ARTIGO
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879557"
---
# <a name="metadata-and-markdown-template"></a>Metadados e o modelo de Markdown

Esse modelo OPS contém exemplos de sintaxe de Markdown, bem como diretrizes sobre como definir os metadados. Para obter o máximo dela, você deve exibir o [Markdown bruto](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) e o [exibição renderizada](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (O Markdown bruto mostra o bloco de metadados, enquanto a exibição renderizada não.)

Ao criar um arquivo Markdown, você deve copiar este modelo para um novo arquivo, preencher os metadados conforme especificado abaixo, definir o cabeçalho H1 acima para o título do artigo e excluir o conteúdo. Qualquer coisa no CAPS colchetes requer sua atenção.


## <a name="metadata"></a>Metadados 

O bloco de metadados completo está acima. Algumas observações importantes:

- Você **deve** tiver um espaço entre os dois-pontos (:) e o valor de um elemento de metadados.
- Dois-pontos em um valor (por exemplo, um título) quebram o analisador de metadados. Em seu lugar, usar a codificação de HTML para um de dois-pontos `&#58;` (por exemplo, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **title**: Esse título aparecerá nos resultados da pesquisa. 
- **author**: O campo Autor deve conter o **nome de usuário do GitHub** do autor, não seu alias.
- **ms.prod**, **ms.technology**: Use "windows-server-threshold" MS. prod (ou w10 se você estiver usando este modelo para criar conteúdo para o Windows 10). Fale com seu contato CX para obter o valor de MS. Technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Markdown básica, GFM e caracteres especiais

Há suporte para todos os Markdown básico e do tipo GitHub. Para obter mais informações sobre isso, consulte:

- [Sintaxe de Markdown de linha de base](https://daringfireball.net/projects/markdown/syntax)
- [Documentação de GFM (Markdown) para GitHub](https://guides.github.com/features/mastering-markdown)

Markdown usa caracteres especiais, como \*, \`, e \# para formatação. Se você quiser incluir um desses caracteres em seu conteúdo, você deve fazer uma das duas coisas:

- Coloque uma barra invertida antes do caractere especial para "escapar" ele (por exemplo, \\ \* para um \*)
- Use o [código de entidade HTML](http://www.ascii.cl/htmlcodes.htm) o caractere (por exemplo, \& \#42\; para um &#42;).

## <a name="headings"></a>Títulos

Títulos devem ser feitos usando o atx-style, ou seja, usar o 1 a 6 caracteres de hash (#) no início da linha para indicar um cabeçalho correspondente aos níveis de cabeçalho HTML H1 a H6. Exemplos de cabeçalhos de primeiro e segundo nível são usados acima. 

Lá **deve** ser apenas um título de primeiro nível (H1) no tópico, que será exibido como o título da página.  

Títulos de segundo nível gerarão o Sumário na página que aparece na seção "neste artigo" sob o título da página.

### <a name="third-level-heading"></a>Título de terceiro nível
#### <a name="fourth-level-heading"></a>Título de quarto nível
##### <a name="fifth-level-heading"></a>Título de quinto nível
###### <a name="sixth-level-heading"></a>Título de sexto nível

## <a name="text-styling"></a>Estilo do texto

*Itálico* 

**Negrito** 

~~Tachado~~

## <a name="links"></a>Links

### <a name="internal-links"></a>Links internos

Para vincular a um cabeçalho no mesmo arquivo Markdown, exiba a origem do artigo publicado, localize a ID do cabeçalho (por exemplo, `id="blockquote"`) e vincule usando # + id (por exemplo, `#blockquote`).

- Exemplo: [Blockquotes](#blockquote)

Para vincular a um arquivo Markdown no mesmo repositório, use [links relativos](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), incluindo ". MD" ao final do nome de arquivo.

- Exemplo: [Dicas e armadilhas](tips-gotchas.md)
- Exemplo: [Ferramentas e configuração para colaboradores](../readme.md)

Para vincular a um cabeçalho em um arquivo Markdown no mesmo repositório, use link relativo + vinculação por hashtag.

- Exemplo: [Excluindo arquivos](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Links externos

Para vincular a um arquivo externo, use a URL completa como o link.

- Exemplo: [GitHub](http://www.github.com)

Se aparecer uma URL em um arquivo Markdown, ela será transformada em um link clicável.

- Exemplo: http://www.github.com

## <a name="lists"></a>Listas

### <a name="ordered-lists"></a>Listas ordenadas

1. Esse 
1. Is
1. Uma
1. Ordenado
1. List  


#### <a name="ordered-list-with-an-embedded-list"></a>Lista ordenada com uma lista inserida

1. Aqui
1. é fornecido
1. an
1. Inserido
    1. Inserida
    1. Professor Black
1. Ordenado
1. lista


### <a name="unordered-lists"></a>Listas não ordenadas

- Esse
- está
- $nbsp;
- com marcadores
- lista


##### <a name="unordered-list-with-an-embedded-list"></a>Lista não ordenada com uma lista inserida

- Esse 
- com marcadores 
- lista
    - Dona violeta
    - SR. marinho
- Contém  
- outros
    1. Coronel Mostarda
    1. Dona branca
- Listas


## <a name="horizontal-rule"></a>Régua horizontal

---

## <a name="tables"></a>Tabelas

Em quase todas as instâncias, use MD formatação para tabelas. Embora tabelas HTML fornecem mais flexibilidade nós não usá-los em nosso conteúdo. Se você tiver uma tabela HTML em seu artigo, não podemos mesclará desse artigo.

| Tabelas        | São           | Fresco  |
| ------------- |:-------------:| -----:|
| a col. 3 está      | alinhado à direita | $1600 |
| a col. 2 está      | centralizado      |   US$ 12 |
| Col 1 é o padrão | alinhado à esquerda     |    $1 |


## <a name="code"></a>Código

### <a name="generic-codeblock"></a>Codeblock genérico

Recue espaços quatro do código para a codificação de codeblock genérico.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks com o identificador de idioma

Use três backticks (&#96;&#96;&#96;) + uma ID de idioma para aplicar cores específicas a um idioma de codificação para um bloco de código.  Aqui está a lista inteira de [IDs dos idiomas Flavored Markdown GFM (GitHub)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C&#9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>Código embutido

Use backticks (&#96;) para `inline code`.

## <a name="blockquotes"></a>Blockquotes

> A seca já durava dez milhões de anos, e o Reino dos terríveis lagartos muito tempo tiver terminado. Aqui no Equador, no continente que um dia seria conhecido como África, a batalha pela existência atingiu um novo clímax de ferocidade, e o vencedor ainda não estava visíveis. Este Terra estéril e deserta, apenas os pequenos ou velozes ou selvagens poderiam prosperem ou até mesmo ter esperança de sobreviver.

## <a name="images"></a>Imagens

### <a name="static-image"></a>Imagem estática

![Este é o texto alt](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Imagem vinculada

[![texto ALT para imagem vinculada](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Alertas

### <a name="note"></a>Observação

> [!NOTE]
> Isso é Observação

### <a name="warning"></a>Aviso

> [!WARNING]
> Isso é um aviso

### <a name="tip"></a>Dica

> [!TIP]
> Isso é TIP

### <a name="important"></a>Importante

> [!IMPORTANT]
> Isso é importante

