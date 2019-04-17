---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: "Configurando um computador para solução de problemas"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurando um computador para solução de problemas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Antes de usar técnicas avançadas de solução de problemas para identificar e corrigir problemas do Active Directory, configure seus computadores para a solução de problemas. Você também deve ter uma compreensão básica de <token>nextref_longhorincludes > conceitos, procedimentos e ferramentas de solução de problemas. </para>
    <para>Para obter informações sobre ferramentas de monitoramento do Windows Server 2008, consulte o Step-by-Step guia para monitoramento de desempenho e confiabilidade no Windows Server 2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>Tarefas de configuração de solução de problemas</title>
    <content>
      <para>para configurar seu computador para solucionar os serviços de domínio do Active Directory (AD DS), execute as seguintes tarefas:</para>
      <para>
        <link xlink:href="#BKMK_2">instalar de ferramentas de administração de servidor remoto para o AD DS</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">configurar o Monitor de confiabilidade e desempenho</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">definir níveis de log</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>Instale as ferramentas de administração de servidor remoto para o AD DS</title>
        <content>
          <para>quando você instala o AD DS para criar um controlador de domínio, as ferramentas administrativas que você usa para gerenciar o AD DS são instaladas automaticamente. Se você quiser gerenciar controladores de domínio remotamente em um computador que não é um controlador de domínio, você pode instalar as ferramentas administrativas em um servidor membro que esteja executando <token>nextref_longhorincludes > ou em um computador que esteja executando o Windows Vista com Service Pack 1 (SP1). Em servidores membro que executam o <token>nextref_longhorincludes >, você usar o recurso ferramentas de administração de servidor remoto (RSAT) no Gerenciador de servidores para instalar as ferramentas de serviços de domínio do Active Directory. RSAT substitui ferramentas de suporte do Windows no Windows Server 2003. Você também pode instalar as ferramentas de serviços de domínio do Active Directory em um computador que esteja executando o Windows Vista com Service Pack 1 (SP1) baixando as ferramentas para esse computador.</para>
          <para>para obter informações sobre como instalar o RSAT, consulte <link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">instalando de ferramentas de administração de servidor remoto para o AD DS</link>.</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title>Configurar o Monitor de confiabilidade e desempenho</title>
        <content>
          <para>Windows Server 2008 inclui o Windows Monitor de confiabilidade e desempenho, que é um snap-in do Console de gerenciamento Microsoft (MMC) que combina a funcionalidade de ferramentas autônomas anteriores, incluindo Logs de desempenho e alertas, Supervisor de desempenho de servidor e Monitor do sistema. Esse snap-in fornece uma interface gráfica do usuário (GUI) para personalizar os conjuntos de Coletores de dados e sessões de rastreamento de eventos.</para>
          <para>Monitor de confiabilidade e desempenho também inclui o Monitor de confiabilidade, um snap-in do MMC que controla as alterações do sistema e os compara a mudanças na estabilidade do sistema, fornecendo uma exibição gráfica de seu relacionamento.</para>
        </content>
      </section>
      <section address="BKMK_4">
        <title>Definir níveis de log</title>
        <content>
          <para>se as informações que você recebe no log de serviço de diretório no Visualizador de eventos não são suficientes para solução de problemas, aumente os níveis de log usando a entrada do registro apropriadas em <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
          <para>por padrão, os níveis de registro em log para todas as entradas são definidos como <embeddedLabel>0</embeddedLabel>, que fornece a quantidade mínima de informações. É o mais alto nível de registro em log <embeddedLabel>5</embeddedLabel>. Aumentar o nível de uma entrada faz com que eventos adicionais a serem registrados no log de eventos de serviço de diretório.</para>
          <para>você pode usar o procedimento a seguir para alterar o nível de log para uma entrada de diagnóstico.</para>
          <alert class="caution">
            <para>, recomendamos que você não diretamente editar o registro a menos que não haja nenhuma outra alternativa. Modificações no registro não são validadas pelo editor do registro ou pelo Windows antes que eles são aplicados e como resultado, os valores incorretos podem ser armazenados. Isso pode resultar em erros irrecuperáveis no sistema. Quando possível, use a política de grupo ou outras ferramentas do Windows, como snap-ins do MMC, realizar tarefas, em vez de edição do registro diretamente. Se você deve editar o registro, tenha muito cuidado.</para>
          </alert>
          <para>
            <embeddedLabel>requisitos</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>a associação ao grupo <embeddedLabel>Admins. do domínio</embeddedLabel>, ou equivalente, é o requisito mínimo para concluir este procedimento. <token>review_detailincludes ></para>
            </listItem>
            <listItem>
              <para>ferramentas: Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>para alterar o nível de registro em log para uma entrada de diagnóstico</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>clique <ui>iniciar</ui>, clique em <ui>executar</ui>, tipo <userInput>regedit</userInput>e clique em <ui>Okey</ui>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>navegar até a entrada para o qual você deseja configurar registro em log <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>duas vezes na entrada e em <embeddedLabel>Base</embeddedLabel>, clique em <embeddedLabel>Decimal</embeddedLabel>. </para>
                </content>
              </step>
              <step>
                <content>
                  <para>em <embeddedLabel>valor</embeddedLabel>, digite um número inteiro de <embeddedLabel>0</embeddedLabel> por meio de <embeddedLabel>5</embeddedLabel>e clique em <ui>Okey</ui>. </para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


