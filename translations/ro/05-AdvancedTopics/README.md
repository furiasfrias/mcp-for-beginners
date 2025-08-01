<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "a5c1d9e9856024d23da4a65a847c75ac",
  "translation_date": "2025-07-18T07:21:37+00:00",
  "source_file": "05-AdvancedTopics/README.md",
  "language_code": "ro"
}
-->
# Subiecte Avansate în MCP

Acest capitol acoperă o serie de subiecte avansate în implementarea Model Context Protocol (MCP), inclusiv integrarea multi-modală, scalabilitatea, bune practici de securitate și integrarea în mediul enterprise. Aceste teme sunt esențiale pentru construirea unor aplicații MCP robuste și pregătite pentru producție, capabile să răspundă cerințelor sistemelor AI moderne.

## Prezentare generală

Această lecție explorează concepte avansate în implementarea Model Context Protocol, concentrându-se pe integrarea multi-modală, scalabilitate, bune practici de securitate și integrarea în mediul enterprise. Aceste subiecte sunt cruciale pentru dezvoltarea aplicațiilor MCP de nivel producție, capabile să gestioneze cerințe complexe în mediile enterprise.

## Obiective de învățare

La finalul acestei lecții, vei putea să:

- Implementezi capabilități multi-modale în cadrul MCP
- Proiectezi arhitecturi MCP scalabile pentru scenarii cu cerințe ridicate
- Aplici bune practici de securitate aliniate principiilor de securitate MCP
- Integrezi MCP cu sisteme și cadre AI enterprise
- Optimizezi performanța și fiabilitatea în medii de producție

## Lecții și proiecte exemplu

| Link | Titlu | Descriere |
|------|-------|-----------|
| [5.1 Integration with Azure](./mcp-integration/README.md) | Integrare cu Azure | Învață cum să integrezi MCP Server pe Azure |
| [5.2 Multi modal sample](./mcp-multi-modality/README.md) | Exemple MCP Multi modal | Exemple pentru răspunsuri audio, imagine și multi-modale |
| [5.3 MCP OAuth2 sample](../../../05-AdvancedTopics/mcp-oauth2-demo) | Demo MCP OAuth2 | Aplicație minimală Spring Boot care arată OAuth2 cu MCP, atât ca Authorization cât și Resource Server. Demonstrează emiterea securizată a token-urilor, endpoint-uri protejate, implementare pe Azure Container Apps și integrare cu API Management. |
| [5.4 Root Contexts](./mcp-root-contexts/README.md) | Contexturi root | Află mai multe despre contextul root și cum să îl implementezi |
| [5.5 Routing](./mcp-routing/README.md) | Rutare | Învață diferite tipuri de rutare |
| [5.6 Sampling](./mcp-sampling/README.md) | Sampling | Învață cum să lucrezi cu sampling |
| [5.7 Scaling](./mcp-scaling/README.md) | Scalare | Învață despre scalare |
| [5.8 Security](./mcp-security/README.md) | Securitate | Asigură securitatea MCP Serverului tău |
| [5.9 Web Search sample](./web-search-mcp/README.md) | Căutare Web MCP | Server și client Python MCP care integrează SerpAPI pentru căutare web, știri, produse și Q&A în timp real. Demonstrează orchestrarea multi-tool, integrarea API extern și gestionarea robustă a erorilor. |
| [5.10 Realtime Streaming](./mcp-realtimestreaming/README.md) | Streaming | Streaming-ul de date în timp real a devenit esențial în lumea actuală bazată pe date, unde afacerile și aplicațiile au nevoie de acces imediat la informații pentru a lua decizii rapide. |
| [5.11 Realtime Web Search](./mcp-realtimesearch/README.md) | Căutare Web | Căutarea web în timp real și modul în care MCP transformă această căutare prin oferirea unei abordări standardizate pentru gestionarea contextului între modele AI, motoare de căutare și aplicații. |
| [5.12 Entra ID Authentication for Model Context Protocol Servers](./mcp-security-entra/README.md) | Autentificare Entra ID | Microsoft Entra ID oferă o soluție robustă de gestionare a identității și accesului în cloud, asigurând că doar utilizatorii și aplicațiile autorizate pot interacționa cu serverul tău MCP. |
| [5.13 Azure AI Foundry Agent Integration](./mcp-foundry-agent-integration/README.md) | Integrare Azure AI Foundry | Învață cum să integrezi serverele Model Context Protocol cu agenții Azure AI Foundry, permițând orchestrarea puternică a uneltelor și capabilități AI enterprise cu conexiuni standardizate către surse externe de date. |
| [5.14 Context Engineering](./mcp-contextengineering/README.md) | Ingineria Contextului | Oportunitățile viitoare ale tehnicilor de inginerie a contextului pentru serverele MCP, inclusiv optimizarea contextului, gestionarea dinamică a contextului și strategii pentru ingineria prompturilor eficiente în cadrul MCP. |

## Referințe suplimentare

Pentru cele mai recente informații despre subiectele avansate MCP, consultă:
- [Documentația MCP](https://modelcontextprotocol.io/)
- [Specificația MCP](https://spec.modelcontextprotocol.io/)
- [Repository GitHub](https://github.com/modelcontextprotocol)

## Concluzii cheie

- Implementările MCP multi-modale extind capabilitățile AI dincolo de procesarea textului
- Scalabilitatea este esențială pentru implementările enterprise și poate fi abordată prin scalare orizontală și verticală
- Măsurile cuprinzătoare de securitate protejează datele și asigură controlul adecvat al accesului
- Integrarea enterprise cu platforme precum Azure OpenAI și Microsoft AI Foundry îmbunătățește capabilitățile MCP
- Implementările avansate MCP beneficiază de arhitecturi optimizate și gestionarea atentă a resurselor

## Exercițiu

Proiectează o implementare MCP de nivel enterprise pentru un caz de utilizare specific:

1. Identifică cerințele multi-modale pentru cazul tău de utilizare
2. Conturează controalele de securitate necesare pentru protejarea datelor sensibile
3. Proiectează o arhitectură scalabilă care să poată gestiona încărcări variabile
4. Planifică punctele de integrare cu sistemele AI enterprise
5. Documentează potențialele blocaje de performanță și strategiile de atenuare

## Resurse suplimentare

- [Documentația Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
- [Documentația Microsoft AI Foundry](https://learn.microsoft.com/en-us/ai-services/)

---

## Ce urmează

- [5.1 MCP Integration](./mcp-integration/README.md)

**Declinare a responsabilității**:  
Acest document a fost tradus folosind serviciul de traducere AI [Co-op Translator](https://github.com/Azure/co-op-translator). Deși ne străduim pentru acuratețe, vă rugăm să rețineți că traducerile automate pot conține erori sau inexactități. Documentul original în limba sa nativă trebuie considerat sursa autorizată. Pentru informații critice, se recomandă traducerea profesională realizată de un specialist uman. Nu ne asumăm răspunderea pentru eventualele neînțelegeri sau interpretări greșite rezultate din utilizarea acestei traduceri.