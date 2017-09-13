---
title: "Kurz k MongoDB, Angular a Node pro Azure – Část 3 | Dokumentace Microsoftu"
description: "Třetí část série kurzů týkající se vytvoření aplikace MongoDB s Angular a Node postavené na službě Azure Cosmos DB s použitím stejných rozhraní API, jako používáte pro MongoDB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: 1fb8d7b9f1014f37f0f3afa20605fce10c45a967
ms.contentlocale: cs-cz
ms.lasthandoff: 09/06/2017

---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-3-build-the-ui-with-angular"></a>Vytvoření aplikace MongoDB s Angular a službou Azure Cosmos DB – Část 3: Vytvoření uživatelského rozhraní pomocí Angular

Tento vícedílný kurz ukazuje, jak vytvořit novou aplikaci [MongoDB API](mongodb-introduction.md) napsanou v Node.js s Express a Angular a jak ji připojit k databázi Azure Cosmos DB.

Třetí část kurzu vychází z [části 2](tutorial-develop-mongodb-nodejs-part2.md) a zabývá se následujícími úlohami:

> [!div class="checklist"]
> * Vytvoření uživatelského rozhraní v Angular
> * Nastavení vzhledu a chování pomocí šablon stylů CSS
> * Místní testování aplikace

## <a name="video-walkthrough"></a>Video s návodem

