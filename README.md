# Övning: Kaffechatbot

I den här övningen ska du bygga en AI-driven chatbot för ett café. Chatbotten ska kunna hjälpa kunder med frågor om kaffemenyn, rekommendera drycker och hantera enklare beställningsönskemål.

Övningen är uppdelad i två steg. Under måndagen bygger du grunden med **Prompt Engineering, Model Parameters och Structured Output**. På onsdagen vidareutvecklar du chatbotten med **Tools**, så att den själv kan hämta information från ett externt API.

## Steg 1 – Bygg en intelligent kaffechatbot (måndag)

Börja med att skapa en enkel chatbot i React som kommunicerar med en lokal språkmodell via Ollama och LangChain.js.

### Del 1 – Prompt Engineering

Skapa en `ChatPromptTemplate` som styr hur chatbotten ska bete sig.

Chatbotten ska:

- Agera som en trevlig och kunnig barista.
- Kunna svara på frågor om caféets kaffemeny.
- Kunna rekommendera drycker utifrån kundens önskemål.
- Komma ihåg tidigare meddelanden under konversationen.
- Inte hitta på drycker eller priser som saknas i menyn.

Till en början ska du använda en **hårdkodad kaffemeny** som skickas med i prompten.

Exempel:

```js
const menu = [
  {
    id: 1,
    title: "Bryggkaffe",
    desc: "Ett klassiskt svart kaffe med fyllig, balanserad smak och mild beska. Passar dig som uppskattar traditionellt kaffe utan mjölk, med tydlig kaffesmak och lagom styrka.",
    price: 39
  },
  {
    id: 2,
    title: "Caffè Doppio",
    desc: "En dubbel espresso med intensiv och kraftfull kaffesmak, tydlig beska och fyllig karaktär. Perfekt för dig som vill ha riktigt starkt kaffe utan mjölk.",
    price: 49
  },
  {
    id: 3,
    title: "Cappuccino",
    desc: "En balanserad kaffedryck med espresso, varm mjölk och luftigt mjölkskum. Krämig och mjuk men med tydlig kaffesmak. Passar dig som vill ha en lagom stark kaffeupplevelse.",
    price: 49
  },
  {
    id: 4,
    title: "Latte Macchiato",
    desc: "En mild och mjölkig kaffedryck med mycket varm mjölk, mjölkskum och en mindre mängd espresso. Len och försiktig kaffesmak. Passar dig som föredrar svagare kaffe.",
    price: 49
  },
  {
    id: 5,
    title: "Kaffe Latte",
    desc: "En stor, len och krämig kaffedryck med espresso och rikligt med varm mjölk. Mildare kaffesmak och låg upplevd beska. Perfekt för dig som gillar mjölkiga kaffedrycker.",
    price: 54
  },
  {
    id: 6,
    title: "Cortado",
    desc: "En liten och smakrik kaffedryck där espresso blandas med en mindre mängd varm mjölk. Starkare kaffesmak än cappuccino och latte, men mjukare än ren espresso.",
    price: 39
  },
  {
    id: 7,
    title: "Gustav Adolfsbakelse",
    desc: "En söt och fyllig bakelse med chokladkaraktär. Passar dig som tycker om choklad, söta desserter och något gott till kaffet.",
    price: 50
  },
  {
    id: 8,
    title: "Semla",
    desc: "Ett klassiskt svenskt bakverk med mjuk vetebulle, söt mandelmassa och luftig vispad grädde. Passar dig som uppskattar gräddiga, söta bakverk och traditionell fika.",
    price: 50
  }
];

export default menu;
```

Använd `ChatPromptTemplate` och `MessagesPlaceholder` för att kombinera systeminstruktioner, meny, konversationshistorik och användarens fråga.

Testa gärna även att använda **zero-shot och few-shot prompting** för att påverka chatbotten.

### Del 2 – Modellinställningar

Experimentera med olika inställningar för din språkmodell.

Testa exempelvis:

- `temperature` – hur varierade blir svaren?
- `topP` – hur påverkas genereringen?
- Begränsning av output – hur långa svar får modellen producera?

Fundera på vilka inställningar som passar bäst för en kundtjänstliknande chatbot som ska ge korrekta och konsekventa svar.

### Del 3 – Structured Output

Bygg en separat funktion där användaren kan skriva en beställning i fritext, exempelvis:

> "Jag skulle vilja ha två cappuccino och en latte."

Använd **Zod och structured output** för att låta modellen omvandla texten till ett JavaScript-objekt.

Exempel på önskat resultat:

```js
{
    items: [
        { name: "Cappuccino", quantity: 2 },
        { name: "Latte", quantity: 1 }
    ]
}
```

Visa sedan beställningen i React, exempelvis som en enkel ordersammanställning.

**Extra utmaning:** Beräkna totalsumman med vanlig JavaScript utifrån priserna i menyn. Låt alltså inte språkmodellen själv räkna ut priset.

## Steg 2 – Ge chatbotten tillgång till Tools (onsdag)

Nu ska du vidareutveckla din chatbot så att den kan hämta aktuell information från ett externt API.

### Del 1 – Skapa en tool för kaffemenyn

Använd API:et nedan:

https://airbean-9pcyw.ondigitalocean.app/api/docs/

Skapa en LangChain-tool som hämtar menyn från API:et.

Din tool ska:

- Göra ett HTTP-anrop till API:et.
- Returnera menyinformation som modellen kan använda.
- Hantera eventuella fel vid API-anropet.

### Del 2 – Låt modellen använda din tool

Koppla din tool till språkmodellen med LangChains funktionalitet för tool calling.

Chatbotten ska nu kunna avgöra när den behöver hämta menyinformation.

Exempel:

**Användare:** "Vad kostar en cappuccino?"

**Chatbotten:** Använder sin tool för att hämta menyn och svarar med det aktuella priset.

**Användare:** "Tack så mycket!"

**Chatbotten:** Svarar utan att behöva hämta menyn igen.

Det är viktigt att förstå skillnaden mellan att applikationen alltid hämtar data och att modellen själv kan begära att en tool används.

### Del 3 – Koppla ihop funktionerna

När du är klar ska din chatbot kunna:

- Besvara vanliga frågor och komma ihåg konversationen.
- Hämta aktuell kaffemeny genom en tool.
- Rekommendera drycker baserat på den hämtade menyn.
- Tolka beställningar med structured output.
- Visa en ordersammanställning i React.

## Viktigt

En språkmodell kan föreslå en beställning, men det är din applikation som ska kontrollera att produkterna finns, beräkna priser och hantera beställningsinformationen.

Du behöver inte skapa någon riktig beställning i caféets system.

Målet med övningen är att förstå hur **prompts, modellinställningar, structured output och tools** tillsammans kan användas för att skapa en mer avancerad AI-applikation.
