# Sports Api
> Dette er stadig under udvikling, og vil kunne ændre sig

> Mangler stadig ***Andre kampe*** og ***Stilling***

---
## Dagens kampe
```
GET: {api-url}/api/matches
```

### Anvendelse
Giver dagens kampe. Bør kaldes ved app to foreground/start og caches lokalt.

### Eksempel
```json
{
    "data": [
        {
            "id": "9781cec8-35e8-42f5-aece-679a77deeca3",
            "sid": 12,
            "startTime": "2018-06-30 16:00:00"
        },
        {
            "id": "9781cf0d-98da-4659-af36-a949f8e75d6a",
            "sid": 12,
            "startTime": "2018-07-30 16:00:00"
        }
    ]
}
```

### id `string`
Kampens unikke id, som bruges til at hente kampdata
### sid `int`
Backend sid
### startTime `string`
Kampens anslået starttidspunkt. Dansk tid. Der er ikke angivet et sluttidspunkt, da det 
som regel vil være meget upræcist. Tjek istedet kampdata for en status.

---
## Kampdata
```
GET: {api-url}/api/matches/{id}
```

### Anvendelse
Kaldes hver 15 sekund så længe kampen er aktiv og bliver set af brugeren.

Returnerer det fulde datasæt, hver gang, så vi ikke skal diffe på data.

### Eksempel
```json
{
    "data": [
        {
            "teams": {
                "home": {
                    "name": "Danmark",
                    "logo": "https://media.api-sports.io/football/teams/2.png"
                },
                "away": {
                    "name": "Frankrig",
                    "logo": "https://media.api-sports.io/football/teams/2.png"
                }
            },
            "status": {
                "short": "1H",
                "elapsed": 24,
                "homeGoal": 2,
                "awayGoal": 1
            },
            "events": [
                {
                    "type": "goal",
                    "details": "normal",
                    "elapsedTime": 23,
                    "homeTeam": false,
                    "player": "C. Eriksen",
                    "assist": "N. Bendtner"
                },
                {
                    "type": "card",
                    "details": "yellow",
                    "elapsedTime": 42,
                    "homeTeam": true,
                    "player": "Mbappe",
                    "assist": null
                },
                {
                    "type": "subst",
                    "details": "subst 1",
                    "elapsedTime": 56,
                    "homeTeam": true,
                    "player": "Mbappe",
                    "assist": "N. Bendtner"
                }
            ],
            "statistics": [
                {
                    "type": "Totale skud",
                    "home": "2",
                    "away": "10"
                },
                {
                    "type": "Boldbesiddelse",
                    "home": "40%",
                    "away": "60%"
                }
            ],
            "lineups": {
                "home": {
                    "formation": "4-3-3",
                    "coach": "Guardiola",
                    "start": [
                        {
                            "name": "Nicklas Bendtner",
                            "number": 2,
                            "pos": "D"
                        },
                        {
                            "name": "Christian Eriksen",
                            "number": 10,
                            "pos": "M"
                        }
                    ],
                    "subst": [
                        {
                            "name": "Nicklas Bendtner",
                            "number": 2,
                            "pos": "D"
                        },
                        {
                            "name": "Christian Eriksen",
                            "number": 10,
                            "pos": "M"
                        }
                    ]
                },
                "away": {}
            }
        }
    ]
}
```

### teams `object`
- ### home `object`
    - ### name `string`
        Dansk version af holdnavn
    - ### logo `string`
        url til logo
- ### away `object`
    - ### name `string`
        Dansk version af holdnavn
    - ### logo `string`
        url til logo
### status `object`
- ### short `string`
    Status for kampen

        NS
        - Not Started

        1H
        - First Half, Kick Off

        HT
        - Halftime

        2H
        - Second Half, 2nd Half Started

        ET
        - Extra Time

        P 
        - Penalty In Progress

        FT
        - Match Finished

        BT
        - Break Time (in Extra Time)

        SU
        - Match Suspended or interrupted
- ### elapsed `int`
    Tid spillet i minutter
- ### homeGoal `int`
    Antal mål hjemmehold
- ### awayGoal `int`
    Antal mål udehold
### Events `array`
- ### type `string`
        goal

        card

        subst
        - player angiver spiller som bliver skiftet ud
        - assist angiver spiller som bliver skiftet ind

        var
        - https://en.wikipedia.org/wiki/Video_assistant_referee 😉
- ### details `string`
    Hver type har unikke details

        For goal:
            normal
            own
            penalty
            missedPenalty

        For card:
            yellow
            yellowSecond
            red

        For subst:
            subst {1, 2, 3}

        For var
            goalCancelled
            penaltyConfirmed
- ### elapsedTime `int`
    Minut hvor det skete
- ### homeTeam `bool`
    Angiver om det var hjemmeholde
- ### player `string`
    Navn på spiller
- ### assist `string optional`
    Navn på spiller som lavede assist
### statistics `array`
- ### type `string`
    Hver type er unik og kommer max 1 gang. Bliver oversat til dansk fra backend

        Shots on Goal
        Shots off Goal
        Shots insidebox
        Shots outsidebox
        Total Shots
        Blocked Shots
        Fouls
        Corner Kicks
        Offsides
        Ball Possession
        Yellow Cards
        Red Cards
        Goalkeeper Saves
        Total passes
        Passes accurate
    - ### home `string`
    - ### away `string`
### lineups `object`
- ### home `array`
    - ### formation `string`
    - ### coach `string`
    - ### start `array`
        - ### name `string`
        - ### number `int`
        - ### pos `string`
                G
                - Målmand

                D
                - Forsvarer

                M
                - Midtbane

                F
                - Angriber
    - ### subst `array`
        samme som start
- ### away
    samme som home