> [!VIDEO https://www.youtube.com/embed/MnxHuqcJVoM]

## <a name="prerequisites"></a>Požadavky

Před zahájením této části kurzu se ujistěte, že jste dokončili kroky v [části 2](tutorial-develop-mongodb-nodejs-part2.md) tohoto kurzu.

> [!TIP]
> Tento kurz vás postupně provede jednotlivými kroky vytvoření aplikace. Pokud si chcete stáhnout dokončený projekt, v [úložišti angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) na GitHubu můžete získat hotovou aplikaci.

## <a name="build-the-ui"></a>Vytvoření uživatelského rozhraní

1. Ve Visual Studio Code kliknutím na tlačítko Zastavit ![Tlačítko Zastavit ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/stop-button.png) zastavte aplikaci Node.

2. V okně příkazového řádku ve Windows nebo v okně Terminálu na Macu zadejte následující příkaz, který vygeneruje komponentu heroes. V tomto kódu je g = generovat, c = komponenta, heroes = název komponenty. Kód používá strukturu plochých souborů (--flat), takže se pro komponentu nevytvoří podsložka.

    ```
    ng g c heroes --flat 
    ```

    V okně terminálu se zobrazí potvrzení nových komponent.

    ```bash
    installing component
      create src\client\app\heroes.component.ts
      update src\client\app\app.module.ts 
    ```

    Podívejme se na vytvořené a nahrané soubory. 

3. Ve Visual Studio Code v podokně **Průzkumník** přejděte do složky **src\client\app** a otevřete nový soubor **heroes.component.ts** vytvořený v kroku 2. Tento soubor komponenty TypeScript byl vytvořený předchozím příkazem.

    > [!TIP]
    > Pokud se složka aplikace ve Visual Studio Code nezobrazuje, stisknutím kombinace kláves CMD + SHIFT + P na Macu nebo Ctrl + Shift + P ve Windows otevřete Paletu příkazů a zadejte *Reload Window* (Znovu načíst okno) pro převzetí změny systému.

    ![Otevření souboru heroes.component.ts](./media/tutorial-develop-mongodb-nodejs-part3/open-folder.png)

4. Ve stejné složce otevřete soubor **app.module.ts** a všimněte si, že na řádku 5 se importovala komponenta `HeroesComponent`, která se také přidala k deklaracím na řádku 10.

    ![Otevření souboru app.module.ts](./media/tutorial-develop-mongodb-nodejs-part3/app-module-file.png)

    Když teď máte komponentu Heroes, vytvořte nový soubor pro její kód HTML. Vzhledem k tomu, že jsme vytvořili minimální aplikaci, by kód HTML měl být ve stejném souboru jako TypeScript, ale my je chceme rozdělit a vytvořit samostatný soubor.

5. V podokně **Průzkumník** klikněte pravým tlačítkem na složku **app**, klikněte na **Nový soubor** a pojmenujte nový soubor *heroes.component.html*.

6. V souboru **heroes.component.ts** odstraňte řádky 5 až 9 

    ```ts
    template: `
        <p>
          heroes Works!
        </p>
      `,
      ```
      a nahraďte je kódem
  
    ```ts
    templateUrl: './heroes.component.html',
    ```

    pro přidání reference na nový soubor HTML.
 
    > [!TIP]
    > K urychlení vývoje můžete použít rozšíření a fragmenty kódu Angular Essentials pro Visual Studio Code od Johna Papy. 
    > 1. Klikněte na tlačítko **Rozšíření** ![Tlačítko Rozšíření ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/extensions-button.png).
    > 2. Do vyhledávacího pole zadejte *angular essentials*.
    > 3. Klikněte na **Nainstalovat**. 
    > 4. Klikněte na tlačítko **Znovu načíst**, aby se použila nová rozšíření.
    > Případně je můžete stáhnout ze stránky [http://jpapa.me/angularessentials](http://jpapa.me/angularessentials). 
    > ![Rozšíření Angular Essentials](./media/tutorial-develop-mongodb-nodejs-part3/angular-essentials-extension.png)

7. Přejděte zpět do souboru **heroes.component.html** a zkopírujte do něj tento kód. Element `<div>` je kontejner pro celou stránku. Uvnitř kontejneru je seznam hrdinů, který potřebujeme vytvořit tak, aby se hrdina po kliknutí vybral a bylo možné ho upravit nebo odstranit v uživatelském rozhraní. Dále v kódu HTML máme několik stylů, abyste věděli, který hrdina je vybraný. Je tam také oblast pro úpravy, abyste mohli přidat nového hrdinu nebo upravit existujícího. 

    ```html
    <div>
      <ul class="heroes">
        <li *ngFor="let hero of heroes" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
          <button class="delete-button" (click)="deleteHero(hero)">Delete</button>
          <div class="hero-element">
            <div class="badge">{{hero.id}}</div>
            <div class="name">{{hero.name}}</div>
            <div class="saying">{{hero.saying}}</div>
          </div>
        </li>
      </ul>
      <div class="editarea">
        <button (click)="enableAddMode()">Add New Hero</button>
        <div *ngIf="selectedHero">
          <div class="editfields">
            <div>
              <label>id: </label>
              <input [(ngModel)]="selectedHero.id" placeholder="id" *ngIf="addingHero" />
              <label *ngIf="!addingHero" class="value">{{selectedHero.id}}</label>
            </div>
            <div>
              <label>name: </label>
              <input [(ngModel)]="selectedHero.name" placeholder="name" />
            </div>
            <div>
              <label>saying: </label>
              <input [(ngModel)]="selectedHero.saying" placeholder="saying" />
            </div>
          </div>
          <button (click)="cancel()">Cancel</button>
          <button (click)="save()">Save</button>
        </div>
      </div>
    </div>
    ```

8. Když teď máme hotový kód HTML, potřebujeme ho přidat do souboru **heroes.component.ts**, abychom mohli s šablonou interagovat. Níže uvedený nový kód přidaný do souboru **heroes.component.ts** přidá šablonu do našeho souboru komponenty. Přidal se konstruktor, který získá několik hrdinů a inicializuje komponentu služby hero, která získá všechna data. Tento kód také přidává potřebné metody pro zpracování událostí v uživatelském rozhraní. Následující kód můžete zkopírovat a nahradit jím stávající kód v souboru **heroes.component.ts**. 

    ```ts
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-heroes',
      templateUrl: './heroes.component.html'
    })
    export class HeroesComponent implements OnInit {
      addingHero = false;
      heroes: any = [];
      selectedHero: Hero;
    
      constructor(private heroService: HeroService) {}
    
      ngOnInit() {
       this.getHeroes();
      }

      cancel() {
        this.addingHero = false;
        this.selectedHero = null;
      }

      deleteHero(hero: Hero) {
        this.heroService.deleteHero(hero).subscribe(res => {
          this.heroes = this.heroes.filter(h => h !== hero);
          if (this.selectedHero === hero) {
            this.selectedHero = null;
          }
        });
      }

      getHeroes() {
        return this.heroService.getHeroes().subscribe(heroes => {
          this.heroes = heroes;
        });
      }

      enableAddMode() {
        this.addingHero = true;
        this.selectedHero = new Hero();
      }

      onSelect(hero: Hero) {
        this.addingHero = false;
        this.selectedHero = hero;
      }

      save() {
        if (this.addingHero) {
          this.heroService.addHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
            this.heroes.push(hero);
          });
        } else {
          this.heroService.updateHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
          });
        }
      }
    }
    ```

9. V **Průzkumníku** otevřete soubor **app/app.module.ts** a aktualizujte řádky 13 (na konci přidejte čárku) a 14 pro přidání importu modulu `FormsModule`. Část importování by teď měla vypadat následovně:

    ```
    imports: [
      BrowserModule,
      FormsModule
    ],
    ```

10. Na řádku 3 přidejte import nového modulu FormsModule. 

    ```
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    ```

## <a name="use-css-to-set-the-look-and-feel"></a>Nastavení vzhledu a chování pomocí šablon stylů CSS

1. V podokně Průzkumník otevřete soubor **src/client/styles.scss**.

2. Do souboru **styles.scss** zkopírujte následující kód a nahraďte jím stávající obsah souboru.

    ```css
    /* You can add global styles to this file, and also import other style files */

    * {
      font-family: Arial;
    }
    h2 {
      color: #444;
      font-weight: lighter;
    }
    body {
      margin: 2em;
    }
    
    body,
    input[text],
    button {
      color: #888;
      // font-family: Cambria, Georgia;
    }
    button {
      font-size: 14px;
      font-family: Arial;
      background-color: #eee;
      border: none;
      padding: 5px 10px;
      border-radius: 4px;
      cursor: pointer;
      cursor: hand;
      &:hover {
        background-color: #cfd8dc;
      }
      &.delete-button {
        float: right;
        background-color: gray !important;
        background-color: rgb(216, 59, 1) !important;
        color: white;
        padding: 4px;
        position: relative;
        font-size: 12px;
      }
    }
    div {
      margin: .1em;
    }

    .selected {
      background-color: #cfd8dc !important;
      background-color: rgb(0, 120, 215) !important;
      color: white;
    }

    .heroes {
      float: left;
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      li {
        cursor: pointer;
        position: relative;
        left: 0;
        background-color: #eee;
        margin: .5em;
        padding: .5em;
        height: 3.0em;
        border-radius: 4px;
        width: 17em;
        &:hover {
          color: #607d8b;
          color: rgb(0, 120, 215);
          background-color: #ddd;
          left: .1em;
        }
        &.selected:hover {
          /*background-color: #BBD8DC !important;*/
          color: white;
        }
      }
      .text {
        position: relative;
        top: -3px;
      }
      .saying {
        margin: 5px 0;
      }
      .name {
        font-weight: bold;
      }
      .badge {
        /* display: inline-block; */
        float: left;
        font-size: small;
        color: white;
        padding: 0.7em 0.7em 0 0.5em;
        background-color: #607d8b;
        background-color: rgb(0, 120, 215);
        background-color:rgb(134, 183, 221);
        line-height: 1em;
        position: relative;
        left: -1px;
        top: -4px;
        height: 3.0em;
        margin-right: .8em;
        border-radius: 4px 0 0 4px;
        width: 1.2em;
      }
    }

    .header-bar {
      background-color: rgb(0, 120, 215);
      height: 4px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    label {
      display: inline-block;
      width: 4em;
      margin: .5em 0;
      color: #888;
      &.value {
        margin-left: 10px;
        font-size: 14px;
      }
    }

    input {
      height: 2em;
      font-size: 1em;
      padding-left: .4em;
      &::placeholder {
          color: lightgray;
          font-weight: normal;
          font-size: 12px;
          letter-spacing: 3px;
      }
    }

    .editarea {
      float: left;
      input {
        margin: 4px;
        height: 20px;
        color: rgb(0, 120, 215);
      }
      button {
        margin: 8px;
      }
      .editfields {
        margin-left: 12px;
      }
    }
    ``` 
3. Uložte soubor. 

## <a name="display-the-component"></a>Zobrazení komponenty

Když teď máme komponentu, jak ji zobrazíme na obrazovce? Pojďme upravit výchozí komponenty v souboru **app.component.ts**.

1. V podokně Průzkumník otevřete soubor **client/app/app.component.ts**.

2. Na řádcích 6 až 8 změňte nadpis na Heroes a pak přidejte název komponenty, kterou jsme vytvořili v souboru **heroes.components.ts** (app-heroes) pro přidání reference na tuto novou komponentu. Část template by teď měla vypadat následovně: 

    ```ts
    template: `
      <h1>Heroes</h1>
      <div class="header-bar"></div>
      <app-heroes></app-heroes>
    `,
    ```

3. V souboru **heroes.components.ts** jsou další komponenty, na které odkazujeme, například komponenta Hero, takže potřebujeme vytvořit i ty. Na příkazovém řádku Angular CLI vytvořte model hero a soubor **hero.ts** pomocí následujícího příkazu, kde g = generovat, cl = třída a hero = název třídy.

    ```bash
    ng g cl hero
    ```

    V okně terminálu se zobrazí potvrzení nové třídy.

    ```bash
    installing class
    create src\client\app\hero.ts
    ```

4. V podokně Průzkumník otevřete soubor **src\client\app\hero.ts**.

5. V souboru **hero.ts** nahraďte obsah následujícím kódem, který přidá třídu Hero s ID, jménem (name) a slavným výrokem (saying). 

    ```ts
      export class Hero {
      id: number;
      name: string;
      saying: string;
    }
    ```

6. Přejděte zpět do souboru **heroes.components.ts** a všimněte si, že na řádku `selectedHero: Hero;` (řádek 10) má `Hero` červené podtržení. 

7. Klikněte na výraz `Hero` a Visual Studio Code na levé straně bloku kódu zobrazí ikonu žárovky. 

    ![Žárovka ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/light-bulb.png)

8. Klikněte na žárovku a pak klikněte na **Importovat Hero z umístění client/app/hero**. nebo **Importovat Hero z umístění ./hero**. (Zpráva se mění v závislosti na vašem nastavení.)

    Na řádku 2 se objeví nový řádek kódu. Pokud řádek 2 odkazuje na client/app/hero, upravte ho tak, aby odkazoval na soubor hero v místní složce (./hero). Řádek 2 by měl vypadat takto:

   ```
   import { Hero } from "./hero";
   ``` 

    Tím je postaráno o model, pořád ale potřebujeme vytvořit službu.

## <a name="create-the-service"></a>Vytvoření služby

1. Na příkazovém řádku Angular CLI vytvořte v souboru **app.module.ts** službu hero zadáním následujícího příkazu, kde g = generovat, s = služba, hero = název služby a příznak -m značí vložení do souboru app.module.

    ```bash
    ng g s hero -m app.module
    ```

    Výstup uvádí, že se vytvořil soubor **hero.service.ts** a soubor **app.module.ts** se aktualizoval.
  
    ```bash
    installing service
      create src\client\app\hero.service.ts
      update src\client\app\app.module.ts
    ```
    
    V souboru app.module.ts se přidaly následující řádky kódu (řádky 6 a 17):
    
    ```typescript
    import { HeroService } from './hero.service';
    ...
        providers: [HeroService],
    ```

2. Ve Visual Studio Code otevřete soubor **hero.service.ts** a zkopírujte do něj následující kód, kterým nahradíte obsah souboru.

    ```ts
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    
    import { Hero } from './hero';
    
    const api = '/api';

    @Injectable()
    export class HeroService {
      constructor(private http: HttpClient) {}

      getHeroes() {
        return this.http.get<Array<Hero>>(`${api}/heroes`)
      }

      deleteHero(hero: Hero) {
        return this.http.delete(`${api}/hero/${hero.id}`);
      }

      addHero(hero: Hero) {
        return this.http.post<Hero>(`${api}/hero/`, hero);
      }

      updateHero(hero: Hero) {
        return this.http.put<Hero>(`${api}/hero/${hero.id}`, hero);
      }
    }
    ```

    Tento kód používá nejnovější verzi modulu HttpClient, kterou Angular nabízí. Tento modul je potřeba zadat, takže to teď provedeme.

3. Ve Visual Studio Code otevřete soubor **app.module.ts** a importujte modul HttpClientModule tak, že aktualizujete část importování, aby zahrnovala HttpClientModule.

    ```ts
    imports: [
      BrowserModule,
      FormsModule,
      HttpClientModule
    ],
    ```

4. V souboru **app.module.ts** přidejte do seznamu k importování příkaz pro import modulu HttpClientModule.

    ```ts
    import { HttpClientModule } from '@angular/common/http';
    ```

5. Ve Visual Studio Code přejděte zpět do souboru **heroes.components.ts**. Všimněte si, že na řádku `constructor(private heroService: HeroService) {}` (řádek 13) má `HeroService` červené podtržení. Klikněte na `HeroService` a na levé straně bloku kódu se zobrazí žárovka. Klikněte na žárovku a pak klikněte na **Importovat HeroService z umístění ./hero.service**. nebo **Importovat HeroService z umístění client/app/hero.service**.

    Kliknutím na žárovku se vloží nový řádek kódu na řádku 2. Pokud řádek 2 odkazuje na složku client/app/hero.service, upravte ho tak, aby odkazoval na soubor hero v místní složce (./hero.service). Řádek 2 by měl vypadat takto:
    
    ```javascript
    import { HeroService } from "./hero.service"
    ```

6. Uložte všechny soubory ve Visual Studio Code.

## <a name="build-the-app"></a>Sestavení aplikace

1. Na příkazovém řádku zadejte následující příkaz, který sestaví aplikaci Angular. 

    ```bash
    ng b
    ``` 

    Pokud dojde k nějakým problémům, v okně terminálu se zobrazí informace o souborech, které potřebují opravu. Po dokončení sestavení se nové soubory přesunou do složky **dist**. Pokud chcete, můžete nové soubory ve složce **dist** zkontrolovat.

    Pojďme aplikaci spustit.

2. Ve Visual Studio Code klikněte na tlačítko **Ladit** ![Ikona Ladit ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png) na levé straně a pak klikněte na tlačítko **Spustit ladění** ![Ikona Spustit ladění ve Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/start-debugging-button.png).

3. Nyní otevřete internetový prohlížeč a přejděte na adresu **localhost:3000**, kde se zobrazí místně spuštěná aplikace.

     ![Místně spuštěná aplikace Heroes](./media/tutorial-develop-mongodb-nodejs-part3/azure-cosmos-db-mongodb-mean-app.png)

## <a name="next-steps"></a>Další kroky

V této části kurzu jste provedli následující:

> [!div class="checklist"]
> * Vytvořili jste uživatelské rozhraní v Angular.
> * Místně jste otestovali aplikaci.

Můžete přejít k další části kurzu a vytvořit účet služby Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vytvoření účtu služby Azure Cosmos DB pomocí Azure CLI](tutorial-develop-mongodb-nodejs-part4.md)

