# Komande 

Zaista nemam zelju da previse detaljno nesto opisujem 
(i iskreno se nadam da cu ostati pri tome), cisto je ideja
da u kratkim crtama zapisem po koju stvar koju sam naucio citajuci
knjigu, ili, eto, nesto sto smatram zanimljivim.

- `$git commit -a` - dodaje samo _track_-ovane fajlove preskacuci stejdzing.
Ovo dodatno znaci da novokreirani fajlovi nece biti dodati jer nisu _track_-ovani.
Fajl se smatra _track_-ovanim u slucaju da je po nadzorom sistema kontrole verzije,
te je i logicno sto novi fajlovi ne spadaju ovde.

- `$git rm <file>` - komanda koja uklanja fajl sa gita.
Nakon ove komande i komitovanja uklonjeni fajl se vise nece nalazi ni lokalno, niti
ce ga git pratiti. Korisni flegovi su: `-f` i `--cached`. U slucaju prvog fajl ce biti
uklonjen iako je trenutno u stejdzingu. Trajno uklonjen. Zato i mora `-f`, da te spreci 
da ne zajebes stvari.
Ako izaberes drugu opciju, `--cached` onda ce samo iz stejdzinga u _working tree_ da se prebaci, odnosno nece ti ulaziti u 
naredni komit. Ovo je i te kako korisno.
Inace, kao ime fajla nije neophodno unositi samo ime konkretno ime fajla, ili foldera,
vec je moguce da to budu i neki sabloni.