# Namenska raba in JPP
Procedura za prekrivanje namenske rabe prostora, rabe tal MKGP in vplivnih območij postajališč.

Potrebni podatki:
 1. postajališča JPP s pogostnostjo voženj
 2. Generalizirana namenska raba prostora, dostopno na poratlu prostorski informacijski sistem: https://dokumenti-pis.mop.gov.si/javno/veljavni/
 3. Raba tal MKGP, dostopno na naslovu: https://rkg.gov.si/vstop/

Postopek analize:
  1. Instaliraj PostgreSQL in PostGIS
  2. Ustvari novo bazo in sheme (kasneje se lahko doda kot SQL koda)
  3. Uvozi podatke
  4. Zaženi PostGIS analiza.sql
  5. Analiziraj podatke in jih prikaži v GIS-u

Avtor: Simon Koblar, 2022
