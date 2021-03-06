

```python
import numpy as np
import pandas as pd
import os

```

    /anaconda3/lib/python3.6/importlib/_bootstrap.py:219: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88
      return f(*args, **kwds)
    /anaconda3/lib/python3.6/importlib/_bootstrap.py:219: RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88
      return f(*args, **kwds)



```python
%%html
<style>
table td {text-align:left !important}
table th {text-align:center !important}
</style>
```


<style>
table td {text-align:left !important}
table th {text-align:center !important}
</style>


## __The Problem to Be Solved__

This project was inspired by the Green Bay Packers, an NFL team that has relied heavily upon a draft and develop philosophy, particularly with respect to their Cornerbacks and Safeties (together, '__Defensive Backs__', or, '__DBs__').  This philosphy involves drafting talented (but much less expensive) players out of college, signing them to three or four year contracts and developing their abilities, rather than paying the higher contract prices of more experienced players who have proven records in the NFL.

Drafting and developing DBs worked well for the Packers for a number of years, until it didn't.  Some of the players they developed into exceptional DBs left the Packers to play for other teams that were willing to pay more  after their initial contracts with the Packers expired.  The Packers adapted for a few years, because they had good, younger players coming up behind those that left.  But, at some point, around 2015 or so, the younger players weren't developing and putting in the kinds of performances needed for a top-notch defense.  The failure to identify players who would become successful at DB hurt the Packers win-loss record and also impacted the organization financially.

Being a successful NCAA player doesn't necessarily translate into becoming a successful NFL player.  The knowledge, skills and abilities needed to be successful in the NFL are different enough that some excellent NCAA players are unable to make the adjustment and never become excellent players in the NFL.

I designed this project to see if I could identify metrics of success through machine learning that might help the Packers identify those players who were more likely to successfully make the transformation from the NCAA to the NFL, for as long as the Packers insist on relying upon draft and develop.  I've defined success as having played on an NFL teams' 53-man roster for at least three years.  The reasoning for this is that if a player has earned a spot on the tight roster for at least three years, many experts have determined that he's at least good enough to play in the NFL and players are typically given two full years to make the transition to the NFL. 

As I've argued, NCAA game statistics are part of the equation.  Excellent play in the NCAA is definitely correlated with the ability to make it in the NFL.  But I also believe that there are other factors, some of which may be identifiable and measurable, that may indicate a player's ability to adapt to the needs of the NFL.

In accordance with this thesis, I've placed an emphasis on factors other than game statistics.  Game statistics are still pieces of the puzzle, but I'd like to understand how important they are and how important other aspects of a player are.  The next section examines the requirements of the position and the following section describes the data I was able to pull together that addresses a player's ability to meet these requirements.

<br>
<br>

## __Requirements of the Position__

Defensive Backs' primary responsibility is to guard against the pass by covering the opposing offense's wide receivers.  This requires them to be able to run fast, jump high, maneuver quickly and have some instincts for finding a ball in the air while doing so.  Being sure-handed is also helpful if they are able to put themselves in positions to intercept the ball.  

Defensive Backs must also be big enough and strong enough to make tackles if the pass is completed and on the occasions when they must tackle a 230+-pound running back who has made it through the Defensive Line and past the Linebackers.  And, they must be durable enough to avoid injuries while doing so.

Finally, they must be 'football smart' enough to learn their responsibilities for each defensive play and to read opposing offenses' formations to be able to anticipate each play.

In comparison to other positions, DBs rely more heavily on raw athleticism and less on football knowledge and experience.  As evidence of this, it is not uncommon for a player to move from wide receiver to DB and/or back.  Both positions rely on the same types of athleticism.  It is also not uncommon for an NCAA football coach to recruit a DB from another sport, such as Track or Basketball; the athleticism that made them successful at these other sports may outweigh years of experience playing football in middle school, high school and early college.  This is seldom the case for skill positions, such as Quarterback, Running Back, Linebacker or even Offensive or Defensive Line.

It should be noted that Safeties, Free Safeties and Strong Safeties also occasionally rush the Quarterback and have greater responsibility in stopping the run game.  As such, they tend to be slightly bigger than pure Cornerbacks.  Players often shift between DB positions over their careers.  Some NCAA teams made the distinction between the positions by listing some players as 'SS', 'FS', 'S', or 'CB', but the vast majority of NCAA teams listed all players as 'DB' throughout most of the years covered in the data collected (2005 - 2017).

