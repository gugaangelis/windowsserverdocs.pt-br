---
title: Como determinar a área mínima de preparação necessária para o DFSR para uma pasta replicada
description: Este artigo é um guia de referência rápida sobre como calcular a área de preparação mínima necessária para que o DFSR funcione corretamente.
ms.prod: windows-server
ms.technology: server-general
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 5e5bfdbb90d2b3e631aaa020a173eca779f0d6b3
ms.sourcegitcommit: fa9a8badf4eb366aeeca7d2905e2cad711ee8dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84715000"
---
# <a name="how-to-determine-the-minimum-staging-area-dfsr-needs-for-a-replicated-folder"></a>Como determinar a área mínima de preparação necessária para o DFSR para uma pasta replicada

Este artigo é um guia de referência rápida sobre como calcular a área de preparação mínima necessária para que o DFSR funcione corretamente. Valores inferiores a esses podem fazer com que a replicação fique lenta ou pare completamente. 

Tenha em mente que estes são *apenas mínimos*. Ao considerar o tamanho da área de preparação, maior a área de preparo, até o tamanho da pasta replicada. Consulte a seção "como determinar se você tem um problema de área de preparação" e as postagens de blog vinculadas ao final deste artigo para obter mais detalhes sobre por que é importante ter uma área de preparo corretamente dimensionada.

