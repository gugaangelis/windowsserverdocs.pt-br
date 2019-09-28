---
title: Considerações sobre o uso de memória no ajuste de desempenho AD DS
description: Uso de memória pelo processo Lsass. exe em controladores de domínio que executam o Windows Server 2012 R2, 2016 e 2019.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 55ac47d835874ddb8e160603f08cbafa985aad2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370292"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considerações de uso de memória para ajuste de desempenho AD DS

Este artigo descreve algumas noções básicas sobre o serviço LSASS (LSASs, também conhecido como o processo Lsass. exe), as práticas recomendadas para a configuração do LSASs e as expectativas de uso de memória. Este artigo deve ser usado como um guia na análise do desempenho do LSASs e do uso de memória em controladores de domínio (DCs). As informações neste artigo podem ser úteis se você tiver dúvidas sobre como ajustar e configurar servidores e controladores de dados para otimizar esse mecanismo.  

O LSASs é responsável pelo gerenciamento da autenticação de domínio da autoridade de segurança local (LSA) e do gerenciamento de Active Directory. O LSASs lida com a autenticação para o cliente e para o servidor e também governa o mecanismo de Active Directory. O LSASs é responsável pelos seguintes componentes:  

- Autoridade de Segurança Local
- Serviço NetLogon
- Serviço SAM (Gerenciador de contas de segurança)
- Serviço do servidor LSA
- Protocolo SSL (SSL)
- Protocolo de autenticação Kerberos V5
- Protocolo de autenticação NTLM
- Outros pacotes de autenticação que são carregados no LSA

Os serviços de banco de dados Active Directory (NTDSAI. dll) funcionam com o mecanismo de armazenamento extensível (ESE, ESENT. dll).

Este é um diagrama visual do uso de memória do LSASs em um DC:

![Diagrama dos componentes que usam a memória do LSASs](media/domain-controller-lsass-memory-usage.png)  

A quantidade de memória que o LSASs usa em um DC aumenta de acordo com o uso de Active Directory. Quando os dados são consultados, eles são armazenados em cache na memória. Como resultado, é normal ver o LSASs usando uma quantidade de memória maior que o tamanho do arquivo de banco de dados Active Directory (NTDS. dit).

Conforme ilustrado no diagrama, o uso de memória do LSASs pode ser dividido em várias partes, incluindo o cache de buffer do banco de dados ESE, o armazenamento de versão do ESE e outros. O restante deste artigo fornece informações sobre cada uma dessas partes.

## <a name="ese-database-buffer-cache"></a>Cache de buffer do banco de dados ESE  
O maior uso de memória variável dentro do LSASs é o cache de buffer do banco de dados ESE. O tamanho do cache pode variar de menos de 1 MB ao tamanho do banco de dados inteiro. Como um cache maior melhora o desempenho, o mecanismo de banco de dados para Active Directory (ESENT) tenta manter o cache o mais amplo possível. Embora o tamanho do cache varie de acordo com a pressão de memória no computador, o tamanho máximo do cache de buffer do banco de dados ESE é limitado *apenas* pela RAM física instalada no computador. Desde que não haja nenhuma outra pressão de memória, o cache pode aumentar para o tamanho do arquivo de banco de dados do Active Directory NTDS. dit. Quanto maior o banco de dados que pode ser armazenado em cache, melhor será o desempenho do DC.  
  
> [!NOTE]
> Devido à forma como o algoritmo de cache do banco de dados funciona, em um sistema de 64 bits no qual o tamanho do banco de dados é menor do que a RAM disponível, o cache do banco de dados pode aumentar maior do que o tamanho do banco de dados em 30 a 40%.

## <a name="ese-version-store"></a>Armazenamento de versão do ESE

Há um uso de memória variável pelo armazenamento de versão do ESE (a parte vermelha no diagrama acima). A quantidade de memória usada depende se você tem o Windows Server 2019 ou versões mais antigas do Windows.

