# Raffle Commands

**Table of Contents** 

- [Create a giveaway (Admin)](#create-a-giveaway-admin)
- [Pause a giveaway (Admin)](#pause-a-giveaway-admin)
- [End a giveaway (Admin)](#end-a-giveaway-admin)
- [Resume a giveaway (Admin)](#resume-a-giveaway-admin)
- [Vote in a giveaway (Anon)](#vote-in-a-giveaway-anon)
- [Pick a winner (Admin)](#pick-a-winner-admin)
- [Hide a giveaway (Admin)](#hide-a-giveaway-admin)
- [End a giveaway (Admin)](#end-a-giveaway-admin)

A giveaway is like a [poll](./poll.md) but you can pick a winner and have to announce a prize.

#### Create a giveaway (Admin)

To create a giveaway you must be an admin and send the following message:

```json
{
  "method":"createRaffle",
  "params":{
    "channel":"CHANNEL",
    "question":"QUESTION",
    "prize":"Description of Prize",
    "choices":[
      {
        "text":"Answer 1"
      },
      {
        "text":"Answer 2"
      }
    ],
    "start_time":"2014­12­10T14:53:33.142Z",
    "nameColor":"Hexcode for color",
    "subscriberOnly":true/false,
    "followerOnly":true/false
  }
}
```

You can decide if the giveaway will only be accessible by followers, subscribers or all users.

The server will then send a message like this to the channel:

```json
{
  "method":"raffleMsg",
  "params":{
    "channel":"CHANNEL",
    "question":"QUESTION",
    "prize":"Description of Prize",
    "choices":[
      {
        "text":"Answer 1",
        "count":0
      },
      {
        "text":"Answer 2",
        "count":0
      }
    ],
    "start_time":"2014­12­10T14:53:33.142Z",
    "status":"started",
    "forAdmin":true/false,
    "nameColor":"Hexcode for color",
    "subscriberOnly":true/false,
    "followerOnly":true/false
  }
}
```

Counts will be removed if you aren't an admin.

#### Pause a giveaway (Admin)

To pause a giveaway send the following message:

```json
{
  "method":"pauseRaffle",
  "params":{
    "channel":"CHANNEL"
  }
}
```

The server will then send a raffleMsg with the status set to "paused".

#### End a giveaway (Admin)

To end a giveaway send the following message:

```json
{
  "method":"endRaffle",
  "params":{
    "channel":"CHANNEL"
  }
}
```

The server will then send a raffleMsg with the status set to "ended". The giveaway must be in this state to roll a winner.

#### Resume a giveaway (Admin)

You can also resume a giveaway if it's still in a paused state.

```json
{
  "method":"startRaffle",
  "params":{
    "channel":"CHANNEL"
  }
}
```

#### Vote in a giveaway (Anon)

To vote in a giveaway a user must send this message to the server:

```json
{
  "method":"voteRaffle",
  "params":{
    "name":"USERNAME",
    "channel":"CHANNEL",
    "choice":"Number of answer, starts at 0"
  }
}
```

The server will periodically send a raffleMsg to admins to update the results.

#### Pick a winner (Admin)

To pick a winner you must end the giveaway first and then send the following message:

```json
{
  "method":"winnerRaffle",
  "params":{
    "channel":"CHANNEL",
    "answer":"Number of answer, starts at 0"
  }
}
```

The server will respond with two times the following message, one for all users without the email (with forAdmin set to false) and one for admins (with forAdmin set to true) with the email:

```json
{
  "method":"winnerRaffle",
  "params":{
    "channel":"CHANNEL",
    "winner_name":"Test-Account",
    "winner_email":"example@example.com",
    "winner_picked":true,
    "forAdmin":true,
    "answer":"Number of answer, starts at 0"
  }
}
```

#### Hide a giveaway (Admin)

To hide a giveaway send this message to the server:

```json
{
  "method":"hideRaffle",
  "params":{
    "channel":"CHANNEL"
  }
}
```

The server will send a raffleMsg to the channel with the status set to "hidden".

#### End a giveaway (Admin)

To end a giveaway send this message to the server:

```json
{
  "method":"cleanupRaffle",
  "params":{
    "channel":"CHANNEL"
  }
}
```

The server will send a raffleMsg to the channel with the status set to "delete".
