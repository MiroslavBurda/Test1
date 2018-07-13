# RBController 

## Určení a cíl

RBController je univerzální deska pro stavbu hobby robotů. Jde v podstatě o shield k desce 
 [ESP32 dev kit](https://www.espressif.com/en/products/hardware/esp32-devkitc/overview), který má dva hlavní cíle: rozšířit počet pinů této desky a umožnit snadné připojení velkého množství různých periférií. 

## Hlavní vlastnosti 

Deska umožňuje současně ovládat až 8 DC motorů (1,5 A trvale, 2A špičkově každý).  

## Napájení


Napájení ideální 2xLi-On -> 8V, lze až do 10V, víc drivery pro motory nezváldnou, taky měření napětí na baterce je do 10 V 
v ESP je softwarově (v knihovně) nastavené napětí 7,2 V, při kterém ESP vypne desku, aby nedošlo k podvibití baterek 
7805 tvoří napětí 5V pro desku, z toho se tvoří 3,3V na stabilizátoru na desce ESP dev kit -> při napájení pouze z USB nebude fungovat rozvod 5V na desce 
7805 dává asi 1A , většinu spotřebuje deska sama , spínaný zdroj by zvládl 2A 
piny vpravo lze zapojit na "centrální zdroj" - propojuje se to jumpery, piny tvoří kaskádu 
(serva zvládnou i 6V, jsou na to stavěná  ) 

## Expandér

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

čtveřice pinů nahoře: 
Reset, ON, OFF, vypínání desky 

zpětivoltovávač dává 5V signál (pouze signál z I2C) pro I2C 5V + piny pro inteligentní LED
 
piny vpravo - 3,3V , GND, sig, stejný sig, 5V ze spínaných zdrojů nebo jumperů , GND , jumpery pro spínané zdroje 
enable signály - zdroje jsou ve výchozím stavu zapnuté a lze je pinem enable vypnout - přivedu na ně kabel z jiného pinu 


na každý spínaný zdroj (step-down) jedno servo nebo dvě mikroserva 


teoreticky lze po proškrábnutí propojky přivést na pin vedle ní libovolné (do 30V) na 5V I2C a inteligentní LED - bez R7 a R6 a R4 a R15-R17 - v praxi radši nedělat 

----------------------
oblouk v pravo, OUT nahoře, IN dole 
 
