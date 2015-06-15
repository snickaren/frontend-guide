# Frontend-guide
Riktlinjer och exempel för användning vid gränssnittsutveckling hos KB.

Repot innehåller setup för anpassning av [Twitter Bootstrap](https://github.com/twbs/bootstrap) samt generering av en stilguide enligt [KSS](https://github.com/kneath/kss) genom [KSS-node](https://github.com/kss-node/kss-node).

## Installation

- Ladda ner och installera [node.js](https://nodejs.org/download/).

- Installera grunt-cli

        npm install -g grunt-cli

- Navigera till mappen _styles_ och installera node-paketen

        npm install


## Anpassning av stilguiden

I mappen _custom_ skapar du egna regler utöver bootstraps egna. Öppna variables.less och testa att ändra på något.
För att lägga till en fil i genereringen öppnar du filen _custom.less_ och lägger till

    @import 'filename.less';

### Kod-riktlinjer

#### KSS

När du lägger till en ny komponent eller en ny modifierare för en komponent så ska denna dokumenteras enligt KSS-syntaxen som går att läsa om [här](http://warpspire.com/kss/syntax/). Detta för att stilguiden ska kunna generera en vy över den nya komponenten/regeln.

Kortfattat går det ut på att ange namn, beskrivning, modifierare samt önskad position i dokumentationen.

##### Exempel

Här skapar jag 2 nya modifierare för klassen .card

    .card {
      &.card-danger {
        background-color: lighten(@brand-danger,15%);
        color: darken(@brand-danger,35%);
      }
      &.card-success {
        background-color: lighten(@brand-success,25%);
        color: darken(@brand-success,25%);
      }
    }
    
Sedan lägger jag följande kommentar ovanför koden

    /*
    Skinning of cards

    Skins for different purposes
    
    .card-danger    - Card skin to use when we want to convey some kind of danger
    .card-success   - Card skin to use when we want to convey some kind of success
    
    Markup:
    <div class="card {{modifier_class}}">
        <h1>Card</h1>
        <p>Description</p>
    </div>
    
    Styleguide 6.1.4
    */

Detta kommer att generera en vy likt denna:

![img](https://dl.dropboxusercontent.com/u/2316209/Screenshot%202015-06-03%2016.24.39.png)

#### LESS-variabler

LESS-variabler av mer global karaktär läggs med fördel till i _variables.less_ (såvida den inte redan finns representerad). Är variabeln specifik för en komponent bör den läggas högst upp i samma fil som komponentens övriga regler.

##### Exempel

__Detta exempelavsnitt är inte aktuellt och bör uppdateras.__

Jag vill redigera navbar-klassens border-width. Jag kollar i _variables.less_ om där redan finns en variabel att redigera. Eftersom det inte finns det så skapar jag en fil i custom-mappen som heter _navbar.less_, inkluderar den i _custom.less_

    @import 'navbar.less';
    
I _navbar.less_ anger jag

    .navbar {
        border-width: @navbar-border-width;
    }
I _variables.less_ letar jag upp om det finns andra ställen där navbar-klassen regleras och lägger till

    @navbar-border-width: 1px;
>Om klassen inte har några andra variabler över huvud taget bör du skapa en ny sektion längst ner i filen där du kommenterar vilken klass det handlar om och lägger in variabeln.

Du har nu gjort din anpassning och samtidigt sett till att variabeln finns tillgänglig för framtida anpassning.

## Bygga stilmall och stilguide

För att bygga din LESS till CSS samt uppdatera dokumentationen behöver du gå till styles-mappen och köra:

    $ grunt

Vill du bygga automatiskt vid varje ändring kan du köra:

    $ grunt watch

För att granska stilguiden lokalt kan du starta watch och servering samtidigt med:

    $ grunt serve

Stilguiden går då att se på: http://localhost:8500/

## Användning av stilmallen i projekt

Den byggda css-filen _kb-style.css_ hamnar i mappen _build/css_. Detta är filen du inkluderar i din tjänst.
