# <a name="metadata-and-version-identifiers"></a>Identificadores de versão e metadados

Aqui está o que você precisa saber sobre marcas, controle de versão e metadados para os artigos no repositório windowsserverdocs-pr. Os autores de artigo são responsáveis por garantir que seus artigos aderem a esses padrões e requisitos.

## <a name="trademarks"></a>Marcas
Não use os símbolos de marca registrada após as referências de produto nos artigos na biblioteca técnica do. Eles não são necessários porque technet.microsoft.com, docs.microsoft.com e outros canais de publicação oficial da Microsoft incluem uma [Trademark](https://www.microsoft.com/trademarks) link de rodapé para uma lista de marcas comerciais da Microsoft. Esse link preenche o requisito legal. Para obter informações, consulte o guia de [CELA](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage), em "sites" e o guia de estilo de escrita do Microsoft [direitos autorais e marcas comerciais](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696) página, em "Páginas da Web em Microsoft.com" 

## <a name="versioning"></a>Controle de versão
Vários tipos de controle de versão se aplicam para os artigos neste repositório: 

-  Controle de versão que indica a versão do produto que um artigo se aplica a é feito de duas maneiras:
    - Manualmente em uma única linha no artigo para que fique visível como texto em um artigo publicado.
    - Por metadados, descrito abaixo.
-  Controle de versão de conteúdo do código-fonte é manipulada por meio do histórico do Github no arquivo. 

## <a name="metadata-structure-and-format"></a>Formato e estrutura de metadados

- Colocar os metadados na parte superior do arquivo, acima do título H1.
- Separe o bloco do restante do conteúdo do arquivo usando apenas três traços na primeira e última linhas do bloco. Não coloque qualquer outro texto nessas linhas.
- Coloque cada par de nome/valor de metadados em uma linha separada.
- Use um dos seguintes padrões de sintaxe, dependendo se os metadados requer valores predefinidos ou aceita valores personalizados. 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>Valores e nomes de metadados

Alguns metadados são necessário para todos os arquivos publicados como artigos na biblioteca técnica do TechNet, enquanto outros metadados é recomendado, mas não obrigatório. Em alguns casos, são permitidos apenas determinados valores para uma parte específica de metadados. 

|Nome|Valor|
|---|---|
|title|Valor personalizado que coincide com o título H1. Determina o que é exibido na guia do navegador.|
|descrição|Mostra os resultados da pesquisa e afeta a SEO. Incluir palavras-chave apropriadas, mas manter o comprimento para menos de 160 caracteres.|
|Autor|Alias de GitHub do autor do artigo principal|
|ms.author|Alias MSFT do desenvolvedor de conteúdo ou autor deste artigo responsável pela área de tecnologia abordada neste artigo.|
|ms.date|Data da última atualização de conteúdo. Não atualize para que as alterações somente metadados. Use o formato mm/dd/aaaa.|
|ms.prod|Identifica a versão do Windows Server para relatórios, BI com base em um modelo predefinido [valor](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969).|
|ms.technology|Identifica a área de tecnologia para relatórios, BI com base em um modelo predefinido [valor](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|GUID que identifica o artigo em todas as localidades para relatórios de BI. Artigos migrados de criação anteriores sistemas já ter isso. Para artigos criados no Github, use uma ferramenta como [ https://guidgenerator.com/ ](https://guidgenerator.com/).| 

## <a name="troubleshooting"></a>Solução de problemas

Os metadados que aparece na parte superior de um artigo publicado acontece quando o arquivo de origem tem erros de formatação. Alguns erros comuns são:

- Não tem hifens triplos na primeira e última linhas de bloco ou um número incorreto de hifens.
- Metadados não seguem a sintaxe necessária: \<nome\>:\<único espaço\>\<valor >

Em seguida, verifique o arquivo para esses e outros erros óbvios, envie uma PR para publicar o arquivo atualizado. Se você estiver parado, o alias de revisores de solicitação de pull de email: wssc-pra@microsoft.com

## <a name="see-also"></a>Consulte também
Os metadados usados neste repositório é com base nos metadados usados em serviços de conteúdo & internacional \(CSI\). Obter mais informações, incluindo metadados opcionais, estão disponível em [ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta).
Para recursos de business intelligence, consulte a equipe de BI do CSI [wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx).