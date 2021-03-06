# SCT-PDA
Documentació d'integració del servei SCT-PDA del Consorci AOC.


# **Via Oberta - SCT Denúncies**


**Índex**

1. [Introducció](#1)
2. [Transmissions de dades disponibles](#2)
3. [Missatgeria dels serveis](#3)
   1. [3.1 Enviament de remesa de butlletes (SCT_REMESA)](#3.1)
      1. [3.1.1 Petició–dades genèriques](#3.1.1)
      2. [3.1.2 Remesa](#3.1.2)
      3. [3.1.3 Resposta específica](#3.1.3)
   2. [3.2 Obtenció de rang de butlletes (SCT\_BUTLLETES)](#3.2)
      1. [3.2.1 Petició – dades específiques](#3.2.1)
      2. [3.2.2 Resposta – dades específiques](#3.2.2)
   3. [3.3 Descàrrega del catàleg de normatives (SCT\_CATALEG)](#3.3)
      1. [3.3.1 Petició – dades específiques](#3.3.1)
      2. [3.3.2 Resposta – dades específiques](#3.3.2)
 4. [Annexes](#4)
      1. [4.1 Càlcul del dígit de control d'un número d'expedient](#4.1)
      2. [4.2 Línia de cobrament i codi de barres d'una butlleta de sanció](#4.2)
      3. [4.3 Línia de cobrament](#4.4)
      4. [4.4 Codi de barres](#4.4)
      5. [4.5 Text dels avisos legals a incloure en les butlletes](#4.5)
 
 

# 1 Introducció <a name="1"></a>

Aquest document detalla la missatgeria associada al servei d&#39;enviament de denúncies al Servei Català de Trànsit(enendavantSCT).

Per a poder realitzar la integració cal conèixer prèviament la següent documentació:

- Document de Missatgeria Genèrica de la PCI del Consorci AOC.


# 2 Transmissions de dades disponibles <a name="2"></a>

Les dades disponibles a través del servei són les que es presenten a continuació:

| **EMISSOR** |
| --- |
| Servei Català de Trànsit |

| **PRODUCTE** | **MODALITAT** | **DESCRIPCIO** | 
| --- | --- | --- |
| SCT\_DENUNCIES | SCT\_REMESA | Enviament de remesa de butlletes(frontal **asíncron** de la PCI). |
| SCT\_DENUNCIES | SCT\_BUTLLETES | Obtenció de rang de butlletes(frontal **síncron** de la PCI). |
| SCT\_DENUNCIES | SCT\_CATALEG | Descàrregadel catàleg de normatives (frontal **síncron** de la PCI). |




# 3 Missatgeria dels serveis <a name="3"></a>


## 3.1 Enviament de remesade butlletes(SCT\_REMESA) <a name="3.1"></a>

Enviament de les denúncies al sistema del SCT previ a la seva incorporació a l'aplicació del Procediment Sancionador del SCT (PSN).

    
### 3.1.1 Petició–dades genèriques <a name="3.1.1"></a>

El fitxer corresponent a la remesa s’ha de referenciar al bloc de dades //Ficheros/Fichero de les dades genèriques de la sol·licitud.

| _Element_ | _Descripció_ |
| --- | --- |
| //Ficheros/Fichero/Contenido | Contingut del fitxer en cas de transferència per MTOM (en la crida correspon a la referència XOP del fitxer). |

    
### 3.1.2 Remesa <a name="3.1.2"></a>


![image](https://user-images.githubusercontent.com/32306731/137331712-bc31edd6-6b5b-4372-977b-6230938c4af6.png) Per limitacions del sistema del SCT el fitxer de remesa no pot superar les 50 butlletes.
Actualment el sistema del SCT únicament suporta l&#39;enviament de denúncies de tipus genèrica. No hi ha previsió a curt termini per rebre denúncies de velocitat o d&#39;alcoholèmia.
ElSCT processa les remeses diàriament a les20h.Les remeses enviades posteriorment no es processaran fins al dia següent.
Terminid&#39;enviament:

- Caducitat de les denúncies: les denúncies no poden ser anteriors a 2 mesos.

- El sistema de SCT treballa amb una finestra de processament de 3 dies laborables. Així,una remesa no pot contenir denúncies que puguin caducar abans de 3 dies des del&#39;enviament de la remesa. 


![image](https://user-images.githubusercontent.com/32306731/137331907-08511998-e779-482b-89d0-2cba19c4e1ed.png)

|  _Element_ | _Descripció_ |
| --- | --- | 
| /DADES/CAPCALERA/FITXER\_PDA |  Nom de fitxer de la remesa. Nomenclatura: BUTCCCCCAAAAMMDDHHMI_<num-ordre>.xml <ul><li>on:</li><li>BUT: prefix.</li><li>CCCCC: codi INE del municipi.</li><li>AAAAMMDD: data d’enviament de la remesa.</li><li>HHMI: hora de la remesa.</li><li>num-ordre: si s’envia més d’una remesa en un dia, és el número d’ordre(en cas contrari: 1).</li></ul> |
| /DADES/CAPCALERA/NUM_BUTLLETES | Número de butlletes que conté la remesa (màxim 50). |
| /DADES/BUTLLETES | Bloc de dades corresponent a les butlletes (El SCT estableix un màxim de 50 butlletes per remesa).  |
| /DADES/BUTLLETES/BUTLLETA | Bloc de dades corresponent a una butlleta. |


      
#### 3.1.2.1 Butlleta

![image](https://user-images.githubusercontent.com/32306731/137348264-8922fc1e-1ca9-4f4c-bb6d-5b375c0bf948.png)![image](https://user-images.githubusercontent.com/32306731/137348293-1f5af3a1-3abb-47e5-b409-626661985a40.png)


| _Element_ | _Descripció_ |
| --- | --- |
| //BUTLLETA/ACCIO | A:alta/B:anul·lació.Encas d&#39;anul·lació de butlletac al informar totes les dades de la butlleta, el motiu(MOTIU\_ANULLACIO) i la data d&#39;anul·lació(DATA\_HORA\_ANULLACIO). |
   | //BUTLLETA/ID\_BUTLLETA | <ul><li>Identificador de la butlleta en el sistema origen o identificador dins de la remesa. </li><li>Dins del sistema del SCT l’identificador de la denúncia són els elements SERVEI_TERRIT i NUM_EXPEDIENT.</li></ul>|
| //BUTLLETA/SERVEI\_TERRIT | Demarcació territorial: 08,17,25,43. |
| //BUTLLETA/NUM\_EXPEDIENT | Número d&#39;expedient.Identificador de la denúncia en el sistemade lSCT.És un número de butlleta del rang de números de butlleta que s&#39;ha lliurat a l&#39;Ajuntament.
Permés detalls sobre el mecanisme d&#39; obtenció del rang de butlletes vegeu l&#39;apartat |
| //BUTLLETA/DIGIT\_CONTROL | Dígit de control de la butlleta. Vegeu l&#39;annex Càlcul del dígit de control d&#39;un número d&#39;expedient. |
| //BUTLLETA/TIPUS\_DENUNCIA | Tipus de denúncia: <ul><li>G: genèrica.</li><li>A: alcoholèmia (aquest cas requereix informar bloc de dades DENUNCIA_ALCOHOLEMIA).</li><li>V: velocitat (aquest cas requereix informar bloc de dades DENUNCIA_VELOCITAT).</li></ul> |
| //BUTLLETA/DATA\_DENUNCIA | Data de la denúncia(formatAAAAMMDD). |
| //BUTLLETA/HORA\_DENUNCIA | Hora de lad enúncia(format HHMISS). |
| //BUTLLETA/IDIOMA\_DENUNCIA | C:català/E:castellà |
| //BUTLLETA/NORMA\_DENUNCIA | Norma infringida(del catàleg de normatives). |
| //BUTLLETA/ARTICLE\_DENUNCIA | Codi de l&#39;article de la infracció(del catàleg de normatives). |
| //BUTLLETA/SUBARTICLE\_DENUNCIA | Codidesubarticlede la infracció(del catàleg de normatives). |
| //BUTLLETA/OPCIO\_DENUNCIA | Número d&#39;opció(del catàleg de normatives). |
| //BUTLLETA/PUNTS | Punts(del catàleg de normatives). |
| //BUTLLETA/MESOS\_RETIRADA | Suspensió(del catàleg de normatives). |
| //BUTLLETA/NUM\_DENUNCIANT | Número de l&#39;agent denunciant. |
| //BUTLLETA/NUM\_NOTIFICADOR | Número de l&#39;agent denunciant. |

| _Element_ | _Descripció_ |
| --- | --- |
| //BUTLLETA/TIPUS\_VIA | <ul>**La via identifica el lloc on s’ha comés la infracció, pot ser de dos tipus:**<li>V: Via interurbana (carretera)</li><li>C: Via urbana (carrer)<br></li>**Quan la via és interurbana s’ha d’indicar:**<li>A l’element VIA, el codi de carretera (p.e. C-33).</li><li>A l’element VIA_NUMERO, el punt quilomètric de la denúncia (format fins a dos dígits decimals i separador coma, p.e. 11,20).<br></li>**Quan la via és urbana s’ha d’indicar:**<li>A l’element VIA, el nom del carrer.</li><li>A l’element VIA_NUMERO, el número de carrer de la denúncia (valor sencer sense part decimal).</li></ul>|
| //BUTLLETA/VIA | Via de la denúncia. |
| //BUTLLETA/VIA\_NUMERO | Número de la via. |
| //BUTLLETA/DESCRIPCIO\_POBLACIO | Descripció de la població. |
| //BUTLLETA/DIRECCIO\_DENUNCIA | Sentit de la denúncia. |
| //BUTLLETA/NUM\_AGENT\_COBRADOR | Número de l&#39;agent cobrador. |
| //BUTLLETA/IMPORT\_NOMINAL | Import de la quantia de la denúncia amb dos decimals.10 primeres posicions és la part sencera (amb zeros al&#39;esquerra) i les dues darreres la part decimal.<ul><li>Per exemple,100:000000010000.</li></ul>|
| //BUTLLETA/IMPORT\_PAGAT | Import pagat de la butlleta amb dos decimals.Format idèntica IMPORT\_NOMINAL. |
| //BUTLLETA/IMPORT\_DESCOMPTE | Import de la denúncia amb descompte amb dos decimals. Format idèntica IMPORT\_NOMINAL. |
| //BUTLLETA/DATA\_LIMIT\_DESCOMPTE | Data límit del descompte(format AAAAMMDD).20 dies des de la data de la infracció segons llei actual. |
| //BUTLLETA/IND\_NOTIFICACIO | Indicador de signatura denunciat i notificació:<ul><li>X:no notificada.</li><li>S:notificada.</li></ul>|
| //BUTLLETA/M\_NO\_NOTIF | Motiu no notificació(obligatori quan és una denúncia no notificada): |

   
| _Element_ | _Descripció_ |
| --- | --- |
| |<ul><li>0001:Conductor absent</li><li>0002:Circular en sentit contrari</li><li>0003:Assistència en accident</li><li>0004:Servei d&#39;urgència</li><li>0005:Regulació de trànsit</li><li>0006:Prova esportiva/recreativa</li><li>0007:Transport especial</li><li>0008:Desobediència agent</li><li>0009:Helicòpter</li><li>0010:Inexistèncialloperaturar infractor</li><li>0011:Servei a peu</li><li>0012:Formular una altra denúncia</li><li>0013:Perill per a la circulació</li><li>0014:Responsabilitat titular</li><li>0015:Completar dades</li><li>0017:Seguretat per a la resta d&#39;usuaris</li><li>0018:Requalificació de denuncia</li><li>0019:Imatge captada automàticament</li><li>0020:Atestats</li><li>0021:Vehicle no logotipat</li><li>0022:Denúncia voluntària</li><li>0023:Manca resultat analítica</li><li>0024:Control estupefaents/RESIDENT</li><li>0025:Denúncia incoada d&#39;ofici</li><li>0026:Manca resultat analítica</li></ul>|
| //BUTLLETA/DATA\_HORA\_ANULLACIO | Data i hora de l&#39;anul·lació(formatAAAA-MM-DDHH:MI:SS). Obligatori quan és una denúncia anul·lada. |
| //BUTLLETA/MOTIU\_ANULLACIO | Motiu anul·lació.Obligatori quan és una denúncia anul·lada: <ul><li>0001:Fet no sancionable</li><li>0002:Denúncia duplicada</li><li>0003:Butlleta anul·lada per l&#39;agent</li><li>0004:Butlleta deteriorada</li><li>0005:Manca de competència per sancionar</li><li>0006:Retard en la tramesa denúncia al SCT</li><li>0007:Error/Manca dades bàsiques denúncia</li><li>0008:Error o manca de DNI</li><li>0009:Denúncies de Transports</li><li>0010:Fotos no vàlides</li><li>0011:Anul·lada canvi adscripció territorial</li><li>0012:Butlleta anul·lada per reposició models</li><li>0013:Butlleta anul·lada per docència</li><li>0014:Butlleta anul·lada a petició SCT</li><li>0015:SENSE ESPECIFICAR</li><li>0016:Butlleta no utilitzada</li><li>0017:Trasllat de l&#39;expedient</li><li>0018:Prescripció de la infracció</li><li>0021:Fet no sancionable</li><li>0024:Trasllat de l&#39;expedient</li><li>0025:Prescripció de la infracció</li><li>0026: Error o manca en les dades bàsiques</li><li>0027: Error o manca de DNI</li><li>0028: Fotos no vàlides</li><li>0029: Butlletes anul·lades d'ofici</li><li>0030: Manca certificat</li><li>0031: Manca competència sancionar</li><li>0032: PDA - Critica No notificada</li><li>0032: PDA - Critica No notificada</li><li>0033: Resultat negatiu</li><li>0034: Butlleta no utilitzada</li><li>0035: Baixa per error en l'aplicació PDA</li></ul>| //BUTLLETA/IND\_DGT\_VEHICLE |<ul><li>S: dades del vehicle obtingudes de consulta a la base de dades de la DGT</li><li>N: introduïdes manualment per l'agent</li></ul>| 
| //BUTLLETA/DESCRIPCIO\_MARCA | Marca. |
| //BUTLLETA/DESCRIPCIO\_MODEL | Model. |
| //BUTLLETA/COLOR\_VEHICLE | Color. |
| //BUTLLETA/TIPUS\_VEHICLE | <ul><li>00:Sense valor</li><li>01:Bicicleta</li><li>02:Ciclomotor</li><li>03:Motocicleta</li><li>04:Turisme</li><li>05:Furgoneta</li><li>06:Remolc</li><li>07:Camió</li><li>08:Autobús</li><li>09:Altres vehicles</li><li>0:Temporals</li><li>11:Turístiques</li><li>12:Vehicle especial</li><li>23:Motocicleta estrangera</li><li>24:Turisme estranger</li><li>25:Furgoneta estrangera</li><li>27:Camió estranger</li><li>28:Autobús estranger</li><li>29:Vehicle genèric</li><li>30:Vehicle especial</li></ul>|
| //BUTLLETA/MATRICULA\_DENUNCIA | Número de la matrícula. |
| //BUTLLETA/NUMERO\_BASTIDOR | Número de bastidor. |
| //BUTLLETA/DESCRIPCIO\_PAIS | Descripció del país.ESPANYA en cas d&#39;Espanya.<ul>En cas de descripció ESPANYA actualment el SCT no valida el format de la matrícula pels vehicles espanyols però es recomana ajustar-se als formats proposats en l’apartat 3.1.3.2 si en un futur el SCT realitza la validació.</ul>|
| //BUTLLETA/IND\_IMMOBILITZACIO\_VEH | Indicador d'immobilització del vehicle (S/ N). |
| //BUTLLETA/NUM\_ACTA\_IMMOBILITZACIO | Número d’acta d’immobilització. Obligatori si IND_IMMOBILITZACIO_VEH = S. |
| //BUTLLETA/NOM\_TITULAR | Nom del titular. |
| //BUTLLETA/COGNOM1\_TITULAR | Primer cognom del titular. |
| //BUTLLETA/COGNOM2\_TITULAR | Segon cognom del titular. |
| //BUTLLETA/NIF\_TITULAR | Número de document identificador del titular / propietari del vehicle (NIF, NIE, CIF, passaport o altres documents). |
| //BUTLLETA/ADRECA\_TITULAR | Adreça del titular. |
| //BUTLLETA/POBLACIO\_TITULAR | Població del titular. |
| //BUTLLETA/PROVINCIA\_TITULAR | Província del titular. |
| //BUTLLETA/CODI\_POSTAL\_TITULAR | Codi postal del titular. |
| //BUTLLETA/FET\_DENUNCIAT | Descripció del fet denunciat. Text lliure, si bé pot portar la informació del catàleg de normatives. |
| //BUTLLETA/FET\_DENUNCIAT2 | Descripció del fet denunciat.Text lliure addicional. |
| //BUTLLETA/IND\_DGT\_CONDUCTOR |<ul><li>S: dades del conductor obtingudes de consulta a la base de dades de la DGT</li><li>N: Introduïdes manualment per l'agent</li></ul>|
| //BUTLLETA/INDICADOR\_CONDUCTOR |<ul><li>I: infractor no conductor</li><li>C: conductor</li></ul>|
| //BUTLLETA/TIPUS\_DOCUMENT | <ul><li>01:Persona física</li><li>02:Persona jurídica</li><li>03:Estranger resident</li><li>04:Estranger no resident</li><li>05:Sense identificar</li></ul>|
| //BUTLLETA/NUMERO\_DOCUMENT | Número de document. |
| //BUTLLETA/TIPUS\_PERMIS | Classe permís de conduir(A,B,etc.). |
| //BUTLLETA/DATA\_EXPEDICIO | Data d&#39;expedició(formatAAAAMMDD). |
| //BUTLLETA/NOM\_CONDUCTOR | Nomd el conductor. |
| //BUTLLETA/COGNOM1\_CONDUCTOR | Primer cognom del conductor. |
| //BUTLLETA/COGNOM2\_CONDUCTOR | Segon cognom del conductor. |
| //BUTLLETA/DATA\_NAIXEMENT\_CONDUCTOR | Data de naixement(formatAAAAMMDD). |
| //BUTLLETA/DESCRIPCIO\_PAIS\_CONDUCTOR | Nacionalitat del conductor. |
| //BUTLLETA/DOMICILI\_CONDUCTOR | Domicili del conductor. |
| //BUTLLETA/DESC\_POBLACIO\_CONDUCTOR | Població del domicili del conductor. |
| //BUTLLETA/CODI\_POSTAL\_CONDUCTOR | Codi postal del conductor. |
| //BUTLLETA/OBSERVACIONS | Observacions. |
| //BUTLLETA/IMATGE | Butlleta codificada en base 64(màxim400K). |
| //BUTLLETA/TIPUS\_IMATGE | Format de la butlleta(jpg/pdf). |
| //BUTLLETA/DENUNCIA\_VELOCITAT |Bloc de dades corresponents a les dades de la denúncia en cas de TIPUS_DENUNCIA = V. Per més detalls vegeu l’apartat 3.1.2.2. |
| //BUTLLETA/DENUNCIA\_ALCOHOLEMIA | Bloc de dades corresponents a les dades de la denúncia en cas de TIPUS_DENUNCIA = A. Per més detalls vegeu l’apartat 3.1.2.3. |
| //BUTLLETA/TARGETA | Bloc de dades corresponents a les dades de pagament. Per més detalls vegeu l’apartat 3.1.2.4. |

  
#### 3.1.2.2 Denúncia velocitat

![image](https://user-images.githubusercontent.com/32306731/137351071-29a3ee46-a4b0-4a79-9251-231123277d7a.png) S’ha previst aquest bloc de dades pel futur enviament de les denúncies de velocitat. Mentrestant, no s’ha d’informar el bloc de dades DENUNCIA_VELOCITAT (només es suporta TIPUS_DENUNCIA = G). 


| _Element_ | _Descripció_ |
| --- | --- |
   | //DENUNCIA\_VELOCITAT/VELOCITAT\_REGISTRADA | Km/h. <ul><li>3 primers dígits: part sencera, zeros a l’esquerra</li><li>3 darrers dígits: part decimal, zeros a la dreta
Per exemple, 142 Km/h: 142000.</li></ul> |
| //DENUNCIA\_VELOCITAT/VELOCITAT\_LIMIT | Km/h. 4 dígits sencers, zeros a l’esquerra. Per exemple, 120 Km/h: 0120. |
| //DENUNCIA\_VELOCITAT/TIPUS\_LIMITACIO | <lu><li>U: Limitació en via urbana</li><li>I: Limitació en via interurbana</li><li>F: Limitació fixada per senyal</li></lu>|
| //DENUNCIA\_VELOCITAT/CINEMOMETRE | Cinemòmetre. |
| //BUTLLETA/CODI\_ANTENA | Antena. |

     
#### 3.1.2.3 Denúncia alcoholèmia

![image](https://user-images.githubusercontent.com/32306731/137351209-bc3df005-8413-41d4-8bde-b85436c0a80f.png) S’ha previst aquest bloc de dades pel futur enviament de les denúncies d’alcoholèmia. Mentrestant, no s’ha d’informar el bloc de dades DENUNCIA_ALCOHOLEMIA (només es suporta TIPUS_DENUNCIA = G). |


| _Element_ | _Descripció_ |
| --- | --- |
| //DENUNCIA\_ALCOHOLEMIA/TIPUS\_LECTURA | <lu><li>A: Aire espirat</li><li>S: Contingut en sang</li></lu> |
   | //DENUNCIA\_ALCOHOLEMIA/PCT\_1\_LECTURA | Taxa alcoholèmia 1a prova. <lu><li>3 primers dígits: part sencera, zeros a l’esquerra</li><li>3 darrers dígits: part decimal, zeros a la dreta. Per exemple, 0,39: 000390.</li></lu>|
| //DENUNCIA\_ALCOHOLEMIA/DATA\_1\_LECTURA | Data 1a prova(format AAAAMMDD). |
| //DENUNCIA\_ALCOHOLEMIA/HORA\_1\_LECTURA | Hora 1a prova(formatHHMISS). |
| //DENUNCIA\_ALCOHOLEMIA/PCT\_2\_LECTURA | Taxa alcoholèmia 2a prova. Format idèntic a PCT\_1\_LECTURA. |
| //DENUNCIA\_ALCOHOLEMIA/DATA\_2\_LECTURA | Data 2a prova(format AAAAMMDD). |
| //DENUNCIA\_ALCOHOLEMIA/HORA\_2\_LECTURA | Hora 2a prova(formatHHMISS). |
| //DENUNCIA\_ALCOHOLEMIA/MODEL\_ETILOMETRE | <lu><li>01:Dräger Alcotest 7110</li><li>02:Lion 1400LS</li><li>03:Dräger Alcotest 7410</li><li>04:Alcotest7110-E</li></lu>|
| //DENUNCIA\_ALCOHOLEMIA/NUMERO\_ETILOMETRE | Número de sèrie de l&#39;etilòmetre. |

    
#### 3.1.2.4 Targeta

![image](https://user-images.githubusercontent.com/32306731/137351411-4736303c-be46-4f3e-aefc-d9b666ad8e56.png) S&#39;ha previs taquest bloc de dades per un possible ús futur del pagament amb targeta. Mentrestant, el valor de l&#39;element IND\_TARGETA serà sempre N. |


| _Element_ | _Descripció_ |
| --- | --- |
| //TARGETA/IND\_TARGETA | <lu><li>S: pagament amb targeta</li><li>N: sense targeta, no implica que s'hagi fet el pagament en metàl·lic</li></lu>|
| //TARGETA/NUMERO\_TARGETA | 4últims dígits de la targeta. |
| //TARGETA/DATA\_CADUCITAT\_TARGETA | Data de caducitat de la targeta(format AAAAMMDD). |
| //TARGETA/NUMERO\_TERMINAL | Número de terminal. |
| //TARGETA/CODI\_COMERC | Codi de comerç. |
| //TARGETA/NUMERO\_COMANDA | Número de comanda. |
| //TARGETA/NUMERO\_AUTORITZACIO | Número d&#39;autorització. |
| //TARGETA/DATA\_JUSTIFICANT | Data del justificant(formatAAAAMMDD). |
| //TARGETA/HORA\_JUSTIFICANT | Hora del justificant(formatHHMISS). |

### 3.1.3 Resposta especiífica <a name="3.1.3"></a>

![image](https://user-images.githubusercontent.com/32306731/137351916-64a9c7ad-166c-4db5-87ff-5e98d5935a5a.png)

| _Element_ | _Descripció_ |
| --- | --- |
| /respostaEnviamentRemesa/DADES | Bloc de dades corresponent al detall de l&#39;enviament de la remesa. |
| //DADES/CAPCALERA/FITXER\_PDA | Nom de fitxer de la remesa. Referència de la remesa. |
| //DADES/CAPCALERA/NUM\_BUTLLETES | Númerode butlletes que conté la remesa. |
| //DADES/CAPCALERA/CODI\_INCIDENCIA | Codi de la incidència. |
| //DADES/CAPCALERA/DESCRIPCIO\_INCIDENCIA | Descripció de la incidència |
| //DADES/BUTLLETES/BUTLLETA | Bloc de dades corresponent al resultat del tractament d&#39;una butlleta. |
| //DADES/BUTLLETES/BUTLLETA/ID\_BUTLLETA | Identificador de la butlleta en el sistema origeno |
| | identificador dins de la remesa. |
| //DADES/BUTLLETES/BUTLLETA/SERVEI\_TERRIT | Servei territorial. |
| //DADES/BUTLLETES/BUTLLETA/NUM\_EXPEDIENT | Número d&#39;expedient. |
| //DADES/BUTLLETES/BUTLLETA/RESULTAT |<lu><li>0: tractament correcte</li><li>1: conté butlletes amb errors no crítics</li><li>2: conté butlletes amb errors crítics</li><li>Quan s&#39;intenta carregar una butlleta, si no supera les validacions del sistema del SCT queda marcada com a crítica i no es tramita. Per tal de tramitar-la cal reenviar-la amb l&#39;error crític corregit.</li></lu> |
| //DADES/BUTLLETES/BUTLLETA/DATA\_LIMIT | Per a butlletes crítiques, data màxima per a reenviar-la amb les dades corregides(formatAAAAMMDD). |
| //DADES/BUTLLETES/BUTLLETA/INCIDENCIES | Bloc de dades corresponent a la relació d&#39;incidències detectades en la butlleta. |
| //INCIDENCIES/INCIDENCIA | Bloc de dades corresponent a una incidència detectada en la butlleta. |
| //INCIDENCIA/CODI\_INCIDENCIA | Codi de incidència. Vegeu apartat 3.1.3.1 d’aquest document. |
| //INCIDENCIA/DESCRIPCIO\_INCIDENCIA | Descripció d&#39;incidència. |
| //INCIDENCIA/INFORMACIO\_ADDICIONAL | Informació addicional de la incidència. | 
| /respostaEnviamentRemesa/resultat/codiResultat | <lu><li>0: enviament de la remesa realitzat correctament (detalls de cada butlleta a l’element DADES).</li><li>0502: error realitzant l’enviament.</li></lu> |
| /respostaEnviamentRemesa/resultat/descripcio | Descripció del resultat. |

      
#### 3.1.3.1 Codis d’incidència

| _Tipus_ | _Codi_ | _Descripció_ | _Campsvalidats_ | _Validació_ |
| --- | --- | --- | --- | --- |
| **Capçalera** | **Capçalera** | **Capçalera** | **Capçalera** | **Capçalera** |
| CRÍTIC | CAP002 | Nombre de denúncies erroni | NUM\_BUTLLETES | Nombre de BUTLLETA de la remesa no coincideix amb el NUM_BUTLLETES. |
| **Validacions de format** | **Validacions de format** | **Validacions de format** | **Validacions de format** | **Validacions de format** |
   | CRÍTIC | INT001 | Dada numèricaerrònia | <ul><li>IMPORT_NOMINAL</li><li>IMPORT_PAGAT</li><li>IMPORT_DESCOMPTE</li></ul> | El valor no es pot convertir a número, aplicant el patró corresponent. |
| NO CRÍTICA | INT002 | Dada numèrica errònia | VIA\_NUMERO | El valor no es pot convertir a número, aplicant el patró corresponent.|
| CRÍTIC | DAT001 | Data errònia | DATA\_DENUNCIA | Format incorrecte AAAAMMDD.. |
| NO CRÍTICA | DAT002 | Data errònia | DATA\_LIMIT\_DESCOMPTE | Format incorrecte AAAAMMDD. |
| CRÍTIC | DAT003 | Data errònia | DATA\_HORA\_ANULLACIO | El valor no es pot convertir a una data segons el format: YYYY-MM-DD-HH:MI:SS&#39;. |
| CRÍTIC | HOR001 | Hora errònia | HORA\_DENUNCIA | El valor no es pot convertir a una hora segons el format HHMISS. |
| **Validacionsdecontingut** | **Validacionsdecontingut** | **Validacionsdecontingut** | **Validacionsdecontingut** | **Validacionsdecontingut** |
| CRÍTIC | BUT002 | Butlleta duplicada | SERVEI\_TERRITNUM\_EXPEDIENT |Aquesta butlleta ja existeix al sistema (Només s’admet una butlleta repetida quan es tracta del reenviament de una butlleta crítica). Una butlleta només és pot enviar una vegada, amb ACCIO:<ul><li>A: Alta.</li><li>B: Anul·lació.</li></ul>|
| CRÍTIC | BUT003 | Butlleta fora de rang | SERVEI\_TERRITNUM\_EXPEDIENT | La butlleta no correspon al rang assignat a la policia local corresponent. |
| CRÍTIC | BUT004 | Element obligatori NO informat | <ul><li>ACCIO</li><li>NORMA_DENUNCIA</li><li>ARTICLE_DENUNCIA</li><li>SUBARTICLE_DENUNCIA</li><li>OPCIO_DENUNCIA</li<li>NUM_DENUNCIANT</li><li>TIPUS_VIA</li><li>VIA</li><li>VIA_NUMERO</li><li>IMPORT_NOMINAL</li><li>IMPORT_PAGAT</li><li>IMPORT_DESCOMPTE</li><li>DATA_LIMIT_DESCOMPTE</li><li>IND_NOTIFICACIO</li></ul>|-|
| CRÍTIC | BUT004 | Element obligatori NO informat | INDICADOR\_CONDUCTORTIPUS\_DOCUMENTNUMERO\_DOCUMENTNOM\_CONDUCTORCOGNOM1\_CONDUCTORCODI\_PAIS\_CONDUCTORDOMICILI\_CONDUCTORCODI\_MUNICIPI\_CONDUCTOR | Aquests elements només són obligatoris per a denúncies notificades. |
| CRÍTIC | BUT007 | Codi control butlleta erroni | DIGIT\_CONTROL |
| NO CRÍTICA | BUT011 | Camp massa llarg | | <ul>Són camps que en l’entorn PDA són més grans que en entorn PSN. Elements la grandària dels quals és més gran a la missatgeria de la remesa que als sistemes del SCT. </ul>|
| NO CRÍTICA | BUT013 | Manca codipostal | CODI\_POSTAL\_CONDUCTOR | El codi postal del conductor no estàinformat. |
| CRÍTIC | BUT014 | Butlleta arxivada | SERVEI\_TERRITNUM\_EXPEDIENT | S&#39;ha tornat a enviar una butlleta que ja s&#39;havia arxivat perquè s&#39;havia rebutcom a CRÍTICA amb errors CRÍTICS i s&#39;ha exhaurit el termini per reenviar-la. |
| NO CRÍTICA | BUT015 | Butlleta reenviada | SERVEI\_TERRITNUM\_EXPEDIENT | Les butlletes reenviades generaran una incidència no crítica per forçar que passin pel mòdul de validació de PSN, malgrat que no hi hagi cap altra incidència en la butlleta. |
| CRÍTIC | BUT018 | Butlleta pagada no notificada | IMPORT\_PAGATIND\_NOTIFICACIO | Quanl&#39;elementIND\_NOTIFICACIOsigui &quot;X&quot; (butlleta no notificada),l&#39;import pagat ha de ser 0, si no és així es genera error. |
| CRÍTIC | NOR001 | Normativainexistent | NORMA\_DENUNCIAARTICLE\_DENUNCIASUBARTICLE\_DENUNCIAOPCIO\_DENUNCIA | No existeix al catàleg de normativa legal. |
| NOCRÍTICA | NOR002 | Normativa NOvigent | NORMA\_DENUNCIAARTICLE\_DENUNCIASUBARTICLE\_DENUNCIAOPCIO\_DENUNCIA | Existeix al catàleg de normativa legal però no és vigent respecte a la data de la denúncia. |
| NO CRÍTICA | NOR003 | Import de la quantia de la denúncia incorrecte | IMPORT\_NOMINAL | Import incorrecte per la normativa infringida. |
| NO CRÍTICA | NOR004 | Nombre de punts adetreure incorrecte | PUNTS | Punts incorrectes per la normativa infringida. |
| CRÍTIC | NOT001 | Camp obligatori NO notificació NO informat | M\_NO\_NOTIF | Quanl&#39;elementIND\_NOTIFICACIOsigui &quot;X&quot; (butlleta no notificada), el motiu és obligatori. |
| CRÍTIC | ANU001 | Camp obligatori anul·lació NO informat | DATA\_HORA\_ANULLACIOMOTIU\_ANULLACIO | És obligatori quan l&#39;element ACCIO és &quot;B&quot; (anul·lació). |
| CRÍTIC | MAT001 | Format matricula errònia | MATRICULA\_DENUNCIA | Actualment no es valida el format de la matrícula pels vehicles espanyols(DESCRIPCIO\_PAIS=ESPANYA)tot i que en un futur el SCT podria validar-lo. |

  
#### 3.1.3.2 Formats de matrícula

Formats de matrícula recomanats pel SCT:

- Turisme/Motocicleta/Camió/Furgoneta/Autobús(on n dígits, L lletres, b espais).

*nnnnLLL LLnnnnLL LLnnnnbL LLnnnnnn LbnnnnLL LbnnnnbL Lbnnnnnn*

- Remolc(on n dígits i L lletres):

*RnnnnLLL*

- VehiclesEspecials(onnnúmeros,Llletres, Efixa,bespais):

*EnnnnLLL LLnnnnnE LbnnnnnE LLnnnnnnE LbnnnnnnE*

- Turístics(onnnúmeros,Llletres,Tfixa,bespais):

*TnnnnLLL nnLLnnnn nnLbnnnn*

- Temporals(on n números, L lletres, T, R, P, S, V fixes, b espais):

*LLnnnnTnn LbnnnnTnn LLnnnnRnn LbnnnnRnn PnnnnLLL SnnnnLLL nnnnLLL*

- Ciclomotors(onnnúmeros,Llletres,Cfixa):

*CnnnnLLL*

- Altres Vehicles(o n nnúmeros,L lletres, CD, OI, CC, TA, H, PMM,PGC fixes,b espais):

| CDnnnn | OInnnn | CCnnnn | TAnnnn | HnnnnLLL | PMMnnnnnn |
| --- | --- | --- | --- | --- | --- |
| PMMnnnnLL | PMMnnnnbL | PGCnnnnnn | PGCnnnnLL | PGCnnnnbL |
 

### 3.2 Obtenció de rang de butlletes (SCT_BUTLLETES) <a name="3.2"></a>

Les policies locals necessiten disposar de butlletes del SCT per poder interposar denúncies competència del SCT. Actualment disposen de talonaris de butlletes en paper lliurats per el SCT que tenen preimprès el número de butlleta(expedient).

Per aquelles policies que s&#39;acullin al sistema de denúncies via PDA ofereix aquesta modalitat per obtenir rangs de butlletes.

Les denúncies competència del SCT han d'estar identificades per:

- SERVEI\_TERRIT:demarcacióterritorial.

- NUM\_EXPEDIENT:no és un valor aleatori,és un número de butlleta del rang de números que es lliura a la policial local corresponent.

Aquesta informació ha de mostrar-se en les butlletes que la policia local notifica als infractors. També en aquelles no notificades.

    
### 3.2.1 Petició–dades específiques <a name="3.2.1"></a>

![image](https://user-images.githubusercontent.com/32306731/137328706-df5c9ce6-fa3b-4350-9042-07e343fca52c.png)

| _Element_ | _Descripció_ |
| --- | --- |
| /resposta Obtencio Rang Butlletes/NUM\_BUTLLETES\_SOLICITADES | Número de butlletes sol·licitades(màxim9999). |

    
### 3.2.2 Resposta –dades específiques <a name="3.2.2"></a>

![image](https://user-images.githubusercontent.com/32306731/137328536-ce4cb9ec-160f-475e-a6a7-a4d8cbf4384d.png)


| _Element_ | _Descripció_ |
| --- | --- |
   | /respostaObtencioRangButlletes/SORTIDA/RESULTAT | <ul><li>0: s&#39;han lliurat les butlletes sol·licitades.</li><li>1: s&#39;han lliurat menys butlletes de les sol·licitades.</li><li>2: no es disposa de butlletes.En cas que s&#39;obtingui aquest error contacteu amb el SCT.</li></ul>|
| /respostaObtencioRangButlletes/SORTIDA/NUM\_BUTLLETES\_LLIURADES | Número de butlletes lliurades. |
| /respostaObtencioRangButlletes/SORTIDA/RANGS/RANG | Bloc de dades corresponent a un rang de butlletes.El sistema del SCT pot lliurar el nombre de butlletes en rangs discontinus. |
| //RANG/ID\_RANG | Identificador del rang. |
| //RANG/SERVEI\_TERRIT | Demarcació territorial. |
| //RANG/TALONARI\_INICIAL | Valor inicial del rang.Número de butlleta inicial. |
| //RANG/TALONARI\_FINAL | Valor final del rang.Número de butlleta final. |
| /respostaObtencioRangButlletes/resultat/codiResultat | <ul><li>0: consulta realitzada correctament.</li><li>- 0502: error realitzant la consulta.</li></ul>|
| /respostaObtencioRangButlletes | Descripció del resultat. |
| /resultat/descripcio | |

   
## 3.3 Descàrrega del catàleg de normatives(SCT\_CATALEG) <a name="3.3"></a>

Les policies locals necessiten disposar del catàleg de normatives per poder indicar la norma infringida quan interposen una denúncia competència del SCT.
   
Actualment disposen d'un catàleg d'infraccions imprès en format de mini llibre, quan realitzen una denúncia escriuen manualment a la butlleta les dades identificatives de la opció normativa infringida.
   
Per aquelles policies que s'acullin al sistema de denúncies via PDA, s'ofereix una modalitat per obtenir el catàleg de normatives en format digital.

   
### 3.3.1 Petició–dades específiques <a name="3.3.1"></a>

La modalitat no requereix cap tipus de dades específiques a l’hora de realitzar la petició.

   
### 3.3.2 Resposta –dades específiques <a name="3.3.2"></a>

| _Element_ | _Descripció_ |
| --- | --- |
| /respostaConsultaCatalegNormatives/NORMATIVES/NORMATIVA | Bloc de dades corresponent al bloc de dades de cadascuna de les normatives. |
| //NORMATIVA/CODI\_NL | Codi de normal legal. |
| //NORMATIVA/ARTICLE | Article infringit. |
| //NORMATIVA/SUBARTICLE | Subarticle infringit. |
| //NORMATIVA/OPCIO | Opció. |
| //NORMATIVA/GRAVETAT | <ul><li>G:Greu</li><li>L:Lleu</li><li>M:Moltgreu</li><li>V:62.2LSV</li><li>W:67.4LSV</li></ul> |
| //NORMATIVA/DESC\_BREU | Descripció breu del concepte de la infracció. |
| //NORMATIVA/FET\_DENUNCIAT | Concepte de la infracció(català). |
| //NORMATIVA/HECHO\_DENUNCIADO | Concepte de la infracció(castellà). |
| //NORMATIVA/FET\_DENUNCIAT\_ARANES | Concepte de la infracció(aranès). |
| //NORMATIVA/IMPORT\_NOMINAL | Quantia de la multa sense descompte amb dos decimals. 10 primeres posicions són la part sencera(amb zeros a l&#39;esquerra) i les dues darreres la part decimal.<ul>Per exemple,100:000000010000.</ul> |
| //NORMATIVA/PUNTS | Pèrdua de punts que comporta la infracció. |
| /respostaConsultaCatalegNormatives/resultat/codiResultat | <ul><li>0: consulta realitzada correctament.</li><li>0502: error realitzant la consulta.</li></ul>|
| /respostaConsultaCatalegNormatives/resultat/descripcio | Descripció del resultat. |

![image](https://user-images.githubusercontent.com/32306731/137328964-69421ef9-e63d-46a0-9f7c-9c24723aac9e.png)

# 4. Annexes <a name="4"></a>

## 4.1 Càlcul del dígit de control d’un número d’expedient <a name="4.1"></a>

Donat un número d’expedient, el dígit de control corresponent es calcula tal i com es descriu a continuació:

1. Obtenir l’emissora a la que pertany l’expedient.

| _Serveiterritorial_ | _Emissora_ |
| --- | --- |
| Barcelona (08) | 8500514 |
   | Girona(17) | <ul><li>Si número expedient entre 1 i 40000 → 8500459</li><li>Si número expedient entre 40001 i 112500 → 8500459</li><li>Si número expedient >= 112501 → 8500472</li></ul>|
| Lleida(25) | 8500484 |
| Tarragona(43) | 8500540 |

2. Si l’emissora és 8500459 el dígit de control és 9.

3. Altrament, per calcular el dígit de control s’ha de diferenciar si l’expedient és de 7 o 8 dígits (número d’expedient > 9999999).
   
Concatenar el servei territorial i el número d’expedient
   
• Expedient de 7 o menys dígits: formatant amb zeros per l’esquerra fins a 7 dígits.
• Expedient de 8 dígits: no cal formatar per l’esquerra.

i a aquest sumar-li el número de l’emissora i invertir el resultat.

![image](https://user-images.githubusercontent.com/32306731/137354001-c03a7535-ffd6-45ac-8dde-fec450064212.png)

Invertint-lo:

![image](https://user-images.githubusercontent.com/32306731/137354091-5df64abb-c438-496b-ac61-515df22cbee7.png)

4. Multiplicar cada dígit del resultat obtingut pel valor que es troba en la mateixa posició en el següent array (constant).

| 2 | 3 | 4 | 5 | 6 | 7 |8 | 9 | 2 | 3 | 4 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

(9\*2)+(2\*3)+(0\*4)+(9\*5)+(6\*6)+(6\*7)+(3\*8)+(9\*9)=252

![image](https://user-images.githubusercontent.com/32306731/137354261-e3c00e71-29d5-43ac-b6ef-498c1b285b43.png)


5. Dividir el valor obtingut entre 11 i quedar-se amb el residu.

![image](https://user-images.githubusercontent.com/32306731/137285147-baec349f-44c0-48b0-9e52-0fe1a68f62ac.png)

6. Si el residu és 10, el dígit de control serà 0. En qualsevol altre cas serà el valor obtingut.

Així,el número d&#39;expedient complert serà 08/5168515-0.

## 4.2 Línia de cobrament i codi de barres d&#39;una butlleta de sanció <a name="4.2"></a>

A continuació es detalla com generar la línia de cobrament i codi de barres que cal incorporar a les
butlletes.

![image](https://user-images.githubusercontent.com/32306731/137284289-9ea74e0c-27bb-445b-8600-7d695a6b3939.png)

### 4.3 Línia de cobrament <a name="4.3"></a>

Per composar la línia de cobrament:

![image](https://user-images.githubusercontent.com/32306731/137283999-406e86ce-78f5-4bc3-a06e-01e0011231ec.png)

- RIN F5 Emissora: valor fix 05850043.

- Sufix (XXX): en funció del Servei Territorial.

![image](https://user-images.githubusercontent.com/32306731/137283786-f7248b53-a16d-4a76-9494-3847164f2a98.png)

| _ServeiTerritorial_ |
| --- |
| _Barcelona(91)_ | _Girona(92)_ | _Lleida(93)_ | _Tarragona(94)_ |
| 608 | 617 | 625 | 643 |

- Referència (SSSSYYYYYYY-CC / SSSYYYYYYYY-CC) consta de tres parts:

  - Servei Territorial (SSSS, 4 posicions i 3 posicions per expedients de 8 dígits):
    - **Barcelona**
      - 0008: per expedients de menys de 8 posicions
      - 008: per expedients de 8 posicions
    - **Girona**
      - 0017: per expedients de menys de 8posicions
      - 017: per expedients de 8 posicions
    - **Lleida**
      - 0025: per expedients de menys de 8 posicions
      - 025: per expedients de 8 posicions
    - **Tarragona**
      - 0043: per expedients de menys de 8 posicions
      - 043: per expedients de 8 posicions

  - Número d’expedient (YYYYYYY o YYYYYYYY- 8 posicions)

  - Dígits de control (CC, 2 posicions) calculats segons l’algoritme inclòs a l’Annex 2 del Quadern 57 &quot;_Cobros por __ventanilla__ y__autoservicio_&quot;.

Depenent de si l’expedient es 8 o menys posicions el format de la referència es:

**‘00’ (2 posicions)** + SERVEI_TERRITORIAL (2 posicions) + **NUM_EXPEDIENT (7 posicions)** + DC (2 posicions)

**‘0’ (1 posició)** + SERVEI_TERRITORIAL (2 posicions) + **NUM_EXPEDIENT (8 posicions)** + DC (2 posicions)

- Data límit (DDMMAAAA): termini de 20 dies des de la data de la denúncia.

- Import amb descompte: S.C.

### 4.4 Codi de barres <a name="4.4"></a>

Pel que fa a la composició del codi de barres de 46 dígits tenim:

- Codi inicial fix (5 posicions): 90507.

- Codi d’emissora fix pel SCT (8 posicions): 05850043.

- Sufix (3 posicions) en base al Servei Territorial, vegeu apartat anterior.

- Servei Territorial (3/4 posicions), vegeu apartat anterior.

- Número d’expedient (7/8 posicions).

- Dígits de control (2 posicions) calculats segons l’algoritme inclòs a l’Annex 2 del Quadern 57 &quot;_Cobros __por ventanilla__ y__autoservicio_&quot;.

- Identificació (6 posicions): correspon a la data límit (DDMMAA), vegeu apartat anterior.

- Import (10 posicions): les 2 darreres posicions corresponen als decimals.

- Dígit final fix (1 posició): 0.

![image](https://user-images.githubusercontent.com/32306731/137281698-9dfc2044-94f7-487f-a7d6-9a4e0707feb3.png) A l’Annex 1 del Quadern 38 (“Códigos de Barras en Documentos Financieros”) podeu consultar la informació referent a la representació del codi de barres EAN-128 que inclou el
format i com es genera el caràcter de control que s’ha d’incloure.

![image](https://user-images.githubusercontent.com/32306731/137281455-fb7a0d64-621e-4337-a136-46323d877708.png)

![image](https://user-images.githubusercontent.com/32306731/137281095-b86e4677-b97f-465f-8eec-cb3bb8f26b4d.png)


## 4.5 Text dels avisos legals a incloure en les butlletes <a name="4.5"></a>

---

**INFORMACIÓ I ADVERTÈNCIES LEGALS**

Sigles  de  les  normes:  R,  Reglament  general  de  circulació  (RD  1428/03);  V,  Reglament  general  de vehicles (RD 2822/98); G, Reglament general de conductors (RD 818/09); A, Llei sobre responsabilitat civil  i  assegurança  en  la  circulació  de  vehicles  de  motor  (Rdleg.  8/04);  TRLSV,  text  refós  de  la  Llei sobre  trànsit,  circulació  de  vehicles  de  motor  i  seguretat  viària  (RDL  6/2015);  E,  Reglament d'autoescoles (RD 1295/03).

**NOTIFICACIÓ DE LA DENÚNCIA I INCOACIÓ DEL PROCEDIMENT**

Us  notifiquem  aquesta  denúncia  i  us  fem  saber  que  queda  incoat  el  corresponent  procediment sancionador (article89 TRLSV)

**PROCEDIMENT ABREUJAT I PROCEDIMENT ORDINARI**

Si us voleu acollir a la tramitació pel procediment abreujat, disposeu d'un termini de 20 dies naturals des  de  la  notificació  per  realitzar  el  pagament  de  l'import  de  la  sanció,  amb  un  descompte  del  50%. Aquest  procediment  implica:  la  renúncia  a  formular  al·legacions  (es  tindran  per  no  presentades),  la finalització  del  procediment  sense  dictar  resolució  expressa,  l'esgotament  de  la  via  administrativa (només es podràrecórrer  davant  del  Jutjat  )  i  la  fermesa  de  la  sanció  des  del  moment  del  pagament.  Si  la  sanció  és greu  i  no  comporta  detracció  de  punts,  no  s'anotarà  com  antecedent  en  el  Registre  de  conductors  i infractors.  En  cas  de  no  efectuar  el  pagament  amb  descompte  del  50%,  continua  la  tramitació  pelprocediment ordinari, que us permet formular al·legacions i proposar o aportar proves en el termini de 20 dies naturals des de lanotificació.  L'escrit  d'al·legacions  s'ha  d'adreçar  al  Servei  Territorial  de  Trànsit  corresponent  i  es  pot presentar en qualsevol dels registres legalment establerts (*).  Si no formuleu al·legacions ni aboneu l'import  de  la  sanció,  aquesta  denúncia  tindrà  efecte  d'acte  resolutori  del  procediment  sancionador. Contra  la  sanció  es  podrà  interposar  recurs  de  reposició,  amb  caràcterpotestatiu,  en  el  termini  d'un mes.  La  sanciópodrà  executar-se  transcorreguts  30  dies  naturals  des  de  la  notificació  d'aquesta denúncia.

**ÒRGANS QUE INTERVENEN EN EL PROCEDIMENT**

Òrgan  instructor:  el/la  cap  del  Servei  Territorial  de  Trànsit  de  Barcelona.  Autoritat  sancionadora:  la persona  titular  de  la  Direcció  del  Servei  Català  de  Trànsit  (article  11.1  de  la  Llei  14/1997,  de  24  de desembre, de creació del Servei Català de Trànsit, D.O.G.C. núm. 5537, de 31-12-2009)

**RESOLUCIÓ DE L'EXPEDIENT SANCIONADOR**

El  termini  màxim  per  resoldre  i  notificar  la  resolució  de  l'expedient  sancionador  iniciat  mitjançant aquesta denúncia és d'un any des de l'inici del procediment (article 112.3 TRLSV)

**PÈRDUA DE PUNTS EN ELS PERMISOS I LLICÈNCIES DE CONDUCCIÓ**

Els punts indicats a aquesta butlleta es detrauran quan la sanció sigui ferma. Podeu consultar el saldodepunts en [www.dgt.es](http://www.dgt.es/)

**NOTIFICACIONS ELECTRÒNIQUES**

La  TRLSV  ha  establert  que  a  partir  del  24-11-2010  es  puguin  efectuar  les  notificacionsper  mitjans electrònics. Si us voleu acollir a aquesta possibilitat, consulteu el web [http://transit.gencat.cat](http://transit.gencat.cat/)

**FORMA DE PAGAMENT**

Podeu utilitzar una de les següents modalitats per fer efectiu el pagament de la multa:

- Pagar-la directament amb targeta a l’agent que us ha denunciat.

- Pagar-la amb aquest imprès, per l’import i fins la data límit de pagament reflectits al peu de la butlleta de denúncia, a qualsevol de les oficines de CaixaBank.

- Pagar-la  amb targeta, directament a qualsevol de  les dependències territorials del  Servei Català de Trànsit.

- Pagar-la per internet mitjançant el portal [http://tramits.gencat.cat](http://tramits.gencat.cat/)

(*) Els subjectes **obligats a relacionar-se electrònicament** amb l’administració (art 14.2i 14.3de la Llei 39/2015 i art 3 de l’Ordre PDA/20/2019) han de presentar les al·legacions per internet accedint al web [http://transit.gencat.cat.](http://transit.gencat.cat/)

**PROTECCIÓ DE DADES.**

Tractament SCT: Expedients sancionadors per infraccions de trànsit. Responsable: la persona titular de  la  Direcció  de  l’SCT  (Diputació,  355,  08009  Barcelona).  Contacte  Delegat  de  protecció  de dades:Diputació,  355,  08009  Barcelonao dpd.interior@gencat.catFinalitat:  gestionar  les  dades relatives  als  expedients  sancionadors  que  s ́incoïn  per  infraccions  en  aquesta  matèria.  Informació addicional accediu al web de l'SCT.Tractament  Ajuntament  de  ...............:  amb  la  finalitat  de  generar  aquest  document,  les  dades personals seran tractades per l’Ajuntament de  xxxxxxxxxxxxxxxxxxxxxx   Podeu consultar informació addicional sobre aquest tractament i protecció de dades en www.xxxxxxxxxxxxxxxxxxxx

---
