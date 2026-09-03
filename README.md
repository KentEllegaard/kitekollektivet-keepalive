# Keep-alive: kitekollektivet.dk

Denne repo indeholder kun et formaal: hvert 20. minut pinger et GitHub Actions-workflow alle 74 landingssider (37 dansk + 37 engelsk) paa kitekollektivet.dk, saa WP Rocket / Flywheels cache aldrig naar at gaa helt koldt mellem rigtige besoegende.

## Hvorfor GitHub Actions og ikke en uptime-tjeneste?

Gratis overvaagningstjenester som cron-job.org afbryder forbindelsen, hvis svaret er storre end 64 KB, og flere af sidernes fulde HTML er 250-450+ KB. Det goer, at de reelt ikke varmer cachen op paa en kold side, uanset om man bruger GET eller HEAD. curl her har ingen tilsvarende graense: den henter hele siden og smider den vaek med det samme (-o /dev/null), saa serveren altid genererer og cacher siden helt faerdigt.

## Hvordan det virker

Filen .github/workflows/keep-alive.yml koerer paa et cron-skema, hvert 20. minut. Filen urls.txt indeholder alle 74 URL'er, en pr. linje. Hvert workflow-run henter alle URL'er parallelt (10 ad gangen) og logger statuskode og svartid for hver. Se under fanen Actions i repoet for at foelge kørslerne.

## Vedligeholdelse

Tilfoejer du nye sider til sitet, saa tilfoej dem bare som en ny linje i urls.txt. Skemaet kan justeres i .github/workflows/keep-alive.yml (feltet cron).
