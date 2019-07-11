---
title: Considerações de uso de memória no ajuste de desempenho do AD DS
description: Uso de memória pelo processo de Lsass.exe em controladores de domínio que estão executando o Windows Server 2012 R2, 2016 e 2019.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799778"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considerações de uso de memória para o ajuste de desempenho do AD DS

Este artigo descreve algumas noções básicas do Local serviço LSASS (LSASS, também conhecido como o processo de Lsass.exe), as práticas recomendadas para a configuração de LSASS e expectativas para o uso de memória. Este artigo deve ser usado como um guia na análise de uso de memória e de desempenho do LSASS em controladores de domínio (DCs). As informações neste artigo podem ser útil se você tiver dúvidas sobre como ajustar e configurar os servidores e controladores de domínio para otimizar esse mecanismo.  

LSASS é responsável pelo gerenciamento da autenticação de domínio (LSA) da autoridade de segurança local e o gerenciamento do Active Directory. LSASS controla a autenticação de cliente e o servidor e também governa o mecanismo do Active Directory. LSASS é responsável para os seguintes componentes:  

- Autoridade de Segurança Local
- Serviço NetLogon
- Serviço de segurança de contas SAM (Gerenciador)
- Serviço do servidor de LSA
- Secure Sockets Layer (SSL)
- Protocolo de autenticação Kerberos v5
- Protocolo de autenticação NTLM
- Outros pacotes de autenticação que carregam na LSA

Serviços de banco de dados do Active Directory (NTDSAI.dll) funcionam com o mecanismo de armazenamento extensível (ESE, ESENT. dll).

Aqui está um diagrama visual LSASS do uso de memória em um controlador de domínio:

![Diagrama de componentes que usam a memória LSASS](media/domain-controller-lsass-memory-usage.png)  

A quantidade de memória que o LSASS usa em um controlador de domínio aumenta de acordo com o uso do Active Directory. Quando dados são consultados, ele é armazenado em cache na memória. Como resultado, é normal ver LSASS usando uma quantidade de memória que é maior que o tamanho do arquivo de banco de dados do Active Directory (NTDS. dit).

Conforme ilustrado no diagrama, o uso de memória LSASS pode ser dividido em várias partes, incluindo o cache de buffer de banco de dados do ESE, o repositório de versão do ESE e outros. O restante deste artigo fornece informações sobre cada uma dessas partes.

## <a name="ese-database-buffer-cache"></a>Cache de buffer do banco de dados ESE  
O maior uso da memória variável dentro do LSASS é o cache de buffer do banco de dados ESE. O tamanho do cache pode variar de menos de 1 MB para o tamanho do banco de dados inteiro. Como um cache maior melhora o desempenho, o mecanismo de banco de dados para o Active Directory (ESENT) tenta manter o cache tão grandes quanto possível. Enquanto o tamanho do cache varia de acordo com a pressão de memória no computador, o tamanho máximo do cache de buffer do banco de dados do ESE é *apenas* limitado pela RAM física instalado no computador. Não há nenhuma outra pressão de memória, desde que o cache pode aumentar para o tamanho do arquivo de banco de dados Ntds. dit de Active Directory. Quanto mais do banco de dados que pode ser armazenados em cache, melhor o desempenho do controlador de domínio será.  
  
> [!NOTE]
> Por causa da maneira que o algoritmo de cache do banco de dados funciona, em um sistema de 64 bits no qual o tamanho do banco de dados é menor do que a RAM disponível, o cache de banco de dados poderá ficar maior que o tamanho do banco de dados por 30 a 40 por cento.

## <a name="ese-version-store"></a>Repositório de versão do ESE

Há uso de memória variável com o repositório de versão do ESE (a parte vermelha no diagrama acima). A quantidade de memória usada depende se você tem o Windows Server 2019 ou versões mais antigas do Windows.

