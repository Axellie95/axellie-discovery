# axellie-discovery

Discovery-formulär som Anton (Axellie) skickar till nya kunder/prospects inför
ett AI-uppdrag. Ett underbibliotek per kund, medvetet neutralt repo-namn så att
inga kundrelationer syns i repo-listan.

| Sökväg | Live-URL | Kund |
|---|---|---|
| `tesselet/` | https://axellie95.github.io/axellie-discovery/tesselet/ | Tesselet Robotics (2026-08) |

## Så funkar formulären

Ett självständigt `index.html` per kund — ingen build, inga beroenden.
Svaren POSTas direkt till KOSA-Supabase (`oyzrcjrvppzhbuethicc`) via anon-nyckeln,
en tabell per kund (t.ex. `public.tesselet_discovery`).

**RLS-mönstret är hela säkerheten:** anon får `INSERT`, aldrig `SELECT`.
Vem som helst kan alltså svara, ingen kan läsa andras svar — bara Anton, via MCP.
Verifiera alltid båda delarna innan en länk går ut:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -X POST "$URL/rest/v1/<tabell>" \
  -H "apikey: $ANON" -H "Authorization: Bearer $ANON" \
  -H "Content-Type: application/json" -d '{"respondent":"TEST"}'   # → 201
curl -s "$URL/rest/v1/<tabell>?select=*" \
  -H "apikey: $ANON" -H "Authorization: Bearer $ANON"              # → []
```

## Ny kund

1. Kopiera närmaste befintliga mapp, byt frågor, tabellnamn och färger.
2. Skapa tabellen + RLS-policyn (`insert to anon with check (true)`).
3. Kör verifieringen ovan, städa bort testraden, pusha.

Sidorna har `noindex` och nämner ingen konfidentiell information — men länken är
publik, så be alltid respondenterna hålla svaren oklassificerade.
