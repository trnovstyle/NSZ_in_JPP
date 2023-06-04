# Vrednotenje nepozidanih stavbnih zemljišč za stanovanjsk ogradnjo z vidika ustreznosti javnega potniškega prometa

Koda je del analize za prispevek, ki bo objavljen v zbirki regionalni dnevi. Podrobnejši opis metodologije bo na voljo v samem prispevku. Ker je prispevek še v fazi recenziranja tu tudi še ni objavljene končne različice programske kode.

Procedura za identifikacijo nepozidanih stavbnih zemljišč ter prekrivanje z vplivnimi območji postajališč javnega potniškega prometa.
analiza sestoji iz treh korakov:
1. identifikacija nepozidanih stavbnih zemljišč;
2. izračun dostopnosti JPP na območju nepozidanih stavbnih zemljišč;
3. analiza zadostnosti nepozidanih stavbnih zemljišč na območju ustrezne dostopnosti JPP.


Potrebni podatki:
 1. postajališča JPP s pogostnostjo voženj
 2. Generalizirana namenska raba prostora, dostopno na poratlu prostorski informacijski sistem: https://dokumenti-pis.mop.gov.si/javno/veljavni/
 3. Sloj skupne dejanske rabe: https://ipi.eprostor.gov.si/jgp/data 

Postopek analize:
  1. Instaliraj PostgreSQL in PostGIS
  2. Ustvari novo bazo in sheme (kasneje se lahko doda kot SQL koda)
  3. Uvozi podatke
  4. Zaženi PostGIS analiza.sql
  5. Analiziraj podatke in jih prikaži v GIS-u

Avtor: Simon Koblar, 2022

