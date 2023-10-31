# Unranked Challenge

## Foreword

All command names are subject to change, right now it's mostly an idea.
There were subcommands thus many of them are duplicated intentionally for similarity of API but this is not required if the commands need to be top level commands or if them being top level commands instead would provide a better experience.

Markers Meaning:

- [NTH] All functionality marked with this is only Nice To Have and can be left out if time does not permit as they were either not used much or not critical.
- [AdminOnly] Indicates only privileged users should be able to execute this command.

## Open Questions requiring a decision

- Should we allow the privileged commands at all?
  They weren't used often and they might give too much power if we let this work across all servers.
- Do we want to support two modes? A mode where they can participate in the global leaderboard and another one where they only participate within that server.
  Would require that we add an option for privileged users to switch between the two.
  I'd discard the local data if they switch to global so it would be a destructive operation to switch to global.

## Supported UI

### Unranked Event Scoring

The commands used for the actual management of users scores during the event.

The previous bot did not support / use slash commands.
So it accepted a number as one of the commands, any integer.
That number would then be sent to `score` as an argument.
If setting the score is easy enough I don't think we need this.

- `disp()` Shows the current scores saved in like a leaderboard
- `rem()` Allows a player to remove themselves from the rankings [NTH]
- `score(value: Number)` Allows a player to register their score (Overwrites if already exists)
- `any_score(enabled: bool)` changes the scoring mode to allow any integer instead of only 0..10 [AdminOnly].

### Idea Management

The commands used to manage the submission and voting on ideas for the next unranked challenge.
Players are allowed to vote for more than one idea at the same time.

- `disp(show_all: bool = false)` Shows the current ideas and the number of votes for each.
  if `show_all` is true then it also shows who voted for each idea.
  This had a lot of info which is why it wasn't the default.
- `new(idea: String)` Creates a new idea
- `rem()` Removes an idea [AdminOnly] (Maybe also allow the original user who created it to remove it?)
- `rename(new_idea: String)` Changes the description of an idea.
  Maybe should be restricted to only the user who created the idea.
  And if someone wants to change someone else's idea maybe it should require [AdminOnly].
- `vote(idea_id: Number)` Allows a player to vote for an idea.
- `unvote(idea_id: Number)` Allows a player to remove their vote for an idea if a number (the ID of the idea) is passed.
- `unvote_all()` Allows a player to remove their vote from all ideas.

## Scheduled Actions

At the start of unranked each season the following should happen.
I used to do it manually when unranked started but that was less than optimal.

- Clear the scores for the last season
- Determine the winning idea (Highest votes or lower ID if tied)
- Set the new challenge message based on the winning idea (Should show with the scores so people know what the challenge is).
- Clear the ideas. Keep any ideas with more than `x` votes except for the winning one. (`x` used to be 3)