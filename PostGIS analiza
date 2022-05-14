--DROP TABLE proc.jpp_buff;

-- naredi bufferje
CREATE TABLE proc.jpp_buff AS
SELECT row_number() over(), sub.frekvenca, ST_Union(ST_Buffer(geom, 500)) AS geom
FROM(
	SELECT
	*,
		CASE 
	WHEN trips_coun >0 AND trips_coun <= 16 THEN 'r1'
		WHEN trips_coun >16 AND trips_coun <= 46 THEN 'r2'
		WHEN trips_coun >46 THEN 'r3'
		ELSE 'r0'
		END frekvenca
	FROM data.jpp_frekvence_2021
	) sub
GROUP BY sub.frekvenca;
CREATE INDEX ON proc.jpp_buff USING gist (geom);
-- naredi posamezna območja za JPP
	-- R_3
CREATE TABLE proc.JPP_R3 AS
	SELECT 'r3' AS FREK_JPP,
			R_3.GEOM
		FROM
			(SELECT *
				FROM proc.JPP_BUFF
				WHERE FREKVENCA IN ('r3')) AS R_3;
-- R2
CREATE TABLE proc.JPP_R2 AS
	(SELECT 'r2' AS FREK_JPP, ST_DIFFERENCE(R2.GEOM,

										R3.GEOM) AS GEOM
		FROM
			(SELECT *
				FROM proc.JPP_BUFF
				WHERE FREKVENCA IN ('r2')) AS R2,

			(SELECT *
				FROM proc.JPP_BUFF
				WHERE FREKVENCA IN ('r3')) AS R3);
-- R1
CREATE TABLE proc.JPP_R1 AS
	(SELECT 'r1' AS FREK_JPP, ST_DIFFERENCE(R1.GEOM, R23.GEOM) AS GEOM
		FROM
			(SELECT *
				FROM proc.JPP_BUFF
				WHERE FREKVENCA IN ('r1')) AS R1,

			(SELECT ST_UNION(GEOM) AS GEOM
				FROM proc.JPP_BUFF
				WHERE FREKVENCA IN ('r3','r2')) AS R23);
				
-- R123
CREATE TABLE proc.JPP_R123 AS
SELECT 'r123' AS FREK_JPP, ST_UNION(GEOM) AS GEOM
				FROM proc.JPP_BUFF;

--create index
CREATE INDEX ON proc.JPP_R1 USING gist (geom);
CREATE INDEX ON proc.JPP_R2 USING gist (geom);
CREATE INDEX ON proc.JPP_R3 USING gist (geom);	
-- CREATE INDEX ON proc.JPP_R12 USING gist (geom);	
CREATE INDEX ON proc.JPP_R123 USING gist (geom);	
-- CREATE INDEX ON proc.JPP_R23 USING gist (geom);
CREATE INDEX ON data.nrp_slo_tm USING gist (geom);

-- združi JPP območja v en sloj za nadaljne izračune
CREATE TABLE proc.jpp_buff_class AS
SELECT *
FROM proc.JPP_R1
UNION 
SELECT *
FROM proc.JPP_R2
UNION 
SELECT *
FROM proc.JPP_R3;
CREATE INDEX ON proc.jpp_buff_class USING gist (geom);

-- nrp kjer ni dostopa z JPP
CREATE TABLE proc.nrp_jpp_nedostopno AS
SELECT objectid,'r0' as frek_jpp, ob_id,onrp,pnrp1,pnrp2,tip_akta,nrp_opis,nrp_vir,nrp_leto,nrp_opombe,priprava_o,datum,
ST_Difference(nrp_slo_tm.geom, jpp.geom) as geom
FROM data.nrp_slo_tm, proc.jpp_r123 AS jpp;
CREATE INDEX ON proc.nrp_jpp_nedostopno USING gist (geom);

-- nrp kjer je dostop z JPP
CREATE TABLE 
proc.nrp_dostopno AS
SELECT objectid,frek_jpp, ob_id,onrp,pnrp1,pnrp2,tip_akta,nrp_opis,nrp_vir,nrp_leto,nrp_opombe,priprava_o,datum, ST_Intersection(postaje.geom,  nrp_slo_tm.geom) AS geom
FROM proc.jpp_buff_class postaje
INNER JOIN data.nrp_slo_tm
ON ST_Intersects(postaje.geom, nrp_slo_tm.geom);
CREATE INDEX ON proc.nrp_dostopno USING gist (geom);

-- združi nedostopna in dostopna območja v en sloj za nadaljne izračune
CREATE TABLE proc.nrp_jpp AS
SELECT *
FROM proc.nrp_dostopno
UNION 
SELECT *
FROM proc.nrp_jpp_nedostopno;
CREATE INDEX ON proc.nrp_jpp USING gist (geom);

-- prekrij z dejansko rabo MKGP
CREATE TABLE 
proc.nrp_jpp_raba AS
SELECT objectid,frek_jpp, ob_id,onrp,pnrp1,pnrp2,tip_akta,nrp_opis,nrp_vir,nrp_leto,nrp_opombe,priprava_o,datum, raba_id, ST_Intersection(nrp_jpp.geom,  raba.geom) AS geom
FROM proc.nrp_jpp
INNER JOIN data.mkgp_raba raba
ON ST_Intersects(nrp_jpp.geom, raba.geom);
CREATE INDEX ON proc.nrp_jpp_raba USING gist (geom);
