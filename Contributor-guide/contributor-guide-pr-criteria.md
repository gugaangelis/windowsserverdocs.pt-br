# <a name="quality-criteria-for-pull-request-review"></a>Examine os critérios de qualidade para solicitação de pull

Solicitações de pull devem atender aos critérios mínimos seja aceito para integração com o repositório. Os revisores de solicitação de pull normalmente revisam apenas o que é nova ou alterada, usando o bloqueio e itens de revisão de qualidade sem bloqueio listados neste artigo para avaliar as alterações. Os revisores podem pedir para obter mais informações ou para um solicitante corrigir problemas antes de aceitar uma solicitação de pull. O solicitante é responsável por fazer as alterações.

>[!IMPORTANT] 
>Abrir uma solicitação pull dispara verificações de qualidade e a publicação de nosso site de preparo interno. É sua responsabilidade para examinar ambos, de links em comentários \(na guia conversa da solicitação pull, clique em **Mostrar todas as verificações** > **detalhes**\). Depois de fazer isso, indicá-lo adicionando a **"ready-to-merge"** rótulo para a PR. \(Clique em **rótulos** ou a engrenagem à direita do fluxo de comentários na PR.) Se você não pode adicionar o rótulo \(vêm colaboradores relataram isso), adicione 'ready-to-merge' um comentário para a solicitação de pull e enviar email para o alias do revisor, wssc-pra@microsoft.com.

## <a name="blocking-content-quality-items"></a>Itens de qualidade do conteúdo de bloqueio

As atualizações devem ser compatíveis com os seguintes critérios a serem mesclados.

| Category | Item de revisão de qualidade |
|----------|---------------------|
|Pré-requisitos| A solicitação de pull não tem nenhum:<br>-erros de validação ou avisos<br>-conflitos de mesclagem<br>-regressões óbvias de conteúdo<br>-repositório ou qualquer incomuns, estranhos arquivos inseridos|
|Integridade do repositório |Solicitação de pull contém menos de 100 arquivos alterados, a menos que ele está atualizando um branch de lançamento do mestre ou fazendo uma atualização em massa semelhantes, como uma correção de metadados. (Na verdade, uma PR deve conter muito menos do que isso, mas após 100 arquivos alterados, o GitHub não exibe as diferenças).|
|Integridade do repositório| Solicitação de pull não mesclada pela pessoa que criou a solicitação de pull.|
|nomenclatura |Nomes de arquivo para arquivos novos seguem a [diretrizes de nomenclatura de arquivo](file-names-and-locations.md).|
|nomenclatura |Novas pastas introduzidas no repositório seguem a [diretrizes de nomenclatura de pasta](file-names-and-locations.md#folder-names-in-the-repo).|
|Nomenclatura/SEO|O título H1 tem informações suficientes para descrever o conteúdo do artigo, para diferenciá-lo de outros artigos e para mapear para palavras-chave prováveis do cliente. Por exemplo, um título H1 de "Visão geral" não atendem a esse critério.|
|Conteúdo|Ele é um documento técnico. Consulte a [o quê vai onde diretrizes](content-channel-guidance.md).|
|Conteúdo|Ele tem um parágrafo introdutório e um corpo processuais ou conceitual do conteúdo. Ele tem conteúdo suficiente para ser considerado um artigo e não é um pequeno fragmento de informações.|
|Conteúdo| Ele segue o padrão de formatação e design do conteúdo: elementos que devem ser listas numeradas são numerados, elementos que devem ser listas não ordenados são usados marcadores. Isso é o uso básico.|
|Conteúdo| Ele tem nenhum gráficos incomuns, arquitetura de informações ou estruturas ou designs obviamente personalizados.|
|Funcionalidade do site/design| O artigo não tem um Sumário criado manualmente dentro do conteúdo. O artigo deve depender de H2s para seu Sumário na página.|
|Funcionalidade do site/design| Se forem usados os títulos H2, há pelo menos duas. Qualquer cabeças H3 são precedidas por um título H2. Usando um título H2 cria um Sumário do artigo de item único. Use os títulos H2 antes de um título H3 para assegurar a que criação de um Sumário.|
|Markdown| Artigo não contém nenhum bloco de HTML; é permitido secundário embutido HTML – como sobrescrito, subscrito, caracteres especiais e outras formatações secundárias que não dá suporte a Markdown. Tabelas HTML são permitidas apenas se a tabela contém com marcadores ou listas numeradas, mas que geralmente indica que o conteúdo pode ser simplificado para que ele possa ser codificado em Markdown.|
|Markdown   |Elementos de markdown personalizados são usados quando apropriado. Por exemplo: Anotações são codificadas usando o! Observe a extensão, não como texto sem formatação.|
|Imagens|As imagens incluem texto alt. **Esse é um requisito de acessibilidade que deve ser preenchido como parte dos critérios de liberação.** [As diretrizes aqui](https://worldready.cloudapp.net/Styleguide/Read?id=2665&topicid=28349). |
|Imagens |GIFs animados não são aceitos no repositório.|
|Imagens | Imagens têm resolução clara, estão livres de palavras escritas incorretamente e não contêm informações privadas [consulte as diretrizes de imagem](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md). |
|Preparando| O artigo tem sido visualizado na preparação e não conter problemas óbvios de formatação:<br>-Uma lista numerada ou com marcadores que aparece como um parágrafo <br> -Código em um bloco de código que aparece parcialmente no bloco de código e parcialmente fora dele <br>– Etapas numeradas incorretamente devido a um recuo com falha|

## <a name="non-blocking-content-quality-items"></a>Sem-bloqueio itens de qualidade de conteúdo

Para esses itens, os revisores de solicitação de pull fornecem comentários e instruções para o autor realizar correções nas próximas solicitações de pull. No entanto, esse comentário não bloqueia a decisão de mesclagem. Os autores devem corrigir nos 3 dias úteis.

| Category | Item de revisão de qualidade |
|----------|---------------------|
|Conteúdo|Os artigos devem ter "próximas etapas" no final com 1 a 3 relevantes e atraentes próximas etapas. Breve texto deve ser incluído que ajuda os clientes a entender por que as próximas etapas são relevantes. (Somente novos artigos).
|Imagens|As imagens usam o estilo do texto explicativo corretos e a cor e capturas de tela usam o estilo de borda e o espaço reservado correto. [Consulte as diretrizes de imagem](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Funcionalidade do site/design|Os títulos H2, quando renderizados no Sumário na página, o ideal é que devem quebrar a não mais de 2 linhas. Títulos mais dificultar o Sumário do artigo verificar.|
|Convenções de estilo|Todos os títulos e cabeçalhos são diferenciam maiusculas de minúsculas.|
|Processo|Quando você excluir ou renomear um artigo, certifique-se de que seguir o processo. Os revisores devem adicionar o comentário e o link a seguir em um comentário de solicitação de pull:<br><br>*Verifique se que você seguiu o processo no guia do Colaborador: [Altere o nome de um artigo ou excluí-lo](rename-or-retire.md).*|
