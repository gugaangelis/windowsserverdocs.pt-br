<properties title="" pageTitle="Nomes de arquivo e locais de artigos técnicos sobre o Windows Server 2016" description="Explica a estrutura de arquivos para artigos e as convenções de nomenclatura que você deve seguir ao criar um novo artigo." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>Nomes de arquivo e locais de artigos técnicos do Windows Server

Sabendo e as regras a seguir ajudam a começar sua solicitação de pull aceita com mais rapidez.

+ [regras]
+ [Padrão]
+ [Aprovação de nome de arquivo]

## <a name="rules"></a>Regras

- Todos os arquivos devem estar no markdown e usar a extensão de arquivo. MD.
- Manter os nomes de arquivo para palavras de 3 a 5, se possível, palavras de 10 é realmente muito tempo para facilitar a leitura e SEO.
- Use letras minúsculas e apenas letras, números e hifens.
- Sem espaços ou caracteres de pontuação. Use um hífen para separar palavras e números no nome do arquivo.
- Usar a forma abreviada de verbos de ação, não o '-ing' formulário: `deploy-nano-server` não `Deploying-Nano-Server`
- Omitir palavras pequenas, como um e, a, no, ou.
- Palavras de ortografia. Evite acrônimos desnecessários ou não aprovados em nomes de arquivos
- Nomes de arquivo devem ser exclusivos - em vez de `overview.md` usar `storage-spaces-overview.md`

Acrônimos e inicialismos em nomes de arquivos - diretrizes específicas:

- Siga as diretrizes da Microsoft existente para abreviações de nomes aceitável
- Padrão da indústria abreviações são aceitáveis conforme necessário em nomes de arquivos.

## <a name="pattern"></a>Padrão

Examine a lista de artigos no repositório para ter uma ideia dos nomes existentes. Aqui está o padrão geral:

 **component-topic-title.md**
 
Por exemplo, `storage-spaces-direct-overview.md`

## <a name="file-name-approval"></a>Aprovação de nome de arquivo

Quando você envia um novo arquivo, os revisores de solicitação de pull examine o nome e fornecem comentários por meio do fluxo de comentários de solicitação de pull se forem necessárias alterações. O nome do arquivo precisa ser corrigido antes que a solicitação de pull for aceita. Colaboradores podem facilmente enviar por push a atualização para a solicitação pull pendente.

## <a name="folders-in-the-repo"></a>Pastas no repositório

Use a estrutura de pasta existente. Não crie pastas sem obter aprovação de um administrador do repositório. Fale com eles se você achar que precisa de uma nova pasta.

O repositório do GitHub deve ter um simples e relativamente simples de estrutura de pasta – \<área >\\\<tecnologia > – para eliminação de duplicação da storage\data de exemplo. Isso tem as seguintes vantagens:
 - É muito simple.
 - É mais próximo do funcionamento do Azure.com
 - Ela torna mais fácil para os gravadores/PMs localizar rapidamente seus tópicos – não há necessidade de me aprofundar em torno de outras pastas procurando em que uma tecnologia em particular está aninhada.
 - Ele mantém a URL curta, que é bom para SEO e experiência do usuário, os clientes, ocasionalmente, examinar a URL de indicações.

## <a name="changing-case-in-file-names"></a>Alterar maiusculas e minúsculas em nomes de arquivo

Os sistemas operacionais Windows diferenciam maiusculas de minúsculas. Se você precisar alterar um nome de arquivo para corrigir as maiusculas e minúsculas, é melhor fazer uma alteração para o conteúdo do arquivo, a menos que esteja em um Linux ou Mac. Por exemplo: 

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Use o `git mv` comando para renomear um arquivo:
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>Links do guia dos colaboradores

- [Índice de artigos com diretrizes](./contributor-guide-index.md)


<!--Anchors-->
[regras]: #rules
[Padrão]: #pattern
[Aprovação de nome de arquivo]: #file-name-approval
