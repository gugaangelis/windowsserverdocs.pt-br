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
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081773"
---
# <a name="metadata-and-markdown-template"></a>Metadados e redução de modelo

Este modelo de operações contém exemplos de sintaxe de redução, bem como orientação sobre como definir os metadados. Para obter o máximo proveito dele, você deve ver a [Redução bruta](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) e o [renderizados o modo de exibição](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (A redução bruta mostra o bloco de metadados, enquanto o modo de exibição renderizado não.)

Ao criar um arquivo de redução, você deve copiar este modelo para um novo arquivo, preencha os metadados como um conjunto especificado abaixo, o cabeçalho de S1 acima para o título do artigo e excluir o conteúdo. Qualquer coisa CAPS entre colchetes requer sua atenção.


## <a name="metadata"></a>Metadados 

O bloco de metadados total está acima. Algumas observações importantes:

- Você **deve** ter um espaço entre os dois-pontos (:) e o valor de um elemento de metadados.
- Em um valor (por exemplo, um título)-e-vírgula quebra o analisador de metadados. Em seu lugar, use a codificação de HTML para dois-pontos de `&#58;` (por exemplo, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **título**: esse título aparecerá nos resultados de mecanismo de pesquisa. 
- **autor**: campo Autor deve conter o **GitHub username** do autor, não seu alias.
- **MS.prod**, **ms.technology**: usar "limite de servidor windows" para ms.prod (ou w10 se você estiver usando esse modelo para criar conteúdo para Windows 10). Converse com seu contato CX para obter o valor de ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Redução básica, GFM e caracteres especiais

Redução de básicos e GitHub-flavored todos é suportado. Para obter mais informações sobre estes, consulte:

- [Sintaxe de redução de linha de base](https://daringfireball.net/projects/markdown/syntax)
- [Documentação de redução (GFM) GitHub-flavored](https://guides.github.com/features/mastering-markdown)

Redução usa caracteres especiais, como \ *, \', e \ # para formatação. Se você desejar incluir um dos seguintes caracteres em seu conteúdo, você deve fazer uma das duas coisas:

- Colocar uma barra invertida antes que o caractere especial "escape" ela (por exemplo, \ \ \ * para um \ *)
- Use o [código HTML da entidade](http://www.ascii.cl/htmlcodes.htm) do caractere (por exemplo, \ & \#42\; por um & #42;).

## <a name="headings"></a>Títulos

Títulos devem ser feitos ou seja, usando o estilo atx, use caracteres de hash de 1 a 6 (#) no início da linha para indicar um título, correspondendo aos níveis de títulos HTML S1 até H6. Exemplos de cabeçalhos de primeiro e segundo nível são usados acima. 

Há **deve** ser apenas um título de primeiro nível (S1) no seu tópico, que será exibido como o título na página.  

Títulos de segundo nível irá gerar um Sumário na página que aparece na seção "neste artigo" sob o título na página.

### <a name="third-level-heading"></a>Título de terceiro nível
#### <a name="fourth-level-heading"></a>Título do quarto nível
##### <a name="fifth-level-heading"></a>Título da quinto nível
###### <a name="sixth-level-heading"></a>Título sexto nível

## <a name="text-styling"></a>Estilo do texto

*Itálico* 

**Negrito** 

~~Tachado~~

## <a name="links"></a>Links

### <a name="internal-links"></a>Links internos

Para vincular a um cabeçalho no mesmo arquivo de redução, exibir a origem do artigo publicado, localize a ID do cabeçalho (por exemplo, `id="blockquote"`) e usando # + id de link (por exemplo, `#blockquote`).

- Exemplo: [Blockquotes](#blockquote)

Para vincular a um arquivo de redução em repo o mesmo, use [links relativos](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), incluindo o ".md" ao final do nome de arquivo.

- Exemplo: [dicas e armadilhas](tips-gotchas.md)
- Exemplo: a [instalação para colaboradores e ferramentas](../readme.md)

Para vincular a um cabeçalho em um arquivo de redução em repo o mesmo, use a vinculação relativa + hashtag vinculação.

- [Excluindo arquivos](tips-gotchas.md#deleting-files) de exemplo:

### <a name="external-links"></a>Links externos

Para vincular a um arquivo externo, use a URL completa, como o link.

- Exemplo: [GitHub](http://www.github.com)

Se uma URL for exibido em um arquivo de redução, ele será transformado em um único link clicável.

- Exemplo:http://www.github.com

## <a name="lists"></a>Listas

### <a name="ordered-lists"></a>Listas com marcadores

1. Isso 
1. É
1. Um
1. Ordenados
1. Lista  


#### <a name="ordered-list-with-an-embedded-list"></a>Lista com uma lista incorporada ordenada

1. Aqui
1. vem
1. um
1. incorporado
    1. Erro de Scarlett
    1. Ameixa professor
1. ordenados
1. list


### <a name="unordered-lists"></a>Listas não ordenadas

- Isso
-  está 
- a
- com marcadores
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Lista sem marcadores com uma lista incorporada

- Isso 
- com marcadores 
- list
    - Pavão Sra.
    - Verde SR.
- contém  
- outros
    1. Colonel Mustard
    1. White Sra.
- listas


## <a name="horizontal-rule"></a>Régua horizontal

---

## <a name="tables"></a>Tabelas

Em quase todas as instância, use MD formatação de tabelas. Enquanto tabelas HTML oferecem mais flexibilidade faça não podemos usá-los em nosso conteúdo. Se você tiver uma tabela HTML em seu artigo, nós não será mesclado desse artigo.

| Tabelas        | São           | Fresco  |
| ------------- |:-------------:| -----:|
| é col 3      | alinhado à direita | US $1600 |
| é Col 2      | centralizado      |   US$ 12 |
| Col 1 é o padrão | alinhado à esquerda     |    $1 |


## <a name="code"></a>Código

### <a name="generic-codeblock"></a>Codeblock genérico

Recuo espaços código quatro para a codificação de codeblock genérico.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks com o identificador de idioma

Usar três backticks (& #96; & #96; & #96;) + a codificação de uma ID de idioma para aplicar a cor de idioma específico a um bloco de código.  Aqui está a lista completa de [IDs de idioma GitHub Flavored redução (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C & #9839;

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

Use backticks (& #96;) para `inline code`.

## <a name="blockquotes"></a>Blockquotes

> O drought tinha duração agora para milhões de dez anos, e o Reinado dos terrível lizards tempo já teve encerrados. Aqui no Equador, no continente que seria um dia ser conhecido como África, batalha existência havia atingido um novo climax de ferocity e o victor ainda não estava visíveis. Na land ambiente e desiccated, somente o pequeno ou swift ou o acirrada poderia florescer ou até mesmo, esperamos sobreviver.

## <a name="images"></a>Images

### <a name="static-image"></a>Imagem estática

![Este é o texto alt](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Imagem vinculada

[![atexto de lt para imagem vinculada](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Alertas

### <a name="note"></a>Observação

> [!NOTE]
> Esta é nota

### <a name="warning"></a>Aviso

> [!WARNING]
> Isso é aviso

### <a name="tip"></a>Dica

> [!TIP]
> Esta é dica

### <a name="important"></a>Importante

> [!IMPORTANT]
> Isso é importante