> [!Note]
> Também temos um hotfix para ajudá-lo a calcular os tamanhos de preparo. [A atualização para a interface de gerenciamento do Replicação do DFS (DFSR) está disponível](https://support.microsoft.com/kb/2607047)

## <a name="rules-of-thumb"></a>Regras gerais

**Windows Server 2003 R2** – a cota da área de preparação deve ser tão grande quanto os 9 maiores arquivos na pasta replicada

**Windows Server 2008 e 2008 R2** – a cota da área de preparação deve ser tão grande quanto a 32 maior de arquivos na pasta replicada

A replicação inicial fará muito mais uso da área de preparo do que a replicação do dia a dia. A configuração da área de preparação maior do que o mínimo durante a replicação inicial é altamente incentivada se você tiver o espaço da unidade disponível

## <a name="where-do-i-get-powershell"></a>Onde obtenho o PowerShell?

O PowerShell está incluído no Windows 2008 e superior. Você deve instalar o PowerShell no Windows Server 2003. Você pode baixar o PowerShell para Windows 2003 [aqui](https://support.microsoft.com/kb/968930).

## <a name="how-do-you-find-these-x-largest-files"></a>Como você encontra esses X maiores arquivos?

Use um script do PowerShell para localizar os maiores arquivos 32 ou 9 e determinar quantos gigabytes eles somam (graças a preciso Pyle para os comandos do PowerShell). Na verdade, vou apresentar três scripts do PowerShell. Cada um é útil por conta própria; no entanto, o número 3 é o mais útil.

1. Execute o comando a seguir:  
   ```Powershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | ft name,length -wrap –auto
   ```
   
   Esse comando retornará os nomes de arquivo e o tamanho dos arquivos em bytes. Útil se você quiser saber quais são os 32 arquivos que são o maior na pasta replicada para que você possa "visitar" seus proprietários.

2. Execute o comando a seguir:  
   ```Poswershell
   Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   ```
   Esse comando retornará o número total de bytes dos 32 maiores arquivos na pasta sem listar os nomes de arquivo.

3. Execute o comando a seguir:  
   ```Poswershell
   $big32 = Get-ChildItem c:\\temp -recurse | Sort-Object length -descending | select-object -first 32 | measure-object -property length –sum
   
   $big32.sum /1gb
   ```
   Esse comando obterá o número total de bytes de 32 maiores arquivos na pasta e fará o cálculo para converter bytes em gigabytes para você. Esse comando é de duas linhas separadas. Você pode colá-los no Shell de comando do PowerShell de uma vez ou executá-los de volta para trás.

## <a name="manual-walkthrough"></a>Instruções manuais

Para demonstrar o processo e, felizmente, aumentar a compreensão do que estamos fazendo, vou percorrer manualmente cada parte.

A execução do comando 1 retornará resultados semelhantes à saída abaixo. Este exemplo usa apenas 16 arquivos para fins de brevidade. Sempre usar o 32 para o Windows 2008 e sistemas operacionais posteriores e o 9 para Windows 2003 R2

### <a name="example-data-returned-by-powershell"></a>Dados de exemplo retornados pelo PowerShell

<table>
<tbody>
<tr class="odd">
<td>Nome</td>
<td>Comprimento</td>
</tr>
<tr class="even">
<td><strong>File5.zip</strong></td>
<td>10286089216</td>
</tr>
<tr class="odd">
<td><strong>archive.zip</strong></td>
<td>6029853696</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>5751522304</td>
</tr>
<tr class="odd">
<td> <strong>file9.zip</strong></td>
<td>5472683008</td>
</tr>
<tr class="even">
<td><strong>MENTOS.zip</strong></td>
<td>5241586688</td>
</tr>
<tr class="odd">
<td><strong>File7.zip</strong></td>
<td>4321264640</td>
</tr>
<tr class="even">
<td><strong>file2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="odd">
<td><strong>frd2.zip</strong></td>
<td>4176765952</td>
</tr>
<tr class="even">
<td><strong>BACKUP.zip</strong></td>
<td>4078994432</td>
</tr>
<tr class="odd">
<td><strong>File44.zip</strong></td>
<td>4058424320</td>
</tr>
<tr class="even">
<td><strong>file11.zip</strong></td>
<td>3858056192</td>
</tr>
<tr class="odd">
<td><strong>Backup2.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="even">
<td><strong>BACKUP3.zip</strong></td>
<td>3815138304</td>
</tr>
<tr class="odd">
<td><strong>Current.zip</strong></td>
<td>3576931328</td>
</tr>
<tr class="even">
<td><strong>Backup8.zip</strong></td>
<td>3307488256</td>
</tr>
<tr class="odd">
<td><strong>File999.zip</strong></td>
<td>3274982400</td>
</tr>
</tbody>
</table>

### <a name="how-to-use-this-data-to-determine-the-minimum-staging-area-size"></a>Como usar esses dados para determinar o tamanho mínimo da área de preparação:

  - Nome = nome do arquivo.
  - Comprimento = bytes
  - Um gigabyte = 1073741824 bytes

Primeiro, você precisa somar o número total de bytes. Em seguida, divida o total por 1073741824. Sugiro usar o Excel ou sua planilha de escolha para fazer a matemática.

### <a name="example"></a>Exemplo

No exemplo acima do número total de bytes = 75241684992. Para obter a cota mínima da área de preparação necessária, preciso dividir 75241684992 por 1073741824.

> 75241684992/1073741824 = 70, 7 GB

Com base nesses dados, definirei minha área de preparação como 71 GB se eu arredondar para o número inteiro mais próximo.

### <a name="real-world-scenario"></a>Cenário do mundo real:

Embora uma explicação manual seja interessante, é provável que não seja o melhor uso do seu tempo para fazer a matemática por conta própria. Para automatizar o processo, use o comando 3 dos exemplos acima. Os resultados terão a seguinte aparência

> [![imagem](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/8204.image_thumb_02CB3914.png "image")](https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/58/02/metablogapi/0876.image_03A39EFE.png)

Usando o comando de exemplo 3 sem nenhum esforço extra, exceto para arredondar para o número inteiro mais próximo, posso determinar que preciso de uma cota de área de preparo de 6 GB para d: \\ docs.

## <a name="do-i-need-to-reboot-or-restart-the-service-for-the-changes-to-be-picked-up"></a>É necessário reinicializar ou reiniciar o serviço para que as alterações sejam coletadas?

As alterações na cota da área de preparação não exigem uma reinicialização ou reinicialização do serviço para entrar em vigor. Será necessário aguardar a replicação do AD e o ciclo de sondagem do AD do DFSR para que as alterações sejam aplicadas.

## <a name="how-to-determine-if-you-have-a-staging-area-problem"></a>Como determinar se você tem um problema de área de preparação

Você detecta problemas da área de preparação monitorando as IDs de eventos específicos nos servidores DFSR. A lista de eventos é 4202, 4204, 4206, 4208 e 4212. Os textos desses eventos estão listados abaixo. É importante distinguir entre 4202 e 4204 e os outros eventos. É possível registrar um número alto de eventos 4202 e 4204 em condições normais de operação. Imagine os eventos 4202 e 4204 como sendo análogos a pegar seu pulso, enquanto 4206, 4208 e 4212 são como problemas de conjunto. Explicarei abaixo como interpretar os eventos 4202 e 4204 abaixo.

### <a name="staging-area-events"></a>Eventos da área de preparação

> ID do evento: **4202**  
> Severidade: **aviso**
> 
> O serviço de Replicação do DFS detectou que o espaço de preparo em uso para a pasta replicada no caminho local (caminho) está acima da marca d' água alta. O serviço tentará excluir os arquivos de preparação mais antigos. O desempenho pode ser afetado.
> 
> ID do evento: **4204**  
> Gravidade: **informativo**
> 
> O serviço de Replicação do DFS excluiu com êxito arquivos de preparação antigos para a pasta replicada no caminho local (caminho). O espaço de preparo agora está abaixo da marca d' água alta.
> 
> ID do evento: **4206**  
> Severidade: **aviso**
> 
> O serviço de Replicação do DFS falhou ao limpar arquivos de preparação antigos para a pasta replicada no caminho local (caminho). O serviço pode falhar ao replicar alguns arquivos grandes e a pasta replicada pode ficar fora de sincronia. O serviço tentará automaticamente a limpeza do espaço de preparo em (x) minutos. O serviço pode iniciar a limpeza mais cedo se detectar que alguns arquivos de preparação foram desbloqueados.
> 
> ID do evento: **4208**  
> Severidade: **aviso**
> 
> O serviço Replicação do DFS detectou que o uso do espaço de preparo está acima da cota de preparo para a pasta replicada no caminho local (caminho). O serviço pode falhar ao replicar alguns arquivos grandes e a pasta replicada pode ficar fora de sincronia. O serviço tentará limpar o espaço de preparo automaticamente.
> 
> ID do evento: **4212**  
> Severidade: **erro**
> 
> O serviço de Replicação do DFS não pôde replicar a pasta replicada no caminho local (caminho) porque o caminho de preparo é inválido ou inacessível.

## <a name="what-is-the-difference-between-4202-and-4208"></a>Qual é a diferença entre 4202 e 4208?

Os eventos 4202 e 4208 têm texto semelhante; ou seja, o DFSR detectou que o uso da área de preparação excede a marca d' água alta. A diferença é que 4208 é registrado depois que a limpeza da área de preparação é executada e a cota de preparo ainda é excedida. 4202 é um evento normal e esperado, enquanto 4208 é anormal e requer intervenção.

## <a name="how-many-4202-4204-events-are-too-many"></a>Quantos eventos 4202, 4204 são muitos?

Não há uma resposta única para essa pergunta. Ao contrário dos eventos 4206, 4208 ou 4212, que são sempre ruins e indicam que a ação é necessária, os eventos 4202 e 4204 ocorrem em condições normais de operação. Ver muitos eventos 4202 e 4204 *pode* indicar um problema. Itens a serem considerados:

1.  O log de RF (pasta replicada) 4202 realizando a replicação inicial? Nesse caso, é normal registrar eventos 4202 e 4204. Você desejará manter isso em até o mínimo possível durante a replicação inicial, fornecendo o máximo possível de área de preparo
2.  Simplesmente verificar o número total de eventos 4202 não é suficiente. Você precisa saber quantas foram registradas por RF. Se você registrar em log 20 4202 eventos para uma RF em um período de 24 horas, isso é alto. No entanto, se você tiver 20 pastas replicadas e houver um evento por pasta, você estará bem.
3.  Você deve examinar vários dias de dados para estabelecer tendências.

Geralmente, eu recomendo que os clientes permitissem que não mais de 1 4202 eventos por pasta replicada por dia em condições normais de operação. "Normal", o que significa que nenhuma replicação inicial está ocorrendo. Baseie isso no raciocínio que:

1.  O tempo gasto na limpeza da área de preparação é o tempo gasto não replicando arquivos. A replicação é pausada enquanto a área de preparação é desmarcada.
2.  O DFSR se beneficia de uma área de preparação completa usando-a para RDC e RDC entre arquivos ou replicar os mesmos arquivos para outros membros
3.  Quanto mais eventos 4202 e 4204 você registrar mais as chances de que você se deparará com a condição em que o DFSR não consegue limpar a área de preparo ou terá de limpar prematuramente os arquivos da área de preparo.
4.  os eventos 4206, 4208 e 4212 são, em minha experiência, são precedidos e seguidos por um alto número de eventos 4202 e 4204.

Embora permitir apenas 1 4202 eventos por RF por dia seja conservador, ele diminui consideravelmente as chances de se executar problemas na área de preparação e utiliza melhor os recursos do seu servidor DFSR para a finalidade de replicar arquivos.


