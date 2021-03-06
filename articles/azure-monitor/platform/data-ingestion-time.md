---
title: Tid för att mata in logg data i Azure Monitor | Microsoft Docs
description: Förklarar de olika faktorer som påverkar svars tiderna vid insamling av loggdata i Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/18/2019
ms.openlocfilehash: 87bfe1109640f158b92f54b945d314ac65a93ddc
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92107920"
---
# <a name="log-data-ingestion-time-in-azure-monitor"></a>Inmatningstid för loggdata i Azure Monitor
Azure Monitor är en hög skalbar data tjänst som tjänar tusentals kunder som skickar terabyte data varje månad i en växande takt. Det finns ofta frågor om hur lång tid det tar för loggdata att bli tillgängliga när de har samlats in. I den här artikeln beskrivs de olika faktorer som påverkar den här svars tiden.

## <a name="typical-latency"></a>Typisk svars tid
Svars tiden avser den tid då data skapas i det övervakade systemet och den tid som det kommer att vara tillgängligt för analys i Azure Monitor. Den vanliga svars tiden för inmatning av loggdata är mellan 2 och 5 minuter. Den specifika svars tiden för alla specifika data varierar beroende på en mängd olika faktorer som beskrivs nedan.


## <a name="factors-affecting-latency"></a>Faktorer som påverkar svars tiden
Den totala inmatnings tiden för en viss uppsättning data kan delas upp i följande hög nivå områden. 

- Agent tid – tid att upptäcka en händelse, samla in den och sedan skicka den till Azure Monitor inmatnings platsen som en loggpost. I de flesta fall hanteras den här processen av en agent.
- Pipeline-tid – Den tid det tar inmatnings-pipelinen att bearbeta loggposten. Detta innefattar att parsa egenskaperna för händelsen och eventuellt lägga till Beräknad information.
- Indexerings tid – tids åtgången för att mata in en loggpost i Azure Monitor Big data Store.

Information om den olika svars tiden som introduceras i den här processen beskrivs nedan.

### <a name="agent-collection-latency"></a>Svarstid för agentinsamling
Agenter och hanterings lösningar använder olika strategier för att samla in data från en virtuell dator, vilket kan påverka svars tiden. Några exempel är följande:

- Windows-händelser, Syslog-händelser och prestanda statistik samlas in direkt. Linux-prestandaräknare avsökers med 30 sekunders intervall.
- IIS-loggar och anpassade loggar samlas in när deras tidsstämpel ändras. För IIS-loggar påverkas detta av ett [förnyat schema som kon figurer ATS i IIS](data-sources-iis-logs.md). 
- Active Directory Replication utför utvärderingen var femte dag, medan Active Directory-utvärdering-lösningen utför en veckovis utvärdering av din Active Directory-infrastruktur. Agenten samlar endast in dessa loggar när utvärderingen är klar.

### <a name="agent-upload-frequency"></a>Frekvens för agentuppladdning
För att säkerställa att Log Analytics-agenten är låg, buffrar agenten loggar och överför dem regelbundet till Azure Monitor. Uppladdnings frekvensen varierar mellan 30 sekunder och 2 minuter beroende på vilken typ av data du har. De flesta data laddas upp under 1 minut. Nätverks förhållandena kan påverka svars tiden för dessa data negativt för att uppnå Azure Monitor inmatnings punkt.

### <a name="azure-activity-logs-resource-logs-and-metrics"></a>Azure aktivitets loggar, resurs loggar och mått
Azure Data lägger till ytterligare tid för att bli tillgänglig vid Log Analytics inmatnings punkt för bearbetning:

