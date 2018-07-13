# Vlastnosti desky RB3201-RBControl

## Určení a cíl

RB3201 - RBControl (RBC) je univerzální deska pro stavbu hobby robotů. Jde v podstatě o shield k desce 
 [ESP32 dev kit](https://www.espressif.com/en/products/hardware/esp32-devkitc/overview), který má dva hlavní cíle: rozšířit počet pinů desky ESP32 a umožnit snadné připojení velkého množství různých periférií. 

## Hlavní vlastnosti 

Deska RBC umožňuje současně ovládat až 8 DC motorů (1,5 A trvale, 2A špičkově každý). Dále umí po osazení spínanými zdroji napájet a ovládat 4 serva nebo 8 mikroserv. Má vyvedeno celkem 6 I2C sběrnic na 3,3 V a 2 I2C sběrnice na 5V. Dále je na desce expandér pinů, který je připojený na I2C a obsluhuje další dva porty A,B po 8 pinech. Na desce jsou vyvedená tři tlačítka, 4 LED a piezo. 

## Další vlastnosti 

Po osazení tranzistorem Q3 je deska chráněná proti přepólování. Přímo na desce je možné měřit reálné hodnoty napětí 3,3V a
5V rozvedených po desce. 
K RBC s ESP32 je možné připojit další RBC bez ESP32 a rozšířit tak počet periférií. Také je možné připojit k I2C na 5V libovolné externí napětí a provozovat I2C na tomto napětí. 

## Napájení

Napájení desky RBC je ideální ze dvou Li-On baterií, které dodávají asi 8 V a dostatečné proudy. K desce lze připojit napětí až do 10V, připojení vyššího napětí neumožňují použité drivery pro motory. 

RBC si hlídá napětí na baterii a umí ho měřit do 10 V. 
V ESP32 je softwarově (v knihovně) nastavené napětí 7,2 V, při kterém ESP32 vypne desku, aby nedošlo k podvybití baterie.  

7805 tvoří napětí 5V pro desku, z toho se tvoří 3,3V na stabilizátoru na desce ESP dev kit -> při napájení pouze z USB nebude fungovat rozvod 5V na desce 
7805 dává asi 1A , většinu spotřebuje deska sama , spínaný zdroj by zvládl 2A 
piny vpravo lze zapojit na "centrální zdroj" - propojuje se  jumpery, piny tvoří kaskádu 
(serva zvládnou i 6V, jsou na to stavěná  ) 

## Expandér
### Popis expandéru


Expandér funguje na 3,3 V , má dva porty A, B - každý má 8 pinů, A je pro uživatele, B je pro tlačítka, LED a vypínání 

Možnosti : 

driver má ochranu proti přetížení i přehřátí -> vypne 
řada pinů za svorkovnicemi - připojení motorů a enkodérů 
do motorů jde baterkové napětí, + 5V napájení enkodérů 
z enkodérů jdou 3,3V (čip si drží 3,3 a sonda když chce ho přetáhne k zemi <--> hallova sonda má otevřený kolektor 
a neumí se proto sama přepnout na logickou 1 ) čínské motory by se měly chovat stejně, protože mají na sobě stejné hallovy sondy, 
mají také navíc pull-upy k 5V, ale tak velké, že by to ESP mělo přežít 
je potřeba si zjistit, které piny jsou použité pro enkodéry a nepoužívat je na něco jiného 

 I2C jsou 2+4 x 3,3 V + 2x 5V 
dále jsou I2C na IN a OUT - použije se v případě zřetězní desek - je možné jedním ESP ovládat další desku s motory, ale bez enkodérů 
lze použít i samostatně (pouze piny SCL, SDA, GND, 3V3) 
26 pinů, z toho 4 pouze vstupní, na dvou je sériová linka, na dvou je I2C, 3 piny jsou komunikace s drivery pro motory, 1 pin měří napětí na baterii 
zbývá 14 

( expandér pinů je připojený na I2C )

Připojení další RBC desky. 

# Popis rozložení pinů na desce RBC

Pro snazší orientaci si otočte desku tak, aby oblouk na desce byl vpravo. 
Při popisu budeme postupovat zleva doprava a shora dolů. 

1. Svorkovnice pro připojení DC motorů 

2. Piny pro připojení motorů s enkodéry 

3. Výstupní piny, slouží signálovému propojení s další deskou RBC. 

4. Výstupní napájení. Použije se v případě připojení další RBC desky. 

5. Drivery pro DC motory. Každý driver poskytuje PWM napájení pro dva motory. 

čtveřice pinů nahoře: 
Reset, ON, OFF, vypínání desky 

zpětivoltovávač dává 5V signál (pouze signál z I2C) pro I2C 5V + piny pro inteligentní LED
 
piny vpravo - 3,3V , GND, sig, stejný sig, 5V ze spínaných zdrojů nebo jumperů , GND , jumpery pro spínané zdroje 
enable signály - zdroje jsou ve výchozím stavu zapnuté a lze je pinem enable vypnout - přivedu na ně kabel z jiného pinu 


na každý spínaný zdroj (step-down) jedno servo nebo dvě mikroserva 


teoreticky lze po proškrábnutí propojky přivést na pin vedle ní libovolné (do 30V) na 5V I2C a inteligentní LED - bez R7 a R6 a R4 a R15-R17 - v praxi radši nedělat 

----------------------
oblouk v pravo, OUT nahoře, IN dole 
 
