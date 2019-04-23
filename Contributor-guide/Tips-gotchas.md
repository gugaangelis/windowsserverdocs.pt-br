# <a name="tips-and-gotchas"></a>Dicas e pegadinhas

Se você for novo no Git, aprender a trabalhar com eficiência pode ser frustrante. Informações são coletadas aqui, portanto, é mais fácil de encontrar que se ele está incluído dentro de outras informações neste guia.

## <a name="basics"></a>Noções básicas:

Comandos do Colaborador para este guia pressupõe que você configurou o Github para especificar o repositório padrão em que você extrair arquivos de. No Github é terminologia, onde você pode extrair arquivos do seu *upstream*. É onde você pode enviar arquivos para seus *origem*. 

Por causa em como nosso repositório e o fluxo de trabalho são criados, seu upstream deve ser definido como nosso repositório, que está sob a organização da Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr e sua origem deve ser seu fork deste repositório, que está em sua conta do Github. Por exemplo, https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr 

>Para verificar suas configurações, digite ```git config -l```. Examine as URLs para certificar-se de que eles se referir ao local que você pretendia.

Algumas dicas de ramificação básica:

-  Trabalhe em ramificações, não em sua cópia local do mestre. Isso torna mais fácil de se recuperar de problemas em suas ramificações e voltar para um ponto BOM e conhecido.

-  Base seus branches locais em um branch no repositório compartilhado, não na sua bifurcação remota. Em seguida, você enviará por push seus branches locais para sua bifurcação remota. Da sua bifurcação remota, você solicitar têm as atualizações na sua bifurcação mesclada em um repositório compartilhado. Os comandos neste artigo mostram como fazer isso.

-  Quando você cria uma ramificação local, seu comando diz ao Git quais branch que você deseja basear seu novo branch. Isso é quando e como você especifica mestre ou um branch de etapa ou recurso. Por exemplo, este comando cria uma nova ramificação de uma ramificação de upstream e, em seguida, faz check-out do novo branch, para que você está pronto para trabalhar nele:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

Para obter ajuda de markdown, começar com este [artigo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) no Github. Para tabelas, consulte [um](https://help.github.com/articles/organizing-information-with-tables/).

## <a name="gotchas"></a>Pegadinhas

### <a name="deleting-files"></a>Excluindo arquivos
Ele não funciona para excluir apenas um arquivo do seu sistema de arquivos. Use os comandos Git para informar ao sistema você pretendem fazer isso, caso contrário, seus arquivos excluídos provavelmente se tornará os arquivos de zumbi que reaparecem. E nunca em um bom momento. Afinal, este é um sistema de controle do código-fonte e se você não diga ele que realmente deseja eliminar esses arquivos, ele irá adicioná-los proveitosamente volta para você. Obrigado, Git! Para obter instruções, consulte [Renomeie ou remova um artigo](rename-r-retire.md).

### <a name="merge-conflicts"></a>Conflitos de mesclagem

Se você receber um conflito de mesclagem enquanto estiver trabalhando localmente, é preciso corrigi-lo ou de volta para fora dela. A grande maioria das vezes é melhor apenas corrigi-los, a menos que eles não são seus arquivos. Mantenha em mente se você não fizer nada, você poderá ser impedido de enviar as alterações para sua origem, portanto, você terá que fazer para algo.

**Como corrigir** – diretrizes do Azure tem mais informações sobre isso e instruções para resolver o conflito. **Uma solicitação de pull com conflitos de mesclagem nunca deve ser aceitas**. Certifique-se de substituir o nome do nosso repositório e ramificações em todos os exemplos de quando você examinar a [instruções](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx). 

**Como fazer** - - se os conflitos estão nos arquivos que você não possui ou há um motivo que você não é possível corrigi-los ao tempo, aqui como de volta. Isso redefine seu conjunto de arquivos local para como eles foram antes que o conflito de mesclagem ocorreu. Execute este comando:

```git merge --abort```

Se você ainda não tiver preparado e confirmado seu trabalho local, você pode fazer isso agora. No entanto, se você enviar por push as alterações e abrir uma solicitação pull, o conflito provavelmente reaparecerá se ele estava em arquivos com alterações. Você precisará corrigi-lo ou obtenha ajuda de outra pessoa para corrigi-lo, antes da solicitação pode ser aceito. Por isso, é melhor usar apenas o método para desbloqueá-la se você tiver trabalho urgente, como uma atualização crítica.

