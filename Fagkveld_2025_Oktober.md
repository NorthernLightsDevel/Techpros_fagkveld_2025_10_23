---

marp: true
theme: default
paginate: true
footer: "Fagdag 23. Oktober 2025 - Sikkerhet og AI"
style: |
  section {
    background: white;
    position: relative;
    padding-top: 1.75rem; /* keep content clear of the logo */
  }
  section::before {
    content: "";
    position: absolute;
    top: 0.75rem;
    left: 0.75rem;
    width: 2.5rem;
    height: 2.5rem;
    background: url("./assets/logo.svg") no-repeat center / contain;
  }
  h1, h2, h3 { letter-spacing: 0.02em; }
  .tiny { font-size: 0.8rem; color: #666; }
  .tight li { margin: 0.25em 0; }
  pre {
    font-size: 0.45rem !important;
    line-height: 1.35;
  }
  pre code,
  pre code.hljs,
  code.hljs {
    font-size: 0.35rem !important;
    line-height: 1.35;
  }
-------------------------------

<!-- _class: lead -->

# Techpros Fagkveld 23.10.2025

## AI - Sikkerhet - Workshop

**Av:** Ola & Per Øyvind
**Dato:** 23 Oct 2025

---

# AI

- Hva kan jeg bruke AI til?
- Bruk av AI / LLMs til kodegenerering - Sikkerhet
- Kodeeksempler fra ChatGPT
- "Remember to make it safe" - ikke en selvfølge.
- AGENTS.md - Hva bør være med, og hvorfor

---

# Hva kan jeg bruke AI til?
- Be om oppsummering av større dokumenter
- Skrive om tekst til besvarelser
- Skrive boilerplate-kode for meg
- Annet?

---

# Bruk av AI til kodegenerering - Sikkerhet

- AI er trent på alt som finnes tilgjengelig på internett
- Studentprosjekter
- Hobbyprosjekter
- Kodeeksempler som viser deler av fiksen

---

# Kodeeksempel fra ChatGPT Codex 
**Prompt:** Please schaffold a server named TPFagKveld for a fagkveld, add a registration and login page as the first pages.
* Resultat:
  ```typescript
  async function handleLogin(req, res) {
    try {
      ...
      const match = registrations.find(reg => reg.email === email && reg.password === password);
      ...
    } catch (err) {
      ...
    }
  }
  ```
* Sikkert?

---

# Kodeeksempel fra ChatGPT Codex 
**Prompt:** Please change the project to C#, update AGENTS.md as well to notify of this change.
* Resultat
  ```csharp
  public Attendee? ValidateCredentials(string email, string password)
  {
      if (!_attendees.TryGetValue(email, out var attendee))
      {
          return null;
      }
  
      var hash = HashPassword(password);
      return attendee.PasswordHash == hash ? attendee : null;
  }
  ```
* Marginalt bedre, brukte SHA256

---

# Kodeeksempel fra ChatGPT Codex 
**Prompt:** Great, now make the login page secure.
- Resultat
  ```csharp
    public Attendee? ValidateCredentials(string email, string password)
    {
        if (!_attendees.TryGetValue(email, out var entry))
            return null;
        var result = _passwordHasher.VerifyHashedPassword(email, entry.PasswordHash, password);
        if (result == PasswordVerificationResult.SuccessRehashNeeded)
        {
            ...
            result = PasswordVerificationResult.Success;
        }
        return result == PasswordVerificationResult.Success
            ? new Attendee(entry.FullName, entry.Email)
            : null;
    }
  ```
* Bedre, brukte RFC2898 med SHA256, og salted hashes.
* Fremtidsrettet og støtter bytte av hashalgoritme

---

# "Remember to make it safe" - ikke en selvfølge.

* AI er trent på all kode som ligger på nett
* Skiller ikke på sikker og usikker kode
* Må bevisst be om å gjøre ting sikkert
* Bør legge til sikkerhet som del av AGENTS.md

---

# AGENTS.md – Kjerneseksjoner

* **Prosjektstruktur:** Kartlegg mapper, viktigste filer og hvor artefakter sjekkes inn så agenter navigerer riktig.
* **Build/Test-kommandoer:** List lokale og CI-kommandoer (`marp --preview`, `marp --pdf`) med når de skal kjøres.
* **Kode- og navnestil:** Gjenbruk `.editorconfig`, innrykk og navnestandarder slik at generert kode matcher resten.

---

# AGENTS.md – Kjerneseksjoner

* **Testing og dekning:** Beskriv hvilke tester som finnes, hvor de ligger, og hvilke kvalitetsporter som må passeres.
* **Sikkerhetskrav:** Dokumenter hvordan passord, nøkler og MFA skal håndteres slik at kritiske mønstre ikke brytes.
* **Commit/PR-retningslinjer:** Forklar hvordan historikken struktureres, krav til PR-beskrivelser og hvem som må godkjenne.
* **Promptlogging:** Vis til `PROMPTS.md` og krev at alle forespørsler logges for sporbarhet.

---

# AGENTS.md – Hvorfor det er kritisk

* **Rask onboarding:** Nye bidragsytere og agenter slipper gjetting og unngår feil mappevalg.
* **Forutsigbar kvalitet:** Felles stil og testkrav gjør at outputen blir stabil fra gang til gang.
* **Sikkerhet i praksis:** Klare guardrails hindrer at automatiserte endringer svekker autentisering eller logging.
* **Audit trail:** Når logging og PR-krav er standardisert, kan vi spore hvem som gjorde hva og hvorfor.
* **Kontinuerlig forbedring:** Dokumentet kan itereres etter hver hendelse og gir én kilde til sannhet.

---

# AGENTS.md – Operasjonelle vaner

* Oppdater dokumentet samtidig som nye verktøy, mapper eller rutiner introduseres.
* Lenke direkte til relevante filer (`AGENTS.md`, `PROMPTS.md`, `appsettings.json`) for raske oppslag.
* Marker områder som **ikke** skal røres (for eksempel hashing-kode) og referer til ansvarlige personer.
* Avslutt med en sjekkliste (for eksempel «Kjør `marp --preview`, oppdater PROMPTS.md, last opp eksport»).

---

# Sikkerhet

* Sikkerhetshendelser fra i sommer
* Hvilke taktikker bruker ondsinnede aktører for å få deg til å trykke?
* E-post spoofing - Nettside-spoofing - eksempler på dette
* Gophish og annet
* Bruk av password managers - Bitwarden, 1 pass, etc.
* Hva gjør man hvis credentials blir lekket, hvem rapporterer man til og hvilke ting skal man tenke på?

---

# Sikkerhetshendelser fra i sommer

- [ KNP Logistics Group](https://thehackernews.com/2025/09/how-one-bad-password-ended-158-year-old.html) - manglende MFA og passordrot ga angripere full tilgang.
- [NPM supply-chain-angrep](https://vercel.com/blog/critical-npm-supply-chain-attack-response-september-8-2025) - avhengighet forsøkte å stjele tokens fra utviklermaskiner.

---

# Hvilke taktikker bruker ondsinnede aktører for å få deg til å trykke?

* Frykt og hast
* Fristelser og penger
* Autoritet og apell
* Nysgjerrighet

---

# E-post spoofing - Nettside-spoofing 

* Spoofing av epost, sms eller telefon-nummer.
* Spoofing kan til og med "være ekte"
* Sandboxing og løgnaktige lenker

---

# Hva er spoofing?
* Ingen validering på SMTP.
* Typosquatting
  * eksempel: rnicrosoft.com og microsoft-login.com (heldigvis legitim)
* Hyperlink tekst viser en annen tekst enn det lenken er: [https://dnb.no](https://www.hacker-paradise.no/owned)
* Url-forkortere: bit.ly, tinyurl, shorturl.at
* Kombinert med teknikkene over er det napp å få!

---

# Gophish og annet
**Gophish** kan brukes for å lure ansatte. Morsomt verktøy!
<img src="https://docs.getgophish.com/user-guide/~gitbook/image?url=https%3A%2F%2F732773220-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LDT_qt7WICxCmlM75gA%252F-LOLuduA5T1_kf4hd_Rk%252F-LOM16w4LVlVfVrIGkB3%252Flocalhost_3333_campaigns_25%28macbook%29.png%3Falt%3Dmedia%26token%3D9632e636-be64-42d9-906d-e783ee984a5a&width=768&dpr=4&quality=100&sign=986f944a&sv=2" width="700px" />

---

# Gophish og annet
**Hoxhunt** kan brukes for å trene ansatte.
<img src="https://thestartupmag.com/wp-content/uploads/2020/01/image5.png" width="600px" />

---

# Bruk av password managers - Bitwarden, 1 pass, etc.

* Ett hovedpassord - hovednøkkel.
* Sikker lagring
* Passord-generering
  * Før hvelvet var "alligator123" utbredt!
* Automatisk utfylling
* Synkronisering

---
## Slutt på unødvendig rotering
* Om noen finner ut at du brukte passordet "Sommer2024" er du ikke trygg i 2025..
* Ingen nedskrevne passord
* Deling og sending av krypterte passord

* **Bitwarden**, **1pass** og **Protonpass** er gode alternativer
* Passordhvelv reduserer dramatisk risikoen for innbrudd. Det er enkelt, men krever litt rutine.

---
# Hva gjør man hvis credentials blir lekket?
**Hvem rapporterer man til og hvilke ting skal man tenke på?**
* Rapporter til nærmeste leder/sikkerhetsavdeling hos kunde hvis det er relevant
* Rapportere til Malin og/eller Per Øyvind så vi er informert
* Samarbeid med kunde sin sikkerhetsavdeling for å mitigere trusselen
* Sørg for å erstatte passord som potensielt er på avveie

---

# Workshop: Kompromittert repo

**Scenario:**
Du er ansatt i et selskap som nylig har fått et sikkerhetsbrudd via passordlekasje i et GitHub-repo. Angriperne er identifisert og dere mistenker at målet er samfunnskritisk infrastruktur.

**Oppdrag:**
- Kartlegg hvilke logger og systemer som må gjennomgås for å avdekke skadeomfang.
- Beskriv strakstiltak og ansvarslinjer som stabiliserer repoet og informerer berørte team.

---

# Workshop-case: Phishing fra Valdress

**Scenario:**
Målrettet spear phishing mot ledergruppen med flere troverdige meldinger som etterspør sensitiv informasjon.

**Oppdrag:**
- Beskriv hvordan dere identifiserer kompromitterte brukere og gjenoppretter sikker kommunikasjon.
- Lag en plan for kommunikasjon, isolering og etterarbeid som gjenoppretter tilliten i organisasjonen.

---

# Workshop-case: AI-promptlekkasje

**Scenario:**
Kundesupport-teamet hos kunden dere jobber for har fått en prompt-injeksjon som ressulterte i at en LLM har delt API-nøkler via en chatbot.

**Oppdrag:**
- Lag en plan for å rotere nøkler og informere berørte kunder.
- Definer tiltak som hindrer at prompt-injeksjonen lykkes igjen (policy, tooling, opplæring).

---

# Failsafe for workshop

- Reserveøvelser med quiz og mini-demo om MFA hvis tiden går tom.
- Klare sjekklister slik at læringsmål oppnås uansett tempo.

**På norsk:** Publikum skal sitte igjen med konkrete grep selv om agendaen må kuttes.
**Plan B:** Ha korte videoer og kodeeksempler klare i `assets/`-mappen.

---

# Det var det

Spørsmål?