It should also be noted that defenses change over time and, with that, the balance of attributes that makes for successful players may also change slightly.  For instance, defenses have been emphasizing speed increasingly in recent years.

<div>
  <table>
    <thead>
      <tr>
        <th>Athleticism</th>
        <th>Football Intelligence</th>
        <th>Physical Attributes</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>accelerate quickly and run fast</td>
        <td>learn responsibilities on each defensive play</td>
        <td>height</td>
      </tr>
      <tr>
        <td>jump high</td>
        <td>read the offense</td>
        <td>weight</td>
      </tr>
      <tr>
        <td>ability to change directions quickly</td>
        <td>find the football while jumping and keeping track of the offensive player</td>
        <td>durability</td>
      </tr>
    </tbody>
  </table>
</div>

<br>
<br>

## __The Path to Success__
According to an [article from CBS News](https://www.cbsnews.com/news/the-odds-of-playing-college-sports/), the odds that a high school senior football player will eventually be drafted by an NFL team are 0.08%.  One in 16 high school senior football players will play college football at some level, some of those players will stop playing during college, and, for those who do continue to play through their senior year of college, only 1 in 50 will be drafted by an NFL team.

There are several possible paths to the NFL, but the typical path involves being a high school standout, getting a full scholarship to play for an NCAA Division I team, gaining enough recognition to be invited to participate in the NFL Combine and eventually being drafted by an NFL team in the Spring of a player's senior year of college.  

Alternatively, a player may stand out at a Division II or even a Division III school, may attend a college Pro Day instead of the Combine and may even play in the Canadian Football League for a year or two before making their way to the NFL.  And, a player not drafted may may opt to walk-on to a team's Training Camp, a period of time prior to the start of each season where players warm-up for the season, learn new plays and many try to prove that they deserve a place at or near the top of the team's depth chart.  

At the beginning of each season, teams are allowed 90-man rosters throughout Training Camp, but the rosters must be trimmed to 53 prior to the first game of the regular season.  Some of those cut may try out for other teams, others may be asked to join the team's Practice Squad.  A player on the Practice Squad may be promoted to the 53-man roster if a player ahead of him on the team's depth chart suffers a season-ending injury.  Either way, he'll have another chance to prove himself at the following season's Training Camp.

<br>
<br>

## __Data: What Would Be Useful?__

Useful data can be broken into three categories:
1. __football experience__
    - number of years played
    - quality of the teams played on
    - performance, as measured by game statistics
2. __raw athleticism__
    - speed and acceleration
    - vertical leap
    - ability to change directions / dexterity / footwork
    - ability to catch
3. __biographical information__ 
    - height
    - weight
    - birth date
    - state in which the player grew up
    - ability to learn a defense and study an offense, which could be measured by academic performance
    
The state in which the player grew up is relevant because some states tend to place a greater emphasis on football.  Also, Southern states often have organized football leagues 9 - 12 months a year, whereas leagues in Northen states tend to be limited to 3-4 months in Autumn.

Birth date may be relevant, according to a theory set forth by Malcolm Gladwell.  In his book, _Outliers_, Mr. Gladwell makes a persuasive argument that players in all sports who are older than their peers growing up tend to be significantly more mentally, emotionally and physically mature than their peers, and thus get more playing time and build more confidence.  

For example, a school district may recommend than students turning 7 prior to January 1st enroll in first grade.  Therefore, students born in early January, just after the cut-off date, could be 10-11 months more mature than their peers.

It's a compelling argument, but it's difficult to apply en masse, as there is no federally-mandated cut-off date.  Instead, each school district across the country sets its own date.  Furthermore, the cut-off date is only a recommendation, so, parents can decide to hold their children back for a year voluntarily.  Checking for each of these factors for the 10,000+ NCAA players in this pool is beyond the scope of this project.

<br>
<br>

## __Data: What is Available?__

__Football Experience__:  High school statistics are not widely available and NFL statistics are irrelevant because I'm approaching this as a recruiter, looking for talent at the college level to draft and develop.  So, NCAA game statistics are the obvious choice.  

I decided that comparing players who played in Divison II or Division III against players in Division I was unfair and not useful.  That's not to say that game statistics for Division II and III players are irrelevant, but, in a machine learning exercise, DII & DIII statistics would have to be discounted at some rate to provide for the difference in the speed of the game and the skills of the players at the different levels.

__Raw Athleticism__:  The NFL Scouting Combine is an invitation-only event that is held in Indianapolis every year in February or March.  Results from the Combine are ideal for testing raw athleticism.  In particular, the 40-yard dash, 3 cone drill, vertical leap and shuttle drills test a player's speed, acceleration, agility and leaping ability.  The downside to using this data is that the event is invitation-only, and not all players who play in the NFL participate in the Combine.  As a result, this data is not available for all NCAA Division I players, or, for all NFL players.

__Biographical Data__:  Each NCAA Division I player's height, weight and home state are available in the NCAA date available from cfbstats.com and described more fully below.

<br>
<br>

## __Data Collection and Cleaning__

### __NCAA Bios__ - Importing .csvs from cfbstats.com
---
I found a collection of .csv files for NCAA players from 2005 - 2013 hosted on someone's Google Drive from a link in a subreddit.  Later, I learned that that data set was also posted on [Kaggle](https://www.kaggle.com/mhixon/college-football-statistics), and that it was initially an open database hosted at [cfbstats.com](http://www.cfbstats.com).  That database disappeared behind a (very expensive) paywall in 2014.  The data set posted on Kaggle had been downloaded by someone just before the site put up the paywall in 2014.

I wrote to cfbstats.com, explained that I was a student and that I wanted to use their data for a data science project.  I asked if they had acadmeic rates, but never heard back from them.  It wouldn't be unreasonable to assume that even the data from 2005 - 2013 improved once access to that data disappeared behind the paywall.  Having access to that data would have saved me a lot of time and could have provided more complete and possibly more reliable data.  If I were actually getting paid for this work by an NFL team, I would have pushed them to purchase access to this data.

This was the most complete source of NCAA data I found.  It contained not only game stats, but also biographical information, including height, weight and home state, which figured to be useful for the reasons stated above.  My first concern was that it only covered players from 2005 - 2013.  I had already set the bar for success at 3+ years in the NFL, which meant that in order to have played at least 3 years in the NFL, a player would have left college after the 2014 season.  Despite the fact that this data set did not contain game data for the NCAA 2014 season, I decided to proceed with it.

The .csvs for this data were split into teams, players and game stats, and each of these was split into one .csv file per year.  The .csvs with the prefix 'player' contained biographical information on each player, as well as a column labeled 'Player Code' which acted as an id to tie the biographical information to the player's game stats for that year, contained in the files prefixed with 'games':

![File Structure](images/cfbstats.com_data_structure.png)

#### Issues:
__1. Two ids for a single player__: After concatenating this data into a single DataFrame, I discovered that some of the players' NCAA careers had been mistakenly split into two or more entries; one id had been assigned to a year or two of a player's career and another id to the remainder of the same player's career, where there should have been a one-to-one relationship between id and player + player stats.  I went through, team-by-team and player-by-player and corrected this, not only by using pandas' .duplicated() function, but also by looking through each name for slight variations on the name.  
__2. Aggregated statistics were inaccurate:__ Within a given year, each player's stats were provided on a game-by-game basis, which I aggregated to get total stats for the year.  After some initial Exploratory Data Analysis (__'EDA'__) I found these stats to be consistently lower than the stats available on the ncaa.org website.

#### NCAA Biographical Information Features

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>player_id</b></td>
        <td>'Player Code' as downloaded; <b>should</b> have been a unique id for each player</td>
      </tr>
      <tr>
        <td><b>player</b></td>
        <td>player's first and last name</td>
      </tr>
      <tr>
        <td><b>team</b></td>
        <td>NCAA Division I school player attended</td>
      </tr>
      <tr>
        <td><b>class</b></td>
        <td>unused</td>
      </tr>
      <tr>
        <td><b>position</b></td>
        <td>player's officially listed position</td>
      </tr>
      <tr>
        <td><b>height</b></td>
        <td>official NCAA listed height</td>
      </tr>
      <tr>
        <td><b>weight</b></td>
        <td>official NCAA listed weight</td>
      </tr>
      <tr>
        <td><b>home_town</b></td>
        <td>town player lived in prior to college</td>
      </tr>
      <tr>
        <td><b>home_state</b></td>
        <td>state player lived in prior to college</td>
      </tr>
      <tr>
        <td><b>home_country</b></td>
        <td>player's country of origin</td>
      </tr>
      <tr>
        <td><b>last_school</b></td>
        <td>last high school attended prior to college</td>
      </tr>
      <tr>
        <td><b>first</b></td>
        <td>first name</td>
      </tr>
      <tr>
        <td><b>last</b></td>
        <td>last name</td>
      </tr>
      <tr>
        <td><b>stats_name</b></td>
        <td>player name as it appears in the NCAA Stats DataFrame - used to merge the two DataFrames</td>
      </tr>
    </tbody>
  </table>

</div>

#### Stats on Stats: NCAA Biographical Data

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>#</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>Players</b></td>
        <td>7,528</td>
        <td>DBs from an NCAA Division I school with biographical data between 2005 and 2013</td>
      </tr>
      <tr>
        <td><b>In Study</b></td>
        <td>5,723</td>
        <td>DBs who had matching NCAA game stats and were included in this project</td>
      </tr>
    </tbody>
  </table>
</div>

<br>
<br>

### __NCAA Stats__ - Using BS4 to scrape stats from the web
---
After an appropriate period of mourning over the loss of a single, clean data source, I decided I had to scrape the [ncaa.org website](https://web1.ncaa.org/stats/StatsSrv/careersearch).  I used BeautifulSoup4 to write a scraper in VS Code that would first query a team, scrape the team's rosters for DBs from 2005 - 2017 to build a unique set of players and their ids, then query each player's id for the player's career stats.  

[ncaa_scraper.py](sausage_factory/ncaa_scraper.py)

I turned each player's career into a single row of a DataFrame by creating a set of columns to represent statistics for each potential year of a player's career - Redshirt Freshman, Freshman, Sophomore, Junior and Senior and collected of a team's players into a single DataFrame.  Again, I needed to go through each team to look for players who had been assigned multiple ids and then join their entries into a single entry.  The game stats for each year are as follows:

#### NCAA Stats Features

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>games</b></td>
        <td>number of games played</td>
      </tr>
      <tr>
        <td><b>tackles_solo</b></td>
        <td>solo tackles</td>
      </tr>
      <tr>
        <td><b>tackles_asst</b></td>
        <td>assisted tackles</td>
      </tr>
      <tr>
        <td><b>tfl_solo</b></td>
        <td>solo tackles that resulted in a loss of yards on the play</td>
      </tr>
      <tr>
        <td><b>tfl_asst</b></td>
        <td>assisted tackles that resulted in a loss of yards on the play</td>
      </tr>
      <tr>
        <td><b>tfl_yards</b></td>
        <td>total number of yards lost by the offense for solo and assisted tackles for loss</td>
      </tr>
      <tr>
        <td><b>sacks_solo</b></td>
        <td>solo sacks; solo tackles of the Quarterback for a loss</td>
      </tr>
      <tr>
        <td><b>sacks_asst</b></td>
        <td>sack assists; assisted tackles of the Quarterback for a loss</td>
      </tr>
      <tr>
        <td><b>sacks_yards</b></td>
        <td>total number of yards lost by the offense for solo and assisted sacks</td>
      </tr>
      <tr>
        <td><b>int</b></td>
        <td>interceptions</td>
      </tr>
      <tr>
        <td><b>int_yards</b></td>
        <td>yards gained following an interception</td>
      </tr>
      <tr>
        <td><b>int_td</b></td>
        <td>touchdowns scored off of an interception</td>
      </tr>
      <tr>
        <td><b>fum</b></td>
        <td>fumbles recovered</td>
      </tr>
      <tr>
        <td><b>fum_yards</b></td>
        <td>yards gained following a fumble recovery</td>
      </tr>
      <tr>
        <td><b>fum_td</b></td>
        <td>touchdowns scored off of fumble recoveries</td>
      </tr>
      <tr>
        <td><b>ffum</b></td>
        <td>forced fumbles; number of fumbles caused by the player</td>
      </tr>
      <tr>
        <td><b>safety</b></td>
        <td>tackles of an opponent in the opponent's end zone</td>
      </tr>
      <tr>
        <td><b>punt_ret</b></td>
        <td>punts returned</td>
      </tr>
      <tr>
        <td><b>punt_ret_yards</b></td>
        <td>yards gained on punt returns</td>
      </tr>
      <tr>
        <td><b>punt_ret_td</b></td>
        <td>touchdowns scored on punt returns</td>
      </tr>
      <tr>
        <td><b>kick_ret</b></td>
        <td>kick offs returned</td>
      </tr>
      <tr>
        <td><b>kick_ret_yards</b></td>
        <td>yards gained on kick off returns</td>
      </tr>
      <tr>
        <td><b>kick_ret_td</b></td>
        <td>touchdowns scored on kick off returns</td>
      </tr>
      <tr>
        <td><b>total_points</b></td>
        <td>total number of points scored by the player from safeties, touchdowns from interception returns, fumble returns, punt returns and kick off returns</td>
      </tr>
    </tbody>
  </table>

</div>

#### Stats on Stats: NCAA Stats

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>#</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>Players</b></td>
        <td>10,536</td>
        <td>DBs played at an NCAA Division I school between 2005 and 2017</td>
      </tr>
      <tr>
        <td><b>In Study</b></td>
        <td>5,723</td>
        <td>DBs who had matching biographical info and were included in this project</td>
      </tr>
    </tbody>
  </table>
</div>

#### Issues:
__1. Two ids for a single player__: After concatenating this data into a single DataFrame, I discovered that, like the cfbstats.com data above, some of the players' NCAA careers had been mistakenly split into two or more entries; one id had been assigned to a year or two of a player's career and another id to the remainder of the same player's career, where there should have been a one-to-one relationship between id and player + player stats.  I went through, team-by-team and player-by-player and corrected this, not only by using pandas' `.duplicated()` function, but also by looking through each name for slight variations on the name.  
</br>
__2. Class years overlapped__: Many players had mislabeled classes.  For example, a player being listed as a junior for two years' worth of stats.  I created a class with some methods to correct Issue #1 above and to deduplicate duplicated classes in an efficient and accurate fashion in this [Notebook](sausage_factory/ncaa_deduper.ipynb).



<br>
<br>

### __NFL Scouting Combine__ - Importing from .csv
---

The NFL Scouting Combine is the ideal source for data on players' raw athleticism.  However, as noted elsewhere, it is an invitation-only event, and only a very small proportion of players are invited each year.  I downloaded Combine data from a [single source](https://www.pro-football-reference.com/draft/2018-combine.htm) in .csv format.  I performed some manipulations to split and move data and rename some columns, but had no issues with this data set.

#### Cleaning:
I took some simple steps to:
- drop unnecessary columns
- break a single column containing draft information into `draft_round`, `draft_pick` and `draft_team` columns
- change the format of the height column from 5-11 to 71 (feet-inches to inches)

[Combine Cleaner Notebook](sausage_factory/Clean_Combine.ipynb)


#### Combine Features

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>player</b></td>
        <td>player's first and last name - will be used to merge the DataFrame from different sources.</td>
      </tr>
      <tr>
        <td><b>draft_team</b></td>
        <td>team that drafted the player out of college</td>
      </tr>
      <tr>
        <td><b>draft_round</b></td>
        <td>round in which player was selected</td>
      </tr>
      <tr>
        <td><b>draft_pick</b></td>
        <td>player was the nth pick overall in that year's draft</td>
      </tr>
      <tr>
        <td><b>draft_position</b></td>
        <td>position the player was drafted to play</td>
      </tr>
      <tr>
        <td><b>college</b></td>
        <td>college the player attended - used with the player's name to merge with other data sources</td>
      </tr>
      <tr>
        <td><b>draft_height</b></td>
        <td>player's official height at the time of the draft</td>
      </tr>
      <tr>
        <td><b>draft_weight</b></td>
        <td>player's official weight at the time of the draft</td>
      </tr>
      <tr>
        <td><b>forty</b></td>
        <td>40-yard dash time</td>
      </tr>
      <tr>
        <td><b>vertical</b></td>
        <td>vertical leap</td>
      </tr>
      <tr>
        <td><b>bench_reps</b></td>
        <td>number of repetitions of bench press at 225 pounds</td>
      </tr>
      <tr>
        <td><b>broad_jump</b></td>
        <td>broad jump distance</td>
      </tr>
      <tr>
        <td><b>three_cone</b></td>
        <td>time in 3 cone drill, use to evaluate agility, quickness and fluidity of movement</td>
      </tr>
      <tr>
        <td><b>shuttle</b></td>
        <td>time in the short shuttle (also called the 20-yard shuttle), used to evaluate the quickness and change-of-direction ability</td>
      </tr>
    </tbody>
  </table>

</div>

#### Stats on Stats: Combine

<div>
  <table>
    <thead>
      <tr>
        <th>Stat</th>
        <th>#</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>Participants</b></td>
        <td>632</td>
        <td>DBs participated in the Combine from 2005 - 2015</td>
      </tr>
      <tr>
        <td><b>Drafted</b></td>
        <td>436</td>
        <td>DBs who participated in the Combine were eventually drafted by an NFL team</td>
      </tr>
      <tr>
        <td><b>In study</b></td>
        <td>287</td>
        <td>DBs who atteneded the Combine were included in this project</td>
      </tr>
    </tbody>
  </table>
</div>

<br>
<br>

### __NFL__ - Accessing Data Using an API
---
I really only needed the number of seasons a player played in the NFL, but I collected a few additional biographical statistics while I was accessing the API.  I chose the sportradar.com API because it contained the information I needed along with some additional biographical information and it appeared to be complete and accurate.  

Their database is behind a paywall, but I was able to sign up for a trial.  When it became apparent that I needed more data than the trial terms would allow, I contacted them and they graciously raised my limits to allow me to download the information I needed.

As with the NCAA statistics, I first queried the teams for their rosters to get ids for each player, then used those ids to access players individually.

#### Cleaning:
I took some simple steps to:
- break birthdate out into three separate columns, 
- concatenate the player's preferred name with the last name, 
- create a separate column containing the state the player's high school was in,
- count the number of seasons played in the NFL,
- take the `.min()` of seasons played to get value for the 'rookie' column,
- take the `.max()` of seasons played to get value for the 'last_season' column,
- added a column to insert the exact spelling of the player's name used in the NCAA DataFrame to merge the two

[NFL Data Cleaner Notebook](sausage_factory/Clean_NFL.ipynb)

#### Issues:
There were no issues, once the limits of the trial were lifted.

#### NFL Features

<div>
  <table>
    <thead>
      <tr>
        <th>Statistic</th>
        <th>Description</th>
      </tr>
    </thead>
      <tr>
        <td><b>player</b></td>
        <td>player's first and last name, used to merge with other data sets</td>
      </tr>
      <tr>
        <td><b>last_name</b></td>
        <td>last name</td>
      </tr>
      <tr>
        <td><b>first_name</b></td>
        <td>First Name</td>
      </tr>
      <tr>
        <td><b>pref_name</b></td>
        <td>preferred name, where a player's first name is not his preferred name, used to compose 'player'</td>
      </tr>
      <tr>
        <td><b>height</b></td>
        <td>official NFL listed height</td>
      </tr>
      <tr>
        <td><b>weight</b></td>
        <td>official NFL listed weight</td>
      </tr>
      <tr>
        <td><b>birth_date</b></td>
        <td>date of birth</td>
      </tr>
      <tr>
        <td><b>birth_year</b></td>
        <td>year of birth</td>
      </tr>
      <tr>
        <td><b>birth_month</b></td>
        <td>month of birth</td>
      </tr>
      <tr>
        <td><b>birth_day</b></td>
        <td>day of birth</td>
      </tr>
      <tr>
        <td><b>hs_state</b></td>
        <td>state in which player attended high school</td>
      </tr>
      <tr>
        <td><b>high_school</b></td>
        <td>name of high school attended</td>
      </tr>
      <tr>
        <td><b>college</b></td>
        <td>college attended - used to merge with other DataFrames</td>
      </tr>
      <tr>
        <td><b>season_count</b></td>
        <td>count of seasons played in the NFL</td>
      </tr>
      <tr>
        <td><b>rookie</b></td>
        <td>first year in the NFL</td>
      </tr>
      <tr>
        <td><b>last_season</b></td>
        <td>year last played in the NFL</td>
      </tr>
      <tr>
        <td><b>seasons</b></td>
        <td>list of all seasons played in the NFL</td>
      </tr>
      <tr>
        <td><b>ncaa_name</b></td>
        <td>player name in combined NCAA bios and stats DataFrame, used to merge with that DataFrame</td>
      </tr>
    </tbody>
  </table>

</div>

#### Stats on Stats: NFL

<div>
  <table>
    <thead>
      <tr>
        <th>Stat</th>
        <th>#</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><b>Players</b></td>
        <td>1,308</td>
        <td>DBs played in the NFL from 2006 - 2017</td>
      </tr>
      <tr>
        <td><b>In study</b></td>
        <td>495</td>
        <td>DBs who played in the NFL and were included in this project</td>
      </tr>
      <tr>
        <td><b>NFL Success</b></td>
        <td>287</td>
        <td>DBs who played at least 3 years and are in the final DataFrame</td>
      </tr>
    </tbody>
  </table>
</div>

### __Merging__


![data](images/data_path.png)

## Exploratory Data Analysis