- Data från resurs loggar tar 2-15 minuter, beroende på Azure-tjänsten. Se [frågan nedan](#checking-ingestion-time) för att undersöka den här svars tiden i din miljö
- Azures plattforms mått tar 3 minuter att skickas till Log Analytics inmatnings platsen.
- Aktivitets logg data tar cirka 10-15 minuter att skickas till Log Analytics inmatnings plats.

När det är tillgängligt vid inmatnings punkten tar data ytterligare 2-5 minuter att bli tillgängliga för frågor.

### <a name="management-solutions-collection"></a>Samling med hanterings lösningar
Vissa lösningar samlar inte in data från en agent och kan använda en samlings metod som inför ytterligare svars tid. Vissa lösningar samlar in data med jämna mellanrum utan att försöka använda real tids insamling. Vissa exempel är följande:

- Microsoft 365 lösning avsöker aktivitets loggar med API: t för hanterings aktiviteten, som för närvarande inte ger några garantier om nästan real tids fördröjning.
- Windows Analytics-lösningar (Uppdateringsefterlevnad till exempel) samlas in av lösningen med en daglig frekvens.

Se dokumentationen för varje lösning för att fastställa dess samlings frekvens.

### <a name="pipeline-process-time"></a>Pipeline-process tid
När logg poster matas in i Azure Monitor pipelinen (som identifieras i egenskapen [_TimeReceived](./log-standard-columns.md#_timereceived) ), skrivs de till temporär lagring för att säkerställa klient isoleringen och se till att data inte förloras. Den här processen lägger normalt till 5-15 sekunder. Vissa hanterings lösningar implementerar tyngre algoritmer för att samla in data och härleda insikter när data strömmas i. Exempel: övervakning av nätverks prestanda sammanställer inkommande data över 3 minuters intervall, vilket effektivt lägger till en fördröjning på 3 minuter. En annan process som lägger till latens är den process som hanterar anpassade loggar. I vissa fall kan den här processen lägga till några minuters svars tid på loggar som samlas in från filer av agenten.

### <a name="new-custom-data-types-provisioning"></a>Ny anpassad data typs etablering
När en ny typ av anpassade data skapas från en [anpassad logg](data-sources-custom-logs.md) eller [data insamlings-API: et](data-collector-api.md)skapar systemet en dedikerad lagrings behållare. Detta är ett engångsjobb som bara inträffar vid den första förekomsten av den här datatypen.

### <a name="surge-protection"></a>Överspännings skydd
Den högsta prioriteten hos Azure Monitor är att se till att ingen kund information förloras, så systemet har ett inbyggt skydd för data toppar. Detta inkluderar buffertar för att säkerställa att även under stor-belastningen fortsätter att fungera. Under normal belastning lägger de här kontrollerna till mindre än en minut, men i extrema förhållanden och fel kan de lägga till avsevärd tid samtidigt som de säkerställer att data är säkra.

### <a name="indexing-time"></a>Indexerings tid
Det finns en inbyggd balans för varje stor data plattform mellan att tillhandahålla funktioner för analys och avancerad sökning i stället för att ge direkt åtkomst till data. Med Azure Monitor kan du köra kraftfulla frågor på miljard tals poster och få resultat inom några sekunder. Detta är möjligt eftersom infrastrukturen omvandlar data dramatiskt under inmatningen och lagrar dem i unika kompakta strukturer. Systemet buffrar data tills det finns tillräckligt med ledigt utrymme för att skapa dessa strukturer. Detta måste slutföras innan logg posten visas i Sök resultaten.

Den här processen tar för närvarande cirka 5 minuter när det finns låg data volym men mindre tid vid högre data hastigheter. Detta verkar counterintuitive, men den här processen gör det möjligt att optimera svars tiden för produktions arbets belastningar med stora volymer.



## <a name="checking-ingestion-time"></a>Kontrollerar Inhämtnings tid
Hämtnings tiden kan variera beroende på olika resurser under olika omständigheter. Du kan använda logg frågor för att identifiera en miljös speciella beteende. I följande tabell anges hur du kan bestämma olika tider för en post när den skapas och skickas till Azure Monitor.

| Steg | Egenskap eller funktion | Kommentarer |
|:---|:---|:---|
| Post skapad vid data Källa | [TimeGenerated](./log-standard-columns.md#timegenerated-and-timestamp) <br>Om data källan inte anger det här värdet, kommer den att ställas in på samma tid som _TimeReceived. |
| Posten togs emot av Azure Monitor-inmatnings slut punkt | [_TimeReceived](./log-standard-columns.md#_timereceived) | |
| Post lagrad i arbets ytan och tillgänglig för frågor | [ingestion_time ()](/azure/kusto/query/ingestiontimefunction) | |

### <a name="ingestion-latency-delays"></a>Fördröjningar vid inmatnings fördröjning
Du kan mäta svars tiden för en speciell post genom att jämföra resultatet av funktionen [ingestion_time ()](/azure/kusto/query/ingestiontimefunction) i egenskapen _TimeGenerated_ . Dessa data kan användas med olika agg regeringar för att ta reda på hur inmatnings fördröjningen fungerar. Granska några percentiler av Inhämtnings tiden för att få insikter om stora mängder data. 

Följande fråga visar t. ex. vilka datorer som hade den högsta inmatnings tiden under de senaste 8 timmarna: 

``` Kusto
Heartbeat
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95), percentiles(AgentLatency,50,95) by Computer 
| top 20 by percentile_E2EIngestionLatency_95 desc
```

Föregående percentils kontroller är lämpliga för att hitta allmänna trender i svars tid. För att kunna identifiera kortvarig insamling i svars tid kan det vara mer effektivt att använda maximalt ( `max()` ).

Om du vill öka detalj nivån för inmatnings tiden för en viss dator under en viss tids period, använder du följande fråga, som också visualiserar data från föregående dag i ett diagram: 


``` Kusto
Heartbeat 
| where TimeGenerated > ago(24h) //and Computer == "ContosoWeb2-Linux"  
| extend E2EIngestionLatencyMin = todouble(datetime_diff("Second",ingestion_time(),TimeGenerated))/60 
| extend AgentLatencyMin = todouble(datetime_diff("Second",_TimeReceived,TimeGenerated))/60 
| summarize percentiles(E2EIngestionLatencyMin,50,95), percentiles(AgentLatencyMin,50,95) by bin(TimeGenerated,30m) 
| render timechart
```
 
Använd följande fråga för att Visa dator hämtnings tiden av landet/regionen som de finns i, baserat på deras IP-adress: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95),percentiles(AgentLatency,50,95) by RemoteIPCountry 
```
 
Olika data typer som härstammar från agenten kan ha olika svars tid, så de tidigare frågorna kan användas med andra typer. Använd följande fråga för att undersöka Inhämtnings tiden för olika Azure-tjänster: 

``` Kusto
AzureDiagnostics 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95), percentiles(AgentLatency,50,95) by ResourceProvider
```

### <a name="resources-that-stop-responding"></a>Resurser som slutar svara 
I vissa fall kan en resurs sluta skicka data. Om du vill förstå om en resurs skickar data eller inte, tittar du på den senaste posten som kan identifieras av fältet standard _TimeGenerated_ .  

Använd tabellen _pulsslag_ för att kontrol lera tillgängligheten för en virtuell dator, eftersom ett pulsslag skickas en gång i minuten av agenten. Använd följande fråga för att visa en lista över de aktiva datorer som inte rapporterade pulsslag nyligen: 

``` Kusto
Heartbeat  
| where TimeGenerated > ago(1d) //show only VMs that were active in the last day 
| summarize NoHeartbeatPeriod = now() - max(TimeGenerated) by Computer  
| top 20 by NoHeartbeatPeriod desc 
```

## <a name="next-steps"></a>Nästa steg
* Läs [serviceavtal (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) för Azure Monitor.