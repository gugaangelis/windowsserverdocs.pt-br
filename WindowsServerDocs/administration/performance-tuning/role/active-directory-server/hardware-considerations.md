---
title: Considerações de hardware no ajuste de desempenho do AD
description: Considerações de hardware no ajuste de desempenho do AD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866087"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considerações de hardware no ajuste de desempenho do ADDS 

>[!Important]
> A seguir está um resumo da chave recomendações e considerações para otimizar o hardware de servidor para cargas de trabalho do Active Directory abordado em mais detalhes na [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) artigo. Os leitores são altamente incentivados a revisar [planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) para uma maior compreensão técnica e as implicações dessas recomendações.

## <a name="avoid-going-to-disk"></a>Evitar o início para o disco

Active Directory armazena em cache o máximo do banco de dados como memória permite. Ao buscar páginas de memória são ordens de magnitude mais rapidamente do que ir para a mídia física, se a mídia está eixo ou baseado em SSD. Adicione mais memória para minimizar a e/s de disco.

-   Práticas recomendadas do Active Directory recomendável colocar RAM suficiente para carregar a DIT inteira na memória, além de acomodar o sistema operacional e outros aplicativos instalados, como um software antivírus, backup, monitoramento e assim por diante.

    -   Para limitações de plataformas herdadas, consulte [uso de memória pelo processo de Lsass.exe em controladores de domínio que executam o Windows Server 2003 ou Windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Usar a memória\\em longo prazo em espera Cache de tempo de vida médio (s) &gt; contador de desempenho de 30 minutos.

-   Colocar o sistema operacional, logs e o banco de dados em volumes separados. Se todos ou a maioria da DIT pode ser armazenados em cache, depois que o cache está pronta e em um estado estável, isso se torna menos relevante e oferece um pouco mais flexibilidade no layout de armazenamento. Em cenários onde a DIT toda não pode ser armazenados em cache, a importância de dividir o sistema operacional, logs e o banco de dados em volumes separados se torna mais importante.

-   Normalmente, taxas de e/s para a DIT são cerca de 90% de leitura e gravação de 10%. Cenários em que os volumes de e/s de gravação significativamente excedem 10 a 20% são considerados pesada de gravação. Cenários com uso intenso de gravação não se beneficiam muito do cache do Active Directory. Para garantir a durabilidade transacional dos dados que são gravados no diretório, do Active Directory não executa o cache de gravação em disco. Em vez disso, ele confirma todas as operações de gravação no disco antes de retornar um status de conclusão com êxito para uma operação, a menos que haja uma solicitação explícita para não fazer isso. Portanto, rápida e/s de disco é importante para o desempenho de operações de gravação para o Active Directory. A seguir estão as recomendações de hardware que podem melhorar o desempenho para esses cenários:

    -   Controladores RAID de hardware

    -   Aumentar o número de discos de baixa latência/alta RPM hospedando os DIT e arquivos de log

    -   Gravação em cache no controlador

-   Examine o desempenho do subsistema de disco individualmente para cada volume. A maioria dos cenários do Active Directory são predominantemente baseado na leitura, assim, as estatísticas no volume que hospeda a DIT são os mais importantes inspecionar. No entanto, não subestime o restante das unidades, incluindo o sistema operacional, de monitoramento e unidades de arquivos de log. Para determinar se o controlador de domínio está configurado corretamente para evitar o armazenamento que está sendo o gargalo de desempenho, consulte a seção em subsistemas de armazenamento para recomendações de armazenamento padrão. Em muitos ambientes, a filosofia é garantir que haja espaço suficiente principal para acomodar as explosões ou picos de carga. Esses limites de aviso limites em que o espaço disponível para acomodar as explosões ou picos de carga se torna restrito e capacidade de resposta do cliente degrada. Em resumo, exceder esses limites não é ruim a curto prazo (algumas vezes ao dia 5 a 15 minutos), no entanto um sistema de execução prolongada com esses tipos de estatísticas está totalmente, não armazenando em cache o banco de dados e pode ser sobre impostos e deve ser investigados.

    -   Banco de dados = =&gt; Instances(lsass/NTDSA)\\latência média de leituras de banco de dados de e/s &lt; gt;15 MS

    -   Banco de dados = =&gt; Instances(lsass/NTDSA)\\banco de dados de e/s de leituras/s &lt; 10

    -   Banco de dados = =&gt; Instances(lsass/NTDSA)\\latência média de gravações no Log de e/s &lt; 10 MS

    -   Banco de dados = =&gt; Instances(lsass/NTDSA)\\e/s de Log gravações/s – informativa apenas.

        Para manter a consistência dos dados, todas as alterações devem ser gravadas no log. Há um número de BOM ou ruim aqui, ele é apenas uma medida de quanto o armazenamento que dão suporte a.

-   Planeje as cargas de e/s de disco não essenciais, como exames de antivírus e de backup por períodos de pico de carga. Além disso, use soluções de backup e antivírus que suportam o recurso de e/s de baixa prioridade introduzido no Windows Server 2008 para reduzir a competição com as necessidades de e/s do Active Directory.

## <a name="dont-over-tax-the-processors"></a>Não mais de imposto sobre os processadores

Processadores que não têm ciclos livres suficientes podem causar tempos de espera longos na obtenção de threads para o processador para execução. Em muitos ambientes, a filosofia é garantir que haja espaço suficiente principal para acomodar as explosões ou picos de carga para minimizar o impacto na capacidade de resposta do cliente nesses cenários. Em resumo, excedendo o abaixo dos limites não é ruim a curto prazo (5 a 15 minutos algumas vezes ao dia), porém um sistema de execução prolongada com esses tipos de estatísticas não fornece qualquer cabeçalho mais espaço para acomodar cargas anormais e pode facilmente ser colocados em um excesso s mais detalhes cenário. Períodos prolongados acima dos limites de gastos de sistemas devem ser investigados para como reduzir a carga do processador.

-   Para obter mais informações sobre como selecionar um processador, consulte [ajuste de desempenho para o Hardware de servidor](../../hardware/index.md).

-   Adicionar hardware, otimizar a carga, direcionar os clientes em outro lugar ou remover a carga do ambiente para reduzir a carga de CPU.

-   Use as informações do processador (\_Total)\\% utilização do processador &lt; contador de desempenho de 60%.

## <a name="avoid-overloading-the-network-adapter"></a>Evite sobrecarregar o adaptador de rede

Assim como com processadores, utilização do adaptador de rede excessivas fará com que os tempos de espera longos para o tráfego de saída para voltar à rede. Active Directory tende a ter as solicitações de entrada pequenas e relativamente significativamente maiores quantidades de dados retornados para os sistemas cliente. Dados enviados excedem muito dados recebidos. Em muitos ambientes, a filosofia é garantir que haja espaço suficiente principal para acomodar as explosões ou picos de carga. Esse limite é um limite de aviso em que o espaço disponível para acomodar as explosões ou picos de carga se torna restrito e capacidade de resposta do cliente degrada. Em resumo, exceder esses limites não é ruim a curto prazo (algumas vezes ao dia 5 a 15 minutos), porém um sistema de execução prolongada com esses tipos de estatísticas está sobre impostos e deve ser investigado.

-   Para obter mais informações sobre como ajustar o subsistema de rede, consulte [ajuste de desempenho para subsistemas de rede](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Use a interface de rede comparar (\*)\\Bytes enviados/s com a interface de rede (\*)\\contador de desempenho de largura de banda atual. A taxa deve ser menor que 60% utilizada.

## <a name="see-also"></a>Consulte também
- [Servidores do Active Directory de ajuste de desempenho](index.md)
- [Considerações de LDAP](ldap-considerations.md)
- [Posicionamento adequado dos controladores de domínio e considerações de site](site-definition-considerations.md)
- [Solucionando problemas de desempenho do ADDS](troubleshoot.md) 
- [Planejamento de capacidade para Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566)