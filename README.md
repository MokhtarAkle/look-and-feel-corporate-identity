# Link: https://mokhtarakle.github.io/the-client-case/

# Corporate identity Coding the Curbs
Ontwerp en maak voor een opdrachtgever een component of website op basis van een bestaande huisstijl

## Inhoudsopgave

  * [Beschrijving](#beschrijving)
  * [Kenmerken](#kenmerken)
  * [Bronnen](#bronnen)
  * [Licentie](#licentie)
  
## Beschrijving
Het doel van de opdracht deze sprint was het toepassen van een bestaande huisstijl op je gemaakte project. Verder heb ik zelf veschillende user stories uitgekozen om uit te werken of om verder aan te werken. Er is een werkend filter en sorteerfunctie toegevoegd en alle elementen zijn aangepast aan de huisstijl zoals uitgewerkt in de [Living styleguide](https://luukbrauckmann.github.io/look-and-feel-living-styleguide/#home) 

![image](https://user-images.githubusercontent.com/45001009/209254339-c88d2d68-70da-426d-bc0d-62ac23691ed8.png)

### Gekozen user stories:

_Als gebruiker wil ik gegevens kunnen opzoeken en filteren, zodat ik makkelijker achter informatie kom_

_Huisstijl op de juiste manier toepassen_

### Filter functie
Er is een werkende filter functie toegevoegd waarmee je de bestaande smartzones kan filteren op al de verschillende kenmerken in verschillende stappen. De huidige implementatie kan maar 1 filter tegelijk toepassen maar dit zal in de toekomst veranderen. Door gebruik te maken van de filters kan je smartzones verstoppen die niet voldoen aan de opgegeven filters. De filter functionaliteit is met JS gebouwd en voldoet aan de bestaande huisstijl zoals beschreven is in de styleguide.  

![image](https://user-images.githubusercontent.com/45001009/209254623-a9c90e47-c341-4cc2-ab2d-a653206cbd1f.png)

### Sorteer functie
Met de sorteer functie kan je de bestaande smartzones sorteren op 1 van de 4 gebruikte categorieëen. De sorteerfunctie kan de smartzones van groot naar klein of andersom sorteren.

![image](https://user-images.githubusercontent.com/45001009/209255305-5354960f-3bc8-430e-8a81-ac8cbf9dc608.png)

## Kenmerken
De website is opgebouwd uit HTML, CSS, JS en de google maps javascript API.

## HTML

### Filter
```

<li class="filterHidden">
                        <p>Filter Op: </p>
                        <p>Locatie: </p>
                        <input type="checkbox" name="filterDistance" value="distance" id="distanceFilter">
                        <label for="distanceFilter">&lt; 5 km </label>
                        <input type="checkbox" name="filterLoc" value="Utrecht" id="utrFilter">
                        <label for="utrFilter">Utrecht</label>
                        <input type="checkbox" name="filterLoc" value="Schiedam" id="schieFilter">
                        <label for="schieFilter">Schiedam</label>
                        <input type="checkbox" name="filterLoc" value="Amsterdam" id="amsFilter">
                        <label for="amsFilter">Amsterdam</label>
                        <input type="checkbox" name="filterLoc" value="Rotterdam" id="rotFilter">
                        <label for="rotFilter">Rotterdam</label>
                        <p>Functie: </p>
                        <input type="checkbox" name="filterFunc" value="Parkeren" id="parkFilter">
                        <label for="parkFilter">Parkeren</label>
                        <input type="checkbox" name="filterFunc" value="Laden en lossen" id="loadFilter">
                        <label for="loadFilter">Laden en Lossen</label>
                        <input type="checkbox" name="filterFunc" value="Deelmobiliteit" id="deelFilter">
                        <label for="deelFilter">Deelmobiliteit</label>
                        <p>Grootte: </p>
                        <input type="checkbox" name="filterGrootte" value="1 plek" id="kleinFilter">
                        <label for="kleinFilter">1 plek</label>
                        <input type="checkbox" name="filterGrootte" value="1,5 plek" id="midFilter">
                        <label for="midFilter">1.5 plek</label>
                        <input type="checkbox" name="filterGrootte" value="2 plek" id="bigFilter">
                        <label for="bigFilter">2 plek</label>
                        <p>Gebruik: </p>
                        <input type="checkbox" name="filterGebruik" value="25" id="25Filter">
                        <label for="25Filter">&lt; 25%</label>
                        <input type="checkbox" name="filterGebruik" value="50" id="50Filter">
                        <label for="50Filter">&lt; 50%</label>
                        <input type="checkbox" name="filterGebruik" value="75" id="75Filter">
                        <label for="75Filter">&lt; 75%</label></li>
                     
```
Het filter is opgebouwd uit verschillende `<p>` elementen die als labels voor de categorieëen functioneren. Verder bestaat het uit verschillende inputs en labels. De inputs zijn allemaal van het type `checkbox` en hebben allemaal een unieke `value` en `id`. De `id` wordt gebruikt om de labels toe te voegen aan de correcte checkbox en de value wordt gecomuniceerd als er verandering plaatsvindt in de `input`. De namen zijn uniform per categorie en worden gebruikt om in JS de categorieëen functionaliteit te geven in een groep.

### Sorteer

```
                    <li class="sortHidden">
                        <p>Sorteer op: </p>
                        <button class="sortSize">Grootte</button>
                        <button class="sortUse">Gebruik</button>
                        <button class="sortFunc">Functie</button>
                        <button class="sortLoc">Locatie</button>
                    </li>
```

De HTML voor de sorteerfunctie bestaat uit 1 `<p>` element als label en 4 buttons met unieke classes. Deze knoppen worden gebruikt om via JS de sorteer functionaliteit uit te voeren.

## JS

### populate() functie
```
function populate(usedArray){
  for (let i = 0; i < usedArray.length; i++){
    populateName[i].textContent = usedArray[i].name;
    populateLocation[i].textContent = usedArray[i].location + ", " + usedArray[i].town;
    populateSize[i].textContent = usedArray[i].size;
    populateUse[i].textContent = usedArray[i].utilization;
    populateFunction[i].textContent = usedArray[i].function + " | " + "\r\n" + usedArray[i].function1 + " | " +  usedArray[i].function2 ;
    listInformation[i].style.display = "block";
    if(parseInt(usedArray[i].utilization) < 50){
      populateUse[i].classList.add("lowFill");
    }
    else if(parseInt(usedArray[i].utilization) > 50 && parseInt(usedArray[i].utilization) < 75){
      populateUse[i].classList.add("mediumFill");
    }
    else if(parseInt(usedArray[i].utilization) > 75){
      populateUse[i].classList.add("highFill");
    }

    if(usedArray[i].function1 == " "){
      populateFunction[i].textContent = usedArray[i].function
    }
    else if(usedArray[i].function2 == " "){
      populateFunction[i].textContent = usedArray[i].function + " | "  + "\r\n" + usedArray[i].function1
    }
  }
}

```
Om gebruik te maken van het filter systeem en de sorteerfunctie heb ik de populate functie aangepast door hier een parameter aan toevoegen. De `populate()` functie maakt gebruik van `.textContent` om de smartzones in de lijst hun content te geven. Deze elementen worden gelijk gesteld aan een array dat als parameter toegevoegd wordt als de functie wordt aangeroepen. Ook heb ik verschillende if statements toegevoegd die de juiste kleuren toevoegen aan het `utilization` element en om functies te verstoppen als deze niet voorkomen in een smartzone.

### depopulate() functie

```
function depopulate(){
  for (let i = 0; i < populateName.length; i++){
    populateName[i].textContent = " ";
    populateLocation[i].textContent = " ";
    populateSize[i].textContent = " ";
    populateUse[i].textContent = " ";
    populateFunction[i].textContent = " ";
    listInformation[i].style.display = "none";
    populateUse[i].classList.remove("lowFill", "mediumFill", "highFill");
  }
}
```
Om de veranderingen in de arrays visueel weer te geven is het nodig geweest om een depopulate functie te implementeren. De depopulate() functie doet het tegenovergestelde van de populate() functie en reset als het ware alle elementen in de smartzones lijst naar niets. Dit zorgt ervoor dat veranderingen in de weergave niet overlopen als je via JS veranderingen aanbrengt.

### Filter functie

```
function filterTest(){
  
  for (let i = 0; i < checkboxes.length; i++){
    checkboxes[i].addEventListener("change", () =>{
      if(checkboxes[i].name == "filterLoc" && checkboxes[i].checked){
        let locCheck = smartzones.filter(location => location.town == checkboxes[i].value);
        depopulate();
        populate(locCheck);
        buttonPopulate(locCheck);
      }
      else if(checkboxes[i].name == "filterFunc" && checkboxes[i].checked){
        let funcCheck = smartzones.filter(functionality => functionality.function == checkboxes[i].value);
        console.log(checkboxes[i].value)
        depopulate();
        populate(funcCheck);
      }
      else if(checkboxes[i].name == "filterGrootte" && checkboxes[i].checked){
        let sizeCheck = smartzones.filter(area => area.size == checkboxes[i].value);
        depopulate();
        populate(sizeCheck);
      }
      else if(checkboxes[i].name == "filterGebruik" && checkboxes[i].checked){
        let usageCheck = smartzones.filter(usage => parseInt(usage.utilization) < parseInt(checkboxes[i].value));
        console.log(checkboxes[i].value)
        depopulate();
        populate(usageCheck);
      }
      else{
        depopulate();
        populate(smartzones);
      }
    });
  }
  }
  filterTest();
}
```
De filter functie checkt een bestaand array op een bepaalde waarde en maakt dan een kopie van dat array zonder de gefilterde waarde. Dit wordt visueel weergeven als het verstoppen van smartzones die niet voldoen aan de opgegeven filters. Door gebruik te maken van een for loop wordt de functie toegepast op alle checkboxes die door de queryselector zijn geselecteerd. Hier wordt vervolgens een eventListener aan toegevoegd om te luisteren naar een verandering, in dit geval is dit het aanvinken van een checkbox. Als er een checkbox wordt aangevinkt dan wordt er gebruik gemaakt van een if statement om deze te checken tegenover de opgegeven namen van de filter categorieëen. Vervolgens wordt er gebruik gemaakt van de .filter functionaliteit om het originele smartzones array te filteren op de opgegeven value van de gecheckte checkbox. Dit array wordt opgeslagen in 1 van de gemaakte variabele en vervolgens meegegeven aan populate() als parameter. Dit zorgt ervoor dat populate() nu de kopie van de smartzone array weergeeft waar de gefilterde elementen zich niet meer bevinden.

### Sorteer functie

```
  function initSort(){
  
  sortSize.addEventListener("click", () =>{
    smartzones.sort((a, b) => {
  const sizeA = a.size.toUpperCase(); 
  const sizeB = b.size.toUpperCase(); 
  if (sizeA < sizeB) {
  return -1;
  }
  if (sizeA > sizeB) {
  return 1;
  }
  depopulate();
  populate(smartzones);
  return 0;
  });
  });

  sortUse.addEventListener("click", () =>{
    smartzones.sort((a, b) => {
  const useA = a.utilization.toUpperCase(); 
  const useB = b.utilization.toUpperCase(); 
  if (useA < useB) {
  return -1;
  }
  if (useA > useB) {
  return 1;
  }
  depopulate();
  populate(smartzones);
  return 0;
  });
  });

  sortFunc.addEventListener("click", () =>{
    smartzones.sort((a, b) => {
  const funcA = a.function.toUpperCase();
  const funcB = b.function.toUpperCase();
  if (funcA < funcB) {
  return -1;
  }
  if (funcA > funcB) {
  return 1;
  }
  depopulate();
  populate(smartzones);
  return 0;
  });
  });

  sortLoc.addEventListener("click", () =>{
    smartzones.sort((a, b) => {
  const locA = a.town.toUpperCase();
  const locB = b.town.toUpperCase();
  if (locA < locB) {
  return -1;
  }
  if (locA > locB) {
  return 1;
  }
  depopulate();
  populate(smartzones);
  return 0;
  });
  });
}
```
De sorteer functie maakt gebruik van .sort om een bestaand array te sorteren van hoog naar laag of vice versa. Dit wordt weergeven in de lijst door de smartzones te sorteren op hun geselecteerde categorieëen. Er wordt eerst een eventListener toegevoegd aan de verschillende knoppen die zijn opgesteld voor de sorteer functie. Vervolgens wordt de .sort functie toegepast op het originele smartzone array met de variabele a en b. Deze variabele worden gelijk gesteld aan 1 van de categorieëen van de array en alle elementen hierin worden vervolgens vergelijkt. Het array wordt dan van groot naar klein gesorteerd en opgeslagen. Dit gesorteerde array wordt vervolgens doorgegeven als parameter aan populate() en weergeven als een gesorteerde lijst.

## Bronnen

## Licentie

![GNU GPL V3](https://www.gnu.org/graphics/gplv3-127x51.png)

This work is licensed under [GNU GPLv3](./LICENSE).
