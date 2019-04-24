---
title: Ajuste de desempenho de servidores Web
description: Recomendações para ajuste de desempenho de servidores Web com Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 72a7a8c2bc7e90a24e8c47c9296aa6670010cd0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891817"
---
# <a name="performance-tuning-web-servers"></a>Ajuste de desempenho de servidores Web


Este tópico descreve as recomendações e os métodos para ajustar o desempenho de servidores Web com Windows Server 2016.


## <a name="selecting-the-proper-hardware-for-performance"></a>Selecionar o hardware adequado para desempenho


É importante selecionar o hardware adequado para atender à carga esperada da Web, considerando a carga média, a carga de pico, a capacidade, os planos de crescimento e os tempos de resposta. Os gargalos de hardware limitam a eficiência do ajuste do software.

O tópico [Ajuste de desempenho de hardware de servidor](../../hardware/index.md) fornece recomendações de hardware para evitar as restrições de desempenho a seguir:

-   CPUs lentas oferecem potência de processamento limitada para cargas de trabalho intensivas da CPU, como cenários ASP, ASP.NET e TLS.

-   Um cache pequeno para o processador L2 ou L3/LLC pode afetar negativamente o desempenho.

-   Uma quantidade limitada de memória afeta o número de sites que podem ser hospedados, quantos scripts dinâmicos de conteúdo (como o ASP.NET, por exemplo) podem ser armazenados e o número de pools de aplicativos ou processos de trabalho.

-   A rede se torna um gargalo devido a um adaptador de rede ineficiente.

-   O sistema de arquivos se torna um gargalo devido à ineficiência do adaptador de armazenamento ou do subsistema de disco.

## <a name="operating-system-best-practices"></a>Práticas recomendadas para sistemas operacionais


Se possível, comece com uma instalação limpa do sistema operacional. Atualizar o software pode deixar configurações do registro desatualizadas, indesejadas ou abaixo do ideal, além de serviços e aplicativos instalados anteriormente que consomem recursos se forem iniciados automaticamente. Se outro sistema operacional estiver instalado e você precisar mantê-lo, instale o novo sistema operacional em uma partição diferente. Caso contrário, a nova instalação substituirá as configurações em %Arquivos de Programas%\\Arquivos Comuns.

Para reduzir a interferência no acesso ao disco, coloque o arquivo de paginação do sistema, o sistema operacional, dados da Web, o cache de modelos ASP e o log de IIS (Serviços de Informações da Internet) em discos físicos separados, se possível.

Para reduzir a contenção para recursos do sistema, instale o Microsoft SQL Server e o IIS em servidores diferentes, se possível.

Evite a instalação de aplicativos e serviços não essenciais. Em alguns casos, talvez valha a pena desabilitar os serviços que não sejam necessários em um sistema.

## <a name="ntfs-file-system-settings"></a>Configurações do sistema de arquivos NTFS

A opção global do sistema **NtfsDisableLastAccessUpdate** (REG\_DWORD) 1 fica em **HKLM\\System\\CurrentControlSet\\Control\\FileSystem** e é definida por padrão como 1. Essa opção reduz a carga de E/S e as latências do disco ao desabilitar a atualização de carimbo de data/hora para o último acesso ao arquivo ou diretório. Instalações limpas do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e do Windows Server 2008 habilitam essa configuração por padrão e você não precisará ajustá-la. Versões anteriores do Windows não configuram essa chave. Se o servidor estiver executando uma versão anterior do Windows ou se for atualizado para o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, você deve habilitar essa configuração.

Desabilitar as atualizações é útil ao utilizar grandes conjuntos de dados (ou vários hosts) que contêm milhares de diretórios. É recomendável que você use o log do IIS, caso mantenha essas informações apenas para a administração da Web.

>[!Warning]
> Alguns aplicativos, como utilitários de backup incrementais, dependem dessas informações de atualização e não funcionam corretamente sem elas.

## <a name="see-also"></a>Consulte também
- [Ajuste de desempenho do IIS 10.0](tuning-iis-10.md)
- [Ajuste do HTTP 1.1/2](http-performance.md)


