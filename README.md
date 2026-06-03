# sahkohinnat

## Hintalähteen varmenneongelma

Selain ei voi kiertää ulkopuolisen API-palvelun vanhentunutta TLS-varmennetta. Jos `https://sahkotin.fi/prices` ei läpäise selaimen varmennevalidointia, `fetch` epäonnistuu ennen kuin sovelluskoodi pääsee käsittelemään vastausta.

Pysyvä korjaus kannattaa tehdä näin:

1. **Ensisijaisesti** aja omassa palvelussa samaan originin alle endpoint `/api/prices`, jolla on oman palvelusi voimassa oleva TLS-varmenne. Endpointin kannattaa palauttaa sama JSON-muoto kuin `sahkotin.fi/prices`, jolloin nykyinen selainkoodi käyttää sitä automaattisesti.
2. **Toissijaisesti** selain yrittää edelleen `sahkotin.fi`-lähdettä.
3. **Varalähteenä lyhyille hintaikkunoille** selain yrittää `porssisahko.net`-rajapintaa ja muuntaa tuntihinnat varttiriveiksi, jotta perusnäkymät eivät jää kokonaan tyhjiksi.

Kuukauden keskiarvo vaatii pitkän aikavälin dataa, joten se kannattaa palvella oman `/api/prices`-proxyn kautta. Lyhyen ikkunan varalähdettä ei käytetä kuukauden keskiarvon laskentaan.
