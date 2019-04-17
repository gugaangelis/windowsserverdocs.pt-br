---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: "Visão geral de eliminação de duplicação de dados"
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 4344108f96d14475c15a31bd1ab917e7fc78ef9f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="data-deduplication-overview"></a>Visão geral de eliminação de duplicação de dados

> Aplicável a: Windows Server (canal semestral), Windows Server 2016

## <a name="what-is-dedup"></a>O que é a Eliminação de Duplicação de Dados?

A Eliminação de Duplicação de Dados, também conhecida como Eliminação de duplicatas, é um recurso do Windows Server 2016 que pode ajudar a reduzir o impacto de dados redundantes nos custos de armazenamento. Quando habilitada, a Eliminação de Duplicação de Dados otimiza o espaço livre em um volume examinando os dados no volume para procurar duplicatas no volume. As duplicatas do conjunto de dados do volume são armazenadas uma vez e (opcionalmente) são compactadas para economizar ainda mais espaço. A Eliminação de Duplicação de Dados otimiza redundâncias sem comprometer a fidelidade ou a integridade dos dados. Mais informações sobre como a Eliminação de Duplicação de Dados funciona podem ser encontradas na seção "[Como funciona a Eliminação de Duplicação de Dados?](understand.md#how-does-dedup-work)" da página [Noções básicas sobre Eliminação de Duplicação de Dados](understand.md).

> [!Important]  
> [O KB4025334](https://support.microsoft.com/kb/4025334) contém um pacote cumulativo de correções para Eliminação de Duplicação de Dados, incluindo correções de confiabilidade importantes; é altamente recomendável a instalação desse pacote durante o uso da Eliminação de Duplicação de Dados com o Windows Server 2016.

## <a name="why-is-dedup-useful"></a>Por que a Eliminação de Duplicação de Dados é útil?

A Eliminação de Duplicação de Dados ajuda os administradores de armazenamento a reduzir os custos associados a dados duplicados. Grandes conjuntos de dados geralmente têm **<u>muita</u>** duplicação, o que aumenta os custos de armazenamento de dados. Por exemplo:

- Os compartilhamentos de arquivos do usuário podem ter várias cópias dos mesmos arquivos ou de arquivos semelhantes.
- Os convidados de virtualização podem ser praticamente idênticos de VM para VM.
- Os instantâneos de backup podem ter algumas diferenças muito pequenas no dia a dia.

A economia de espaço que pode ser obtida com a Eliminação de Duplicação de Dados depende do conjunto de dados ou da carga de trabalho no volume. Os conjuntos de dados com alta duplicação podem ter taxas de otimização de até 95% ou uma redução de 20x na utilização de armazenamento. A tabela a seguir realça as economias típicas da eliminação de duplicação para vários tipos de conteúdo:

| Cenário       | Conteúdo                                        | Economia típica de espaço |
|----------------|------------------------------------------------|-----------------------|
| Documentos do usuário | Documentos do Office, fotos, música, vídeos, etc.  | 30% a 50%                |
| Compartilhamentos de implantação | Binários de software, arquivos cab, símbolos, etc. | 70% a 80%                |
| Bibliotecas de virtualização | ISOs, arquivos de disco rígido virtual, etc.  | 80% a 95%                |
| Compartilhamento geral de arquivos | Todos os anteriores                           | 50% a 60%                |

## <a id="when-can-dedup-be-used"></a>Quando a Eliminação de Duplicação de Dados pode ser usada?  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>Servidores de arquivos de finalidade geral</b><br />
Servidores de arquivo de finalidade geral são servidores de arquivos de uso geral que podem conter qualquer um dos seguintes tipos de compartilhamentos: <ul>
                    <li>Compartilhamentos de equipe</li>
                    <li>Pastas base de usuários</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">Pastas de trabalho</a></li>
                    <li>Compartilhamentos de desenvolvimento de software</li>
                </ul>
Servidores de arquivos de finalidade geral são bons candidatos para Eliminação de Duplicação de Dados, porque os vários usuários tendem a ter muitas cópias ou versões do mesmo arquivo. Os compartilhamentos de desenvolvimento de software se beneficiam da Eliminação de Duplicação de Dados, porque muitos binários permanecem essencialmente inalterados de um build para outro. 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">
                <b>Implantações de VDI (Virtual Desktop Infrastructure)</b><br />
Servidores VDI, como <a href="https://technet.microsoft.com/library/cc725560.aspx">Serviços da Área de Trabalho Remota</a>, fornecem uma opção simples para que as organizações provisionem áreas de trabalho para os usuários. Há muitas razões para que uma organização recorra a essa tecnologia: <ul>
                    <li><b>Implantação de aplicativos</b>: você pode implantar rapidamente aplicativos em sua empresa. Isso é particularmente útil quando você tem aplicativos que são atualizados com frequência, são usados raramente ou são difíceis de gerenciar.</li>
                    <li><b>Consolidação de aplicativos</b>: quando você instala e executa aplicativos em um conjunto de máquinas virtuais gerenciadas centralmente, você acaba com a necessidade de atualizar aplicativos em computadores cliente. Essa opção também reduz a quantidade de largura de banda de rede necessária para acessar os aplicativos.</li>
                    <li><b>Acesso remoto</b>: os usuários podem acessar aplicativos corporativos por meio de dispositivos, como computadores domésticos, quiosques, hardware que consomem pouca energia e sistemas operacionais diferentes do Windows.</li>
                    <li><b>Acesso a filial</b>: implantações de VDI podem fornecer melhor desempenho de aplicativos para funcionários de filiais que precisam de acesso a repositórios de dados centralizados. Às vezes, aplicativos que fazem uso intensivo de dados não têm protocolos de cliente/servidor que são otimizados para conexões de baixa velocidade.</li>
                </ul>
As implantações de VDI são ótimas candidatas para Eliminação de Duplicação de Dados, porque os discos rígidos virtuais que controlam as áreas de trabalho remotas para os usuários são essencialmente idênticos. Além disso, a Eliminação de Duplicação de Dados pode ajudar com os *problemas de inicialização de VDI*, a queda no desempenho de armazenamento quando muitos usuários fazem logon simultaneamente em suas áreas de trabalho para começar o dia.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>Destinos de backup, como aplicativos de backup virtualizado</b><br />
Aplicativos de backup, como o <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft DPM (Data Protection Manager)</a>, são excelentes candidatos para a Eliminação de Duplicação de Dados devido à duplicação significativa entre os instantâneos de backup.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Outras cargas de trabalho</b><br />
                [Outras cargas de trabalho também podem ser excelentes candidatos para a Eliminação de Duplicação de Dados](install-enable.md#enable-dedup-candidate-workloads).
            </td>
        </tr>
    </tbody>
</table>
