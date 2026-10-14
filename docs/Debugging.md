# Debugging



## 1. Vilket fel undersökte du?

Fel 3 - Sökningen är skiftlägeskänslig.


## 2. Hur återskapade du felet?

Jag körde applikationen och visade alla bokningar för att se vilka namn som fanns sparade. Testade sedan att söka på "demin", som finns i Nackademin, och lyckades. Testade igen och sökte efter "Demin" med stort D och fick ingen träff.



## 3. Vilket resultat fick du?

När jag sökte på "Demin" så fick jag meddelandet "No results found" och hade därmed återskapat felet.



## 4. Vilket resultat skulle du ha fått?

Sökningen ska fungera oavsett om söktermen innehåller stora eller små bokstäver. Jag förväntade mig därför att få tillbaka bokningen för "Nackademin AB" när jag söker efter "Demin".



## 5. Vilken del av koden orsakade felet?

`BookingService.Search` använder sig utav `Contains` för att hitta bokningar där namnet innehåller delar eller hela söktermen. `Contains` saknade dock inställningen för att ignorera skillnader mellan stora och små bokstäver.

## 6. Varför gav koden ett felaktigt resultat?

`Contains` var skiftlägeskänslig och därför hittades bokningen bara om söktermens stora och små bokstäver matchade motsvarande del av det sparade namnet.

## 7. Vad ändrade du?

Jag lade till en `StringComparison.OrdinalIgnoreCase` som argument i anropet till `Contains`. Detta gör så att sökningen i `CustomerName` efter `searchTerm` ignorerar skillnader mellan stora och små bokstäver.



## 8. Hur kontrollerade du att ändringen fungerade?

Jag körde applikationen igen och sökte på sparade namn med olika kombinationer av stora och små bokstäver. Sökningarna gav de förväntade träffarna.



## 9. Vilket automatiserat test skyddar mot att felet återkommer?

Testet `Search_WhenSearchTermHasDifferentCasing_ReturnsBooking()` söker på "avanoria" och kontrollerar att bokningen för "Avanoria AB" returneras. Om sökningen blir skiftlägeskänslig igen misslyckas detta test vilket gör att felet kan upptäckas när testerna körs.