- Nas 2019 versões do Windows Server, o LSASs, por padrão, pode usar até 400 MB de memória (dependendo do número de CPUs) em um computador de 64 bits para o armazenamento de versão do ESE. Para obter mais informações sobre como o armazenamento de versão é usado, consulte a seguinte postagem de blog do ASKDS por Ryan Ries: [O armazenamento de versão chamado e todos eles estão fora dos buckets](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- No Windows Server 2019, isso é simplificado e quando o serviço NTDS é iniciado pela primeira vez, o tamanho do armazenamento de versão do ESE agora é calculado como 10% da RAM física, com um mínimo de 400 MB e um máximo de 4 GB. Para obter ótimos detalhes sobre isso e solução de problemas de armazenamento de versão, consulte outro blog excelente de Ryan Ries: [Aprofunde-se: Active Directory alterações no repositório de versão do ESE no servidor 2019 @ no__t-0.

## <a name="other-memory-use"></a>Outro uso de memória

Por fim, há código, pilhas, heaps e várias estruturas de dados de tamanho fixo (por exemplo, o cache de esquema). A quantidade de memória que o LSASs usa pode variar, dependendo da carga no computador. À medida que o número de threads em execução aumenta, o número de pilhas de memória é aumentado. Em média, o LSASs usa 100 MB a 300 MB de memória para esses componentes fixos. Quando uma quantidade maior de RAM é instalada, o LSASs pode usar mais RAM e menos memória virtual.

**Limite ou minimize o número de programas em seu controlador de domínio ou adicione RAM adicional quando apropriado**

Para obter um desempenho ideal, o LSASs leva o máximo de RAM possível em um determinado DC. O LSASs abandona essa RAM como outros processos solicitam. A ideia é otimizar o desempenho do LSASs, embora ainda seja responsável por outros processos que possam ser executados em um computador. A lista de programas a serem observados inclui agentes de monitoramento. Alguns clientes têm agentes separados para várias funções de servidor que podem consumir recursos de RAM consideráveis. Alguns podem emitir muitas consultas WMI, para as quais temos alguns detalhes abaixo.

Por isso e para aumentar o desempenho, é uma boa prática limitar ou minimizar o número de programas em um controlador de domínio. Se não houver nenhuma solicitação de memória, o LSASs usará essa memória para armazenar em cache o banco de dados Active Directory e, portanto, obter o desempenho ideal.

Quando você observar que um controlador de domínio tem problemas de desempenho, observe também os processos com utilização de memória significativa. Eles podem ter um problema que você precisa solucionar. Eles podem incluir componentes da Microsoft. Certifique-se de acompanhar as atualizações de manutenção recentes @ no__t-0Microsoft inclui soluções para utilização excessiva de memória como parte das atualizações de qualidade, que também podem ajudar o desempenho do seu DC.

Há instalações internas do sistema operacional que podem consumir RAM significativa dependendo do perfil de uso:

- **Servidor de arquivos**. Os DCs também são servidores de arquivos para compartilhamentos de SYSVOL e Netlogon, atendendo a diretiva de grupo e scripts para política e scripts de inicialização/logon.
  No entanto, vemos que os clientes usam DCs para atender a outro conteúdo de arquivo. O servidor de arquivos SMB consumiria RAM para rastrear os clientes ativos, mas o conteúdo do arquivo faria com que o cache de arquivos do so crescesse e competiria com o cache do banco de dados ESE para RAM.  

- **Consultas WMI**. As soluções de monitoramento geralmente fazem muitas consultas WMI. Uma consulta individual pode ser barata para ser executada. Geralmente, é o volume de chamadas que incorre em sobrecarga, especialmente quando as soluções de monitoramento extraem novos eventos dos vários logs de eventos gerenciados pelo Windows.  

  O log de eventos que produz a maior parte do volume é normalmente o log de eventos de segurança. E esse também é o log de eventos que os administradores de segurança desejam coletar, especialmente de DCs.  

  O serviço WMI usa um esquema de alocação de memória dinâmica que otimiza as consultas. Portanto, o serviço WMI pode alocar muita memória, concorrendo novamente com o cache do banco de dados ESE.  
