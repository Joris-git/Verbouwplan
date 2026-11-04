# Verbouwplan begane grond

Een plattegrond van de begane grond op schaal, waarin je zelf kunt uitproberen hoe je het huis anders indeelt: de badkamer verplaatsen, muren slopen of bijbouwen, sanitair verschuiven.

**Open de app:** https://joris-git.github.io/Verbouwplan/

## Zo werkt het

1. **Bekijk de tekening.** Bovenin is de tuin, onderin de straat. Bij het eerste bezoek krijg je een korte uitleg. Die vind je altijd terug onder **?**.
2. **Tik en sleep.** Tik op een toilet, douche of wastafel om hem te kiezen, en sleep hem naar een andere plek. Hij klikt vast tegen muren. De rode getallen zijn de afstand tot de muren, in centimeters.
3. **Voeg iets toe.** Via **+ Toevoegen** (op een computer: de lijst links) zet je een nieuw toilet, bad, douche, wastafel of muur neer. Met **Draai 90°**, **Nog een** en **Verwijder** pas je het aan.
4. **Muren.** Tik een muur eerst aan en sleep hem daarna. Zo schuif je hem niet per ongeluk mee. Een muur schuift alleen dwars op zijn richting. Muren die eraan vastzitten worden vanzelf langer of korter, deuren en ramen in de muur schuiven mee en de kamers ernaast worden groter of kleiner. Met de blokjes aan de uiteinden maak je een muur langer of korter. **Slopen** haalt hem weg. Een gearceerde muur is dragend: daarvoor heb je een constructeur nodig.
5. **Controleer of het past.** De blauwe zone voor sanitair is de ruimte die je nodig hebt om het te gebruiken. Wordt die rood, dan staat het te krap. De bruine stip rechts is de standleiding (de hoofdafvoer). Hoe dichter een toilet daarbij staat, hoe eenvoudiger de afvoer.
6. **Vergelijk varianten.** De huidige situatie blijft altijd bewaard. Bij je eerste wijziging maakt de app automatisch *Variant 1* aan. Wissel bovenaan tussen varianten. Tik op een lege plek voor het overzicht: wat er gesloopt en bijgebouwd wordt, en de m² per ruimte.
7. **Bekijk het in 3D.** Tik bovenin op **3D**. Je ziet de variant die openstaat als maquette. Sleep om te draaien en knijp of scroll om te zoomen. Met **Schuin**, **Boven**, **Tuinzijde** en **Straatzijde** kies je een kijkrichting. **Muren laag** zet de muren op halve hoogte, zodat je beter in de kamers kijkt. Kies **Rondkijken** en tik op de vloer om in een kamer te staan. Sleep dan om rond te kijken. Nieuwe muren zijn rood en wat je sloopt is doorzichtig rood. Verschuiven doe je in **2D**. Een element aantikken in 3D kan wel, en dan werken Draai 90°, Slopen en Verwijder gewoon.

8. **Vertel wat je wilt.** Tik op **Chat** en typ bijvoorbeeld "zet het toilet 50 cm naar links" of "zet een ligbad in slaapkamer 2". Claude past de tekening aan in een variant en vertelt wat het heeft gedaan. Met ↶ draai je de hele opdracht in één keer terug. Hiervoor heb je een eigen API-sleutel van Anthropic nodig ([console.anthropic.com](https://console.anthropic.com/settings/keys)). Die staat los van een Claude-abonnement, en een opdracht kost meestal 5 tot 20 cent. De sleutel blijft alleen in je browser. Heb je geen sleutel? Kopieer je variant als tekst, plak die in een gewone Claude-chat met je vraag, en plak het antwoord terug in *Openen als nieuwe variant*.

**Foutje?** ↶ draait je laatste stap terug.

## Goed om te weten

- Varianten worden bewaard in de browser waarin je werkt. Om een variant op een ander apparaat of bij iemand anders te openen: kies in het overzicht *Kopieer variant als tekst* en plak die tekst daar in het vak *Openen als nieuwe variant*.
- De tekening is gebaseerd op de bouwtekening uit 1997. Maten kunnen enkele centimeters afwijken. Meet altijd na voordat je iets bestelt of laat maken.
- Voor de 3D-weergave heb je internet nodig: die laadt de eerste keer een extra onderdeel (Three.js). In 3D is het plafond 2,60 m, zijn deuren 2,10 m hoog en zitten ramen vanaf 90 cm. Dat zijn aannames, geen maten van de tekening.
- Bij de chat gaat je tekening (zonder adres) naar Anthropic, zodat Claude je vraag kan beantwoorden.
- De app geeft een eerste indruk. Een aannemer, installateur of constructeur bepaalt wat technisch kan.