- Nas versões do Windows Server antes de 2019 do Windows Server, por padrão que LSASS pode usar até cerca de 400 MB de memória (dependendo do número de CPUs) em um computador de 64 bits para a versão do ESE armazene. Para obter mais informações sobre como o armazenamento de versão é usado consulte o seguinte blog ASKDS lançar por Ryan Ries: [A versão Store chamado e estiverem fora de Buckets](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- No Windows Server de 2019, isso é simplificado e quando o primeiro inicia o serviço NTDS, o tamanho de armazenamento de versão do ESE é calculado agora como 10% da RAM física, com um mínimo de 400MB e um máximo de 4GB. Para detalhes excelentes sobre isso e solução de problemas do repositório de versão, consulte outro blog excelente do Ryan Ries: [Aprofundamento: Active Directory ESE versão Store alterações no servidor de 2019](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510).

## <a name="other-memory-use"></a>Outro uso de memória

Por fim, há várias estruturas de dados de tamanho fixo (por exemplo, o cache de esquema), pilhas, heaps e código. A quantidade de memória que usa o LSASS pode variar, dependendo da carga no computador. Conforme aumenta o número de threads em execução, aumenta o número de pilhas. Em média, o LSASS usa 100 MB a 300 MB de memória para esses componentes fixas. Quando uma quantidade maior de RAM é instalada, o LSASS pode usar mais RAM e menos memória virtual.

**Limitar ou minimizar o número de programas no controlador de domínio ou adicione mais RAM, quando apropriado**

Para um desempenho ideal, o LSASS leva RAM possíveis em um determinado controlador de domínio. LSASS abandona essa memória RAM, pois outros processos pedem-lo. A ideia é otimizar o desempenho de LSASS enquanto ainda é responsável por outros processos que possam ser executadas em um computador. A lista de programas a observar inclui agentes de monitoramento. Alguns clientes têm agentes separados para várias funções de servidor que podem consumir recursos consideráveis de RAM. Alguns podem emitir várias consultas WMI, para os quais temos alguns detalhes abaixo.

Por causa disso e para aumentar o desempenho, é uma boa prática para limitar ou minimizar o número de programas em um controlador de domínio. Se não houver nenhuma solicitação de memória, o LSASS usa essa memória para armazenar em cache o banco de dados do Active Directory e, portanto, obter o melhor desempenho.

Quando você percebe que um controlador de domínio tem problemas de desempenho, inspecione também para processos com utilização significativa de memória. Eles podem ter um problema que você precisar solucionar problemas. Eles podem incluir componentes da Microsoft. Verifique se você acompanhe as recentes atualizações de manutenção&mdash;Microsoft inclui soluções para utilização excessiva de memória como parte das atualizações de qualidade, que também pode ajudar o desempenho do seu controlador de domínio.

Há recursos internos do sistema operacional que podem consumir RAM significativo dependendo do perfil de uso:

- **Servidor de arquivos**. Controladores de domínio também são servidores de arquivos para os compartilhamentos SYSVOL e Netlogon, diretiva de grupo e scripts para scripts de inicialização/logon e a política de manutenção.
  No entanto, podemos ver que os clientes usar controladores de domínio para outro conteúdo do arquivo de serviço. O servidor de arquivos SMB, em seguida, consumiria RAM para acompanhar os clientes do Active Directory, mas, o conteúdo do arquivo tornaria o cache de arquivos do sistema operacional crescer e competir com o cache de banco de dados ESE para RAM.  

- **Consultas WMI**. Soluções de monitoramento normalmente fazem muitas consultas WMI. Uma consulta individual pode ser barata executar. Geralmente é o volume de chamadas que gera uma sobrecarga, especialmente à medida que as soluções de monitoramento extrair novos eventos de vários logs de eventos que gerencia o Windows.  

  O log de eventos que produz o volume a maioria dos normalmente é o log de eventos de segurança. E isso também é o log de eventos que deseja que os administradores de segurança para coletar, especialmente de controladores de domínio.  

  O serviço WMI usa um esquema de alocação de memória dinâmica que otimizem as consultas. Portanto, o serviço WMI pode alocar muita memória, competindo novamente com o cache de banco de dados ESE.  
