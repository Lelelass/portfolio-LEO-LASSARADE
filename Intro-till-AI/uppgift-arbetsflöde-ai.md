# Arbetsflöde för AI-projekt
## Maskininlärningsalgoritmen linjär regression
## Scenariot : Förutspå huspriser baserat på olika ”features”  
&nbsp;

### 1. Samla in och sprara datan  

&nbsp;

För just förutspåning av huspriser, finns det en del träningsset som ges fri tillgång till med pedagogisk syfte. T.ex finns det en *Real estate price prediction* dataset hos *kaggle.com*.[^1] Att titta på en sådan kan ge en bra indikation om de variablerna som kan påverka huspriserna, och därför om vilken data man ska leta efter.

Vill man samla in data om den svenska bostadsmarknaden, kan man först vända sig till öppna resurser. Det finns data på https://www.maklarstatistik.se/ som är fritt att använda. Där finns det variabel såsom område, pris vid tidpunkt, utveckling att plocka fram.

Man behöver dock utvecklad data som är bunden till enstaka bostäder (antal rum, byggår, balkong eller ej, etc.) om man vill ha en mer realistiskt och användbar modell. Det är osannolikt att alla bostadsobjekt i ett område är precis lika. Som tur är, så ger nationellstora webbsidor såsom hemnet.se och booli.se ut enorm mängd information. Booli ger till och med tillgång till sin data genom en API[^2]

Finns det inget enkelt och direkt sätt att hämta upp data, så kan man använda sig av Web crawling. Python är bra för att for att skapa webbspindlar. I så fall konfigurerar man i förhand på vilket sätt datan som hämtas upp sparras.[^3]
Slutpriser för objekt kan bli hittade på olika sidor, ofta är varje bostadsobjekt kopplad till ytterligare labbeled-data. (kvarter, antal rum etc).
Problemet är rättigheter, där datan som finns på sidorna är inte garanterat öppet och kan inte återanvändas hur som helst, teoretiskt.

Det är sannolikt att man också kommer vilja ha positioneringsdata för bostadsobjekt. Detta kan också vidare kopplas till distans till en skola, kollektivtrafikhållplats osv. Här kan data komma som koordinater.

&nbsp;


### 2. Visualisera datan

&nbsp;

För att få en snabb inblick av datan man fått fram, så kan man skapa en Pandas DataFrame
av varje dataset och köra metoder som .head() etc. för att få en snabb inblick av datastrukturen.

För att sen bygga upp grafer och alla möjliga sätt att visualisera sin data — med hjälp av Python i vårt fall — kan man använda sig av Matplotlib. Det är en av de äldsta och fortfarande mest använda verktygen för datavisualisering med Python. Den har ursprung i Matlab som redan var i bruk under 80:talet.[^4]

Det finns moduler som är byggda på Matplotlib som Seaborn, om man vill fräscha upp den grafiska sidan. Det finns också helt andra libraries såsom gg-plot, bokeh som har var sina fördelar vid specifika problem.

&nbsp;


### 3. Bearbeta datan till rätt format

&nbsp;

För att bearbeta data i rätt format, så är det viktigt att veta i förväg vilken är rätt format. Det är bra att identifiera detta redan i första steget ett målformat innan man börjar plocka upp data.

Är det så att man har hämtat ut data från olika källor, så kan man hämna med en olika formatering för varje dataset. Datasetsformat kvalité kan också variera kraftigt. På en källa kan man ha fått en nästintill färdig dataset, som kan t.o.m. vara användbart direkt i sitt original skick i våran slutliga dataframe. Samtidigt som man kan ha en "röra" efter att ha web-crawlat. Har man gps-koordinater, så är det möjligt att man måste formatera om.

Oavsett är det ideellt att i ett första steg, säkra att allt sin rådata kan vara på .csv format.

Om man ska under processens gång använda en Pandas dataframe, så måste man vara säkert på att data som hämnas i cellerna är bearbetbar. Man måste också tänka på hanteringen av "Null" värdena som kan uppkomma om man har hål i dataset.

&nbsp;

### 4. Linjär regression

&nbsp;

Linjär regression är en typ av regressionsanalys i vilken det finns en beroende(y) variabel och en oberoende(x) variabel, och där det finns en linjär förhållande mellan dessa två variabler.[^5]

linjen som representerar det bästa alternativen skapas av följande ekvation:

$y = A + B . x$


Målet är att hitta de bästa värdena för A och B. Där *error* mellan det förväntade värdet och det rejäla värdet är minst.

Man börjar med att använda  **Cost function**, som också kallas för Mean Squared Error **(MSE)**:

$$
minimize \frac{1}{n} \sum_{i=1}^{n}{(pred_{i}-y_{i})^2}
$$

Sen, man ska gå igenom **Gradient Descent** för att uppdatera värden av A och B på så sätt så att *MSE* förminskas. Man börjar med startvärde för A och B och byter värden steg för steg för att komma närmare botten av MSE funktionen. I linjär regression har MSE funktionen en *Convex* utseende. Storleken av stegen man väljer att ta kallas för **Learning Rate**. Är de små kan det ta tid, är de för stora kan man missa botten.

Nya värden för A och B hittas genom "Partial derivatives"

&nbsp;




### 5. Driftsätta modellen

&nbsp;



Det går bland annat att lägga sin AI-model på en cloud server med hjälp av *Flask*. Alla de största cloud tjänsterna såsom AWS, Google Cloud, Azure har inbyggd support för Python. Då kan man både installera packages och ha script som körs på molnen[^6], och som kan länkas med en web-application som user interface. Utbudet av plattformar för utnyttja sina AI-modeller är i en fas av ökning.[^7] 


&nbsp;

### 6. Teknologier som används i de olika stegen av maskininlärningsprocessen

&nbsp;

Jobbar man med Python så ska man först använda sig av de tidigare nämnda Libraries (2.) för att lada och visualisera sin data. Det är viktigt eftersom att vi behöver plocka två parametrar i en linear regression, och att visualisera datan kan hjälpa oss med att plocka de som är mest passande. Denna process är kallad **feature engineering** [^8]. Ifall av bostadsmarknaden brukar man välja boyta och pris.

Sen ska man dela sin data mellan träning och test. Från detta steg är det vanligt att använda **scikit learn** library, som har en metod för en sån delning.

En *linear regression model* kan importeras för att jobba ifrån. Den körs över train data. Efter detta kan man analysera noggrannhet (accuracy) med **scikit**s inbyggd MSE mätningsfunktion.

Man kan avsluta med en Gradient Descent som är en *itterative optimization algorithm*.


&nbsp;

---

&nbsp;

### 7. Referenser

&nbsp;

[^1]https://www.kaggle.com/quantbruce/real-estate-price-prediction/

[^2]https://www.booli.se/p/api/

[^3] https://towardsdatascience.com/6-web-scraping-tools-that-make-collecting-data-a-breeze-457c44e4411d

[^4]https://mode.com/blog/python-data-visualization-libraries/

[^5]https://towardsdatascience.com/introduction-to-machine-learning-algorithms-linear-regression-14c4e325882a

[^6]https://cloud.google.com/python/docs/getting-started

[^7]https://towardsdatascience.com/10-ways-to-deploy-and-serve-ai-models-to-make-predictions-336527ef00b2

[^8]https://towardsdatascience.com/linear-regression-5100fe32993a

