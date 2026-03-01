# Pongstrong
Pongstrong is a webapp for managing Beer-Pong tournaments, which at the time were organized by two friends of mine. This is the first big project I ever finished, and it successfully was used in production as well. 

It was designed to run on a normal server, but for some reason I put it into a container of which then only one was allowed, and I had to manage the state by loading a backup every time it restarted. 

I had never touched cloud infrastructure before, so this was a lot of unnecessary work I did just to get Pongstrong online.

If you're looking for the more complete and feature-rich version of Pongstrong, check it out [here](https://github.com/Captainbommel/pongstrong-flutter-app).

## Tech Stack

The backend is written in Go using the standard library for HTTP and `text/template` for rendering the HTML pages. Session management is handled by `gorilla/sessions` and the state is backed up to Google Cloud Storage.


## Examples

I filled the templates with data so anyone can see how Pongstrong would look like:
- [controlpanel.html](examples/controlpanel.html)
- [login.html](examples/login.html)
- [regeln.html](examples/regeln.html)
- [spielfeld.html](examples/spielfeld.html)
- [turnierbaum.html](examples/turnierbaum.html)
- [übersicht.html](examples/übersicht.html)

## Features

Under `/controlpanel`, if you have the admin password, you can initialize a tournament by uploading a JSON file with the following structure:

```json
[
  [
    { "name": "Team1", "mem1": "Player1", "mem2": "Player2" },
    { "name": "Team2", "mem1": "Player3", "mem2": "Player4" },
    { "name": "Team3", "mem1": "Player5", "mem2": "Player6" },
    { "name": "Team4", "mem1": "Player7", "mem2": "Player8" }
  ],
  [
    { "name": "Team5", "mem1": "Player9", "mem2": "Player10" },
    { "name": "Team6", "mem1": "Player11", "mem2": "Player12" },
    { "name": "Team7", "mem1": "Player13", "mem2": "Player14" },
    { "name": "Team8", "mem1": "Player15", "mem2": "Player16" }
  ]
]
```

Each inner array represents a group of 4 teams.

This generates all the matches for the first phase of the tournament. Everyone with just the user password can then login and start or finish matches. To finish a match you have to input the amount of cups each team has scored, which then are displayed in the standings table and in an overview page for the matches in each group.

After all matches of the first phase are finished, one can advance to the knockout phase using a button in the admin panel. At this point it is also a good idea to let players check all results and correct them, which also can be done from the admin panel. Even though there is a hint for this, you have to know how the IDs for the matches are determined to update the correct match.

The second phase works mostly like the first one for players, before starting to play they start the match in the app and afterwards finish it by inputting the result. The brackets are named Champions, Europa, and Conference, loosely inspired by the UEFA competitions. The Europa and Conference winners play each other in a Super round, and the winner of that faces the Champions winner. All of this can be viewed on the bracket page.

There is also a page for the tournament's rules, in case there was a dispute between teams, but I doubt anyone ever used that. It does however explain the scoring system.

At the end of a tournament the app exports a file with points for each individual player based on how far their team made it. This was used to track player performance across multiple tournaments. 


## Configurability

The tournament structure and the rules that are applied to the scores are specifically designed for the tournaments organized by my friends and can't be configured. There is, however, code for a 6 and an 8 group version that you can manually switch out depending on what is needed.

Progress is only saved every few minutes because of the weird way it was set up in a Docker container. The backup interval and a few other things like the user and admin passwords are configured through environment variables.

This project is licensed under the [MIT License](LICENSE).
